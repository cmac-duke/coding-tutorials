
---
date: 2017-01-15
title: Supercollider + Arduino
video_id: _iH8f5alzWA
description: Have non-technical people update Jekyll sites
categories:
  - Arduino
resources:
  - name: Source code
    link: https://github.com/CloudCannon/creative-jekyll-theme/
  - name: CloudCannon
    link: https://cloudcannon.com
type: Video
set: getting-started
set_order: 1
---

Getting Supercollider to talk to arduino it pretty straighfoward. Supercollider and Arudino could pass data back and forth. Maybe your interested in a sensor that creates a sonification, or a motor that turns along with some supercollider pattern. There are two ways to go about connecting the two plaforms. The first would be to connect Arudino to supercollider via OSC. Another way to talk to arduino from supercollider is to program the arduino directly from supercollider. We can do this through the use of the Arduino Quark.

What is a Quark? Quarks are packages, or libraries, much like in other programming languages. You can add Quarks to your supercollider sketch by execuing


```
Quarks.gui;
```

Now that we have the quark installed, lets plug in our Arduino via the USB port.

If we run `SerialPort.listDevices` we should see the adruino listed in the console.

```
SerialPort.listDevices;
```

From there we can set the usb port much like we do in the Arduino IDE. Here, we'll set the port to usbmodem1a21 at 9600 and well post a message to make sure that everything is working correctly.

```
$w means you are writing on the port
$a that the port is analog (well, simulated via PWM)

SerialPort.devices;

p=ArduinoSMS("/dev/tty.usbmodem1a21", 9600);

p.action = { | ... msg | msg.postln; };
```


Next lets create a basic synth to talk to the arudino.

```
(
SynthDef(\arduino,{|freq=300, grain=0.1, room=40, pan=0, out=0, amp=0.2, attack=0.2, sustain=0.2 release=0.2, gate=0.2|
    var source, env;
    env=EnvGen.ar(Env.adsr(attack, release, sustain), doneAction:2);
    source=LFTri.ar(freq);
    source=GVerb.ar(source, room, 7);
    source=Pan2.ar(source, pan);
    Out.ar(out, source*amp);
}).add;
)

Synth(\arduino);
```


```
(
p.action={
    |... msg|

    msg[0].normalize(100, 700);
    msg[1].postln;
    Synth(\arduino, [\freq, [msg[0], \gate, [msg[0]].linexp(0, 50, 200, 1100)]);
};
)

```



```
(
p.action= {|...msg|
    m=[msg[0], msg[1]];
    case
    {m[0]=="a"} {
        "a: ".post;
        msg[1].postln;
        a= msg[1].linlin(0, 1024, 0.0, 1.0);
        "msg a scaled: ".post;
        a.postln;
    };
};
)
```

```
SerialPort.closeAll;
```


```
(
fork {
    8.do {
        p.send($w, $d, 13, 1);
        1.25.wait;
        p.send($w, $d, 13, 0);
        0.25.wait;
    }
}
)

p.send($w, $a, 0, 128);

p.close;
```