---
date: 2017-01-16
title: Working with Patterns
categories:
  - Supercollider
description: "Update your email address"
type: Document
---
One of the most powerful features you'll run across in supercollider are patterns. Patterns are just what they sound like- a way of organizing the output of your synths into musical pattens without having to run through tasks. Patterns are powerful in supercollider for a couple of reasons: 

1) you can sync them to the internal clock as well as to eachother
2) you can pass data between patterns 
3) they provide an extra layer of composition with seqences, randomness etc
4) they can interface with OSC

While patterns are a building block of compositions in supercollider, I struggled to understand them initially, as they work slightly differently then tasks or routines. A basic pattern in supercollider starts with a Pbind

```
Pbind(
  \key, \value,
  \key, \value,
  etc..
)
```

Pbinds are a wrapper around you pattern, binding the key value pairs within it. A key-value pair works much like key-values in other languages. The key represents a variable, typically a variable in the synth that youre using. The value represents the value for that variable, which can be a static number, input, or another patter, as well see below.

If I create a basic synth

```
(
SynthDef(\mybasicsynth, {|freq=300, amp=1, pan=0, out=0|
  var src, env;
  env= EnvGen.kr(Env.perc(0.1), doneAction:2);
  src= SinOsc.ar(freq);
  src= Pan2.ar(src, pan);
  Out.ar(out, src*env*amp)
}).add
) 
```

and I want to include it in to a pattern that I am creating, I can set the key '\instrument' to the value '\mybasicsynth' to have the patter load the synth created above.

```
(
Pbind(
  \instrument, \mybasicsynth,
).play
)
```

I can then set the frequency of 'mybasicsynht' to the 'freq' key in the pattern.

```
(
Pbind(
  \instrument, \mybasicsynth,
  \freq, 600
).play
)
```

...Great, so we have a synthdef, and we can play it with a pattern, but that's not all that interesting. What if we want to change the freqency dynamically?
Well, we can use a Pseq. Pseq is a sequence within our Pbind. Like Pbind 'binds' the key value pairs, Pseq 'sequences' the values, where the array is the sequence and the following value is how many times it is played.

If i want to play the mybasicsynth in a pattern, where the sequence is for the [300, 400, 500, 200], and i want it to play through 5 time, I would write my pattern like this:

```
(
Pbind(
  \instrument, \mybasicsynth,
  \freq, Pseq([300, 400, 500, 200], 5)
).play
)
```

Likewise, if i want to change the pan of my instrument, all I have to do is add the 'pan' key value pair

```
(
Pbind(
  \instrument, \mybasicsynth,
  \freq, Pseq([300, 400, 500, 200], 5)
  \pan, Pseq([-1, 0, 1], 5)
).play
)
```

And so on...There are many different types of patterns that I can use. All of them begin with P as in Pattern, followed by some reference to what it does. For instance Prand() plays the array of valuse in random order.

```
(
Pbind(
  \instrument, \mybasicsynth,
  \freq, Prand([300, 400, 500, 200], 5)
  \pan, Pseq([-1, 0, 1], 5)
).play
)
```