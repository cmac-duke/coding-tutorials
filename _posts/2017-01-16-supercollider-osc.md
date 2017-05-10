---
date: 2017-01-16
title: Working with OSC
categories:
  - Supercollider
description: Step-by-step instructions on creating a Jekyll plugin
type: Document
---
OSC or Open Sound Controll is a protocol for passing information from on platform or piece of code to another. It is typically used for performance, or installation and was created as an alternative to MIDI. OSC libraries can be found in most create coding languages. If we have a piece of Processing code and we want it to pass information to a piece of Supercollider or Pure Data code, we can do so with OSC. 

Implementing OSC in Supercollider is relatively straight forward, however them examples and documentation are pretty lacking.

Lets take a look at OSC protocol in Supercollider with OSCFunc. 

First, well boot the server and set an address. This will be the address port that the OSC messages will be transmitted and recieved.

Here we're setting the net address to "127.0.0.1", which just means our local computer. Next, we're setting the port to 57120. This port can be any 5 diget number, but its best to keep the ports to ones that we know will be free. 57120 is usually safe.

```
(
s.boot;
s= Server(\myServer, NetAddr("127.0.0.1", 57120));
)
```


Next, we'll create a basic synth as we have in other tutorials. This synth will recieve the OSC messages, so its important to leave it flexible enough to change the values that you want to change. Don't hard code values!

```
~mysynth=SynthDef(\linea, {
    |
    freq=200, pan=0, amp=0.1, room=40, rev=5, gate=1
    |
    var env, src, mod;
    env=EnvGen.kr(Env.asr(0.01, 1, 0.1), gate);
    mod=SinOsc.kr(0.01).range(0.01, 0.04);
    src=SinOsc.ar(freq)*mod;
    src=GVerb.ar(src, room, rev, 0.6, 0.3);
    src=Pan2.ar(src, pan);
    Out.ar(0, src*env*amp)
}).add;
```

Lastly, well create as OSCFunc and set the frequency to the message that we would like to recieve. 

```
OSCFunc.new({|msg|~mysynth.set(\freq, [msg].postln);});

~mysynth.play;
```


Now that we have the basic setup down, we can turn our attention to what to connect it to. Let try processing. If you are unfimiliar with Processing, please find some basic setup tutorials [here]

Processing is a framework for creating visual and sound pieces, interfacing with hardware, and much more.

```
import oscP5.*;
import netP5.*;
  
OscP5 oscP5;
NetAddress myRemoteLocation;

void setup() {
  size(400,400);
  frameRate(25);
  myRemoteLocation = new NetAddress("127.0.0.1",57120);
}


void draw() {
  background(0);  
}

void mousePressed() {

  OscMessage myMessage = new OscMessage("/test");
  
  myMessage.add(123);

  oscP5.send(myMessage, myRemoteLocation); 
}
```
