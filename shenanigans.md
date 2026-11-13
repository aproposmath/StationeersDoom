# Shenanigans

Quirks of Stationeers the save relies on. Nothing here is a mod: it is what the game does with a hand-edited save
and chips that stay inside the IC10 rules at run time.

**Code size is unlimited.** The editor caps a chip at 128 lines, but a chip loaded from a save runs
code of any length, only 128 instructions per tick. Lookup tables live in code as jump tables with hundreds of
thousands of lines (floor, walls, the writer chains), and the enemy animations are loaded from code into stack
memory every tick (`clr`, then one `put` per word). The whole save holds about 10 million lines of IC10.

**Chips run in load order, each its whole tick at once.** The order of the housings in the save is the execution
order within a tick, and network channel writes are seen by every later chip in the same tick. The renderer is a
pipeline of about a thousand chips that depends on it.

**Many devices in one cell.** A 0.5 m cell keeps only its last-loaded device, but every device still finds its cable on
load. Diodes are packed far denser than the game would let you build. This works as long as no cable is edited in game.

**A housing without a data cable reaches everything.**
A two-port chip housing with no connection on the data port can read and write any device in the world through its
pins d0-d5, if they are set accordingly in the save file. This is very important for renderer performance (smaller
networks, avoid expensive `sd` instructions on large networks), and allows cross-network communication (for level and
sprite storage).

**Tablets are the keyboard.** Four Advanced Tablets in the toolbelt each hold a chip that reads the helmet, jetpack
and both hands. Key presses that toggle gear (helmet lamp, flashlight, jetpack stabilizer) flip bits the tablets
write to their `Volume`, and Logic Transmitters connect them to the renderer. Each tablet runs at a different point
of the tick, right before one of the frames, so a shot fired mid-tick shows in the very next in-between frame.
Movement is the suit itself: its pressure and temperature settings (bound to the arrow keys) are read and reset
every tick through another Logic Transmitter.

**Sequencers as instruments.** The music and the shotgun are step sequencers driven by chips; writing a
sequencer's `Time` restarts it at an offset, which turns a loop into a one-shot sound effect.
There are 6 tracks from the Doom soundtrack encoded in one chip with more than 40,000 lines of code.

**Multiple frames in one tick.** Chips run on a worker thread while the main thread keeps rendering; a diode `Color`
write is queued and applied by the main thread in its next frame (at 60 FPS, for instance), so pixels written later
in the tick appear later. A stop watch's `Time` is updated continuously during a tick by another thread, so chips can
tell where in the tick they are. Frame A is written first, then *busy-wait* chips spend their instructions on batch
reads (`lb` over thousands of devices, a few milliseconds each) until the next frame's target time, then frame B is
written over it, the same for frames C and D. A scheduler chip spaces the frames evenly over the tick and drops the
ones that would not fit, from the measured times of the last ticks. So the renderer keeps one thread busy for most of
every tick (about 500 ms). Unfortunately, this is not working as well as expected, so maybe the visual glitches will be 
fixed in a future update.

