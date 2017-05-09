---
date: 2017-01-16
title: Creating Instruments
categories:
  - Supercollider
description: "Deploy your Jekyll site to Azure"
type: Document
---
SynthDefs in supercollider are an essential part of organizing the sounds you create. You can think of them as instruments, or 'synths', that can be reused, and communication with other sections of your compositions to create patterns of sound. When you're first starting out writing supercollider code, it is tempting to continue to pile on UGens into a long string. To this point, we have been creating sounds with a single line of code like this one:

```
{Pan2.ar(FreeVerb.ar(SinOsc.ar(400)*LFTri.kr(20), 0.3, 0.8), 0)}.play;
```

Quickly your code will get out of hand. Moreover, you'll have to set your pile of UGens to a variable to be able to use them throughout your code. Lastly, the sound created above is not at all flexible, since the vaibles for each UGen are hard coded into the sound. SynthDefs are essental to both organization and structure, and create flexable instruments that can then be used throughout your code. I will walk you through each apect of the SynthDef, and how it solves each of the problems listed above.


```
SynthDef(\mysound, { |freq=400, mod=20, mix= 0.3, room=0.8, pan=0, amp=1, out=0|
    var src;
    src = SinOsc.ar(freq);
    src =  src * LFTri.kr(mod);
    src = FreeVerb.ar(src, mix, room);
    src = Pan2.ar(src, pan);
    Out.ar(out, src*amp);
});
```

You might think: but the writing this sound like a SynthDef is so much more typing!!

This is true, but the work that you put in to crafting your sythdefs on the front end makes up for hours of headache and confusion on the backend.

First, we'll look at the SynthDefs structure:

As you can see, the synthdef makes sense structurally. Each UGen has its own line, and each line is folded into the line above it. src=FreeVerb.ar() is fit into src Pan2.ar below it. 

Next, the SynthDefs flexibility. As stated, the first instument that we created has hard coded numbers. This means that if we want to change the 'freq', mod', 'pan' or the 'room', well have to copy the entire sounds, and rewrite the numbers accordingly.

With a SynthDef, instead of copying over the entire string of UGens to change, say, the 'freq', we can just say

```
Synth(\mysound, [\freq, 600]);
```
