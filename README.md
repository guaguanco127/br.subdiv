# Max/MSP Patches and Abstractions
## br.subdiv.1.0



By Brian Riordan  
guaguanco127 [at] gmail [dot] com  
brianriordanmusic [at] gmail [dot] com  
[https://www.brianriordanmusic.com/](https://www.brianriordanmusic.com/)

Repository for br.subdiv.1.0, with all related files, can be found here: [https://github.com/guaguanco127/br.subdiv](https://github.com/guaguanco127/br.subdiv)  
Additional programs can be found here: [https://github.com/guaguanco127/br.max](https://github.com/guaguanco127/br.max)

These files were created with Max 9.

## Links

[About](#About)  
[Max/MSP Abstractions](https://github.com/guaguanco127/br.subdiv/tree/main/MaxMSP%20Abstraction) To use as an abstraction within Max/MSP (installation, inlet, outlets and the example patch are in its README)  

## <a name="About"></a>About

Max's [subdiv~] splits a phasor into equal steps. When you change the number of steps while it runs, the new grid takes over at once and the ramp jumps to its place in it. To [what~] or [edge~] that jump looks like a new step, so you get a count (a bang, a note, a click) at the moment you change the division, not on a beat.

br.subdiv does the same thing as [subdiv~], with the same inlet and the same three outlets, but it only counts real steps. The new division still takes effect at once; the next count is the new grid's next real step.

Example: phasor at 0.6, division 3 changed to 4. [subdiv~] jumps from step 1 to step 2 and counts it right there. br.subdiv lets the jump happen silently and counts step 3 at 0.75.

**How:** inside is [subdiv~] with @syncupdate 0, followed by a small gen~ filter in place of [what~]. A real step starts where the ramp begins to rise from the bottom. A division change jumps to the middle of the ramp (0.8 down to 0.4, for example), so it never counts. The filter only looks at the ramp itself, so pattern and prob still work: a silent step (prob 0) is not counted, and the step after it is. It allows two samples of margin: a change within two samples of a real step counts once, as that step.

**Why not @syncupdate 2?** [subdiv~] can also wait for the input phasor to wrap before changing (@syncupdate 2). That is click-free too, but a change can wait a whole cycle of the input phasor. br.subdiv changes right away.
