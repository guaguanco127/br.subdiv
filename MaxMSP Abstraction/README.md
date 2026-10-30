# Max/MSP Abstractions:
## br.subdiv.1.0



By Brian Riordan  
guaguanco127 [at] gmail [dot] com  
brianriordanmusic [at] gmail [dot] com  
[https://www.brianriordanmusic.com/](https://www.brianriordanmusic.com/)

Repository for br.subdiv.1.0, with all related files, can be found here: [https://github.com/guaguanco127/br.subdiv](https://github.com/guaguanco127/br.subdiv)  
Additional programs can be found here: [https://github.com/guaguanco127/br.max](https://github.com/guaguanco127/br.max)

These files were created with Max 9.

## Table of Contents

[About](#About)  
[What is an abstraction?](#Abstraction)  
[How To Install](#Install)  
[How To Use](#Use)  
[Example Patch](#Example)  



## <a name="About"></a>About

Max's [subdiv~] splits a phasor into equal steps. When you change the number of steps while it runs, the new grid takes over at once and the ramp jumps to its place in it. To [what~] or [edge~] that jump looks like a new step, so you get a count (a bang, a note, a click) at the moment you change the division, not on a beat.

br.subdiv does the same thing as [subdiv~], with the same inlet and the same three outlets, but it only counts real steps. The new division still takes effect at once; the next count is the new grid's next real step.

Example: phasor at 0.6, division 3 changed to 4. [subdiv~] jumps from step 1 to step 2 and counts it right there. br.subdiv lets the jump happen silently and counts step 3 at 0.75.

**How:** inside is [subdiv~] with @syncupdate 0, followed by a small gen~ filter in place of [what~]. A real step starts where the ramp begins to rise from the bottom. A division change jumps to the middle of the ramp (0.8 down to 0.4, for example), so it never counts. The filter only looks at the ramp itself, so pattern and prob still work: a silent step (prob 0) is not counted, and the step after it is. It allows two samples of margin: a change within two samples of a real step counts once, as that step.

**Why not @syncupdate 2?** [subdiv~] can also wait for the input phasor to wrap before changing (@syncupdate 2). That is click-free too, but a change can wait a whole cycle of the input phasor. br.subdiv changes right away.

## <a name="Abstraction"></a>What is an Abstraction?

An abstraction is a subpatcher that is saved as an external file, and can be used just like a standard Max object. As long as your abstraction can be found in the Max file path, you can type its name into a new object box and it will be loaded directly into your patch.

By saving your logic in an abstraction, you can create modules that can be used in future work with little or no additional programming. This allows you to parlay your Max knowledge into more efficient work in the future, and will help you create programming systems that are modular and easier to maintain.

## <a name="Install"></a>How To Install

1. Make sure you have Max 9 installed in your computer. And, make sure you are using a Max patch that is inside of a folder.

2. Copy br.subdiv.1.0.maxpat into the same folder as the Max patch you are using. It is a single file: the gen~ filter is stored inside it.

3. Create an object with its name and, if you like, the division as an argument and any [subdiv~] attributes, for example br.subdiv.1.0 4 @pattern 2 1 1 (do not include brackets). Use it wherever you would use [subdiv~].

## <a name="Use"></a>How To Use

Patch it like [subdiv~]: a phasor~ into the inlet, and any [subdiv~] message into the same inlet. Hover over the inlet or an outlet in Max to see what it does.

| Inlet | What it takes | Default |
|---|---|---|
| 1 | Phasor (signal). Int = division, 1 to 16384. Every [subdiv~] message: pattern, prob, lockprob, silentmode, syncupdate | division 4 (or argument 1), syncupdate 0 |

| Outlet | Name | Type |
|---|---|---|
| 1 | Ramp: the subdivided phasor, 0 to 1 per step. Same as [subdiv~] | Signal |
| 2 | Step number. Same as [subdiv~] | Signal |
| 3 | Step number at each real step start, never on a division change | Int |

**Attributes:** the box passes any [subdiv~] attribute on when the patch opens, for example br.subdiv.1.0 4 @prob 1 0.5 1 0. Setting @syncupdate to 1 or 2 brings back [subdiv~]'s own waiting behaviour.

**Outlet 3 timing:** the number is read from the signal at the end of the audio vector in which the step starts, so it is as accurate as Max's scheduler allows. For sample-accurate timing use the signal outlets.

## <a name="Example"></a>Example Patch

Open _br.subdiv.example.1.0.maxpat (keep it in the same folder as br.subdiv.1.0.maxpat) and turn on audio.

- **3, 4, 7:** change the division while it runs. The scope jumps to the new grid at once; the step number and the button only change on real steps.
- **prob 1 0.5 1 0** and **prob none:** a chance per step (0 = a silent step, not counted), and back to normal.
- **pattern 2 1 1** and **pattern 1 1 1 1:** uneven step lengths for division 4, and back to even steps.
