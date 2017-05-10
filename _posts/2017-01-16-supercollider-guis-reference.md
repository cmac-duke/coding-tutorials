---
date: 2017-01-16
title: Supercollider GUI
categories:
  - Supercollider
description: "Deploy your Jekyll site to Azure"
type: Document
---

The supercollider GUI system is complex. Typically, if I am going to interface with supercollider, I opt to use another programming language like Pure Data and pass OSC messages to Supercollider. However, here is a quick refernece for those of you who might want to develop a GUI in Supercollider itself. This GUI interfaces with a WiiMote, and reads OSC. I chose this example as a robust GUI with button, knobs an sliders. If you are simply looking for a GUI for a synth, I might check out allgui from the Quarks repository.

```
(
~spec =[0.1, 1, \exponential].asSpec;
~msg;
w = Window.new("BODY", bounds:Rect(100,100,400,900)).layout_(
VLayout(
    Button(w).states_([["Up", Color.rand], ["Down", Color.rand]])
    .action_(OSCFunc.newMatching({|msg, time, addr, recvPort| [msg].postln;}, '/wii/2/button/Down', recvPort:57120, argTemplate: {if(msg==1, {z.play}, {z.stop})})),
        HLayout(
Button(w).states_([["Left", Color.rand], ["Left", Color.rand]])
            .action_(OSCFunc.newMatching({|msg, time, addr, recvPort| [msg].postln;}, '/wii/2/button/Left', recvPort:57120, argTemplate: {if(msg==1, {z.play}, {z.stop})})),
            Button(w).states_([["Right", Color.rand], ["Right", Color.rand]])
            .action_(OSCFunc.newMatching({|msg, time, addr, recvPort| [msg].postln;}, '/wii/2/button/Right', recvPort:57120,argTemplate: {if(msg==1, {z.play}, {z.stop})})),
                 ),
        Button(w).states_([["Down", Color.rand], ["Down", Color.rand]])
        .action_(OSCFunc.newMatching({|msg, time, addr, recvPort| [msg].postln;}, '/wii/2/button/Down', recvPort:57120, argTemplate: {if(msg==1, {z.play}, {z.stop})})),

         Button(w).states_([["A", Color.rand], ["A", Color.rand]])
        .action_(OSCFunc.newMatching({|msg, time, addr, recvPort| [msg].postln;}, '/wii/2/button/A', recvPort:57120, argTemplate: {if(msg==1, {z.play}, {z.stop})})),

         Button(w).states_([["B", Color.rand], ["B", Color.rand]])
        .action_(OSCFunc.newMatching({|msg, time, addr, recvPort| [msg].postln;},  '/wii/2/button/A', recvPort:57120, argTemplate: {if(msg==1, {z.play}, {z.stop})})),
        ),

            HLayout(
            QSlider2D(w),
            QSlider2D(w),
                    ),
            HLayout(
            QSlider2D(w),
            QSlider2D(w),
                ),
            HLayout(
            QSlider2D(w),
            QSlider2D(w),
                ),
            HLayout(
            QSlider2D(w),
            QSlider2D(w),
                ),
            )
).front;
)
```
