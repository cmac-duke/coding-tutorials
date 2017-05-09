---
date: 2017-01-16
title: Hello World
categories:
  - Supercollider
description: "Add a credit card to your account"
type: Document
---

SuperCollider is a platform for audio synthesis and algorithmic composition, used by musicians, artists, and researchers working with sound. It is free and open source software available for Windows, macOS, and Linux. 
[You can find it here](http://supercollider.github.io)

## Boot The Server

Our first step is to boot the server. Supercollider works on a client/server paradime. The interface we're working with sends messages to the server, which traslates the messages and creates the sound out of our speakers. 

```
s.boot;
```


hit Shift + Return to execute the line of code. You should see it highlighted in orange

## Hello World

Supercollider works with Unit Generators or UGens. A UGen is just a command that tells the server what kind of waveform, or sound, to make. 

```
{SinOsc.ar(400)}.play;
```

Lets break down this line of code. Here we are using the SinWave UGen 'SinOsc'. SinOsc takes arguments, and we are setting the first argument, frequency, to 400hz. We are wrapping the SinOsc UGen in curly braces, and telling Supercollider to play the sound with '.play'.


## Manipulation

It is not hard to expand hello world. Lets take that original sound and multiply it by another sine wave. This one, we are using the control rate '.kr' and setting the frequency to 30hz. Using control rate tells supercollider that the sine wave that we're creating isn't meant to make a sound. We're just using it to manipulate the first sinewave that we're created

```
{SinOsc.ar(400)*SinOsc.kr(30)}.play
```

## Nested UGen

A powerful feature of UGens is that they can be nested inside one another. This allows us to combine an infinite amount of UGens to create very complex sounds. Here we are simply taking a sinewave, and nesting it inside of a panning UGen, 'Pan2'. Pan2 will output the sound to two speakers.

```
{Pan2.ar(SinOsc.ar(400))}.play;
```

We can continue to add UGens to the mix. Here we're adding some reverb with 'FreeVerb', some mouse interaction with MouseX and MouseY and and we've changed one of our sinewaves to a low-frequency triangle wave.

```
{Pan2.ar(FreeVerb.ar(SinOsc.ar(MouseX.kr(200, 800))*LFTri.kr(MouseY.kr(1, 20)), 0.8, 0.4), 0)}.play;
```


