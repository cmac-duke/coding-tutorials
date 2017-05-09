
I was able to get my hands on a leap motion. Leap motion controllers are motion sensors for your hands. Much like kinects, they measure the distance your hand is from the sensor. The data can be fed into Supercollider or Processing via OSC to manipulate sound and visuals. Below is some code that I've developed that takes input from the Leap Motion, converts it to OSC, and manipulates plays some basic instruments.

```
(
s.boot;
~masterAmp=1;

SynthDef(\sine1, {|freqA=400, freqB=800, amp=1, pan=0, out=0, mod=3, room=0.4|
    var src, env;
    env=EnvGen.kr(Env.perc(0.01, 10), doneAction:2);
    src=PMOsc.ar(freqA, freqB, mod, 0, 0.1);
    src=FreeVerb.ar(src, 0.3, room);
    src=Pan2.ar(src, pan);
    Out.ar(out, src*env*amp);
}).add;

SynthDef(\woodnoise, {|out=0, amp=0.3, pan=0, sustain=0.5, t_trig=1, freq=100, rq=0.06|
    var env, signal;
    var rho, theta, b1, b2;
    b1 = 2.0 * 0.97576 * cos(0.161447);
    b2 = 0.9757.squared.neg;
    signal = SOS.ar(K2A.ar(t_trig), 1.0, 0.0, 0.0, b1, b2);
    signal = Decay2.ar(signal, 0.4, 0.8, signal);
    signal = Limiter.ar(Resonz.ar(signal, freq, rq*0.5), 0.9);
    env = EnvGen.kr(Env.perc(0.00001, sustain, amp), doneAction:2);
    Out.ar(out, Pan2.ar(signal, pan)*env);
}).add;

SynthDef(\noiseklank,{|freq=400, amp=1, pan=0, out=8, mod=2, attack=1, decay=1, room=0.8|
    var src, env;
    env=EnvGen.kr(Env.perc(0.01, 1), doneAction:2);
    src=BPF.ar(WhiteNoise.ar(0.5)*0.3, 700, 0.3);
    src=DynKlank.ar(`[[freq, freq, freq+100],[1, 1]], src, 0.6);
    src=Pan2.ar(src, pan);
    src=FreeVerb.ar(src, 0.3, room);
    Out.ar(out, src*env*amp*~masterAmp);
}).add;

d =Buffer.read(s,"/Users/mbk5020a/Downloads/sms_alert_bamboo.caf (AAC audio).wav");
SynthDef(\fragments, { |out=8, bufnum, start, time = 1, stretch = 1, amp = 1, attack = 0.01, decay = 6, pan=0|
    var src, env;
    env=EnvGen.kr(Env.linen(attack, time, decay), doneAction: 2);
    src = PlayBuf.ar(1, bufnum, rate: stretch.reciprocal, startPos: start, doneAction:2);
    src = PitchShift.ar(src, pitchRatio: stretch);
    src=Pan2.ar(src, pan);
    Out.ar(out, src*env*amp)
}).add;

SynthDef(\zip, {|start=1 end=300, time=0.4 amp=1, pan=0, out=0, room=0.8, attack=1, release=1|
    var src, env;
    env=EnvGen.kr(Env([0, 1, 0], [attack, release]), doneAction:2);
    src=Impulse.ar(XLine.kr(start, end, time));
    src=SinOsc.ar(src, src);
    src=Pan2.ar(src, pan);
    Out.ar(out, src*env*amp*~masterAmp);
}).add;
)

(
~pattern1=Pbind(
    \instrument, \sine1,
    \freqA, Pseq([200, 300, 500, 400, 200], inf),
    \freqB, Pdefn(\freq1),
    \mod, Pdefn(\mod1),
    \pan, Pdefn(\pan1),
    \delta, 12
);
~pattern2=Pbind(
    \instrument, \noiseklank,
    \freq, Pdefn(\freq2),
    \attack, 0.01,
    \release, 0.1,
    \room, 0.3,
    \mul, 1,
    \pan, Pdefn(\pan2),
    \amp, Pdefn(\amp2),
    \dur, 0.3,
    \delta,  Pseq((0.4!6) ++ (0.2!4),inf)
);
~pattern3=Pbind(
    \instrument, \woodnoise,
    \freq,  Pdefn(\freq3),
    \pan, Pdefn(\pan3),
    \delta, Pdefn(\delta3),
);
~pattern4=Pbind(
    \instrument, \fragments,
    \bufnum, d,
    \start, 0.1,
    \delta, Pexprand(0.2, 10, inf),
    \time, Pdefn(\time4),
    \stretch, Pdefn(\stretch4),
    \amp, 0.4,
    \attack, 0.1,
    \decay, Pdefn(\decay4),
);
~pattern5=Pbind(
    \instrument, \zip,
    \start, Pdefn(\start5),
    \end, Pdefn(\end5),
    \time, Pdefn(\time5),
    \pan, 0,
    \delta, 2
);
)

(
~range=1.8;
//sine1
OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\freq1, msg[1].linlin(-2, 2, 100, 800,).postln);}
    {Pdefn(\freq1, 0)}
}, '1/fader1');

OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\mod1, msg[1].linlin(-2, 2, 1, 30));}
    {Pdefn(\mod1, 0)}
}, '1/Xfader1');

OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\pan1, msg[1].linlin(-2, 2, -1, 1));}
    {Pdefn(\pan1, 0)}
}, '1/Zfader1');

//sine2
OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\freq2, msg[1].linlin(-2, 2, 100, 800));}
    {Pdefn(\freq2, 0)}
}, '1/fader2');

OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\amp2, msg[1].linlin(-2, 2, 0.1, 0.3));}
    {Pdefn(\amp2, 0)}
}, '1/Xfader2');

OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\pan2, msg[1].linlin(-2, 2, -1, 1));}
    {Pdefn(\pan2, 0)}
}, '1/Zfader2');

//sine3
OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\freq3, msg[1].linlin(-2, 2, 100, 800));}
    {Pdefn(\freq3, 0)}
}, '1/fader3');

OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\delta3, msg[1].linlin(-2, 2, 0.01, 0.5));}
    {Pdefn(\delta3, 0)}
}, '1/Xfader3');

OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\pan3, msg[1].linlin(-2, 2, -1, 1));}
    {Pdefn(\pan3, 0)}
}, '1/Zfader3');

//sine4
OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\time4, msg[1].linlin(-2, 2, 1, 10));}
    {Pdefn(\time4, 0)}
}, '1/fader4');

OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\stretch4, msg[1].linlin(-2, 2, 1, 10));}
    {Pdefn(\stretch4, 0)}
}, '1/Xfader4');

OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\decay4, msg[1].linlin(-2, 2, 0.1, 5));}
    {Pdefn(\decay4, 0)}
}, '1/Zfader1');

//sine5
OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\start5, msg[1].linlin(-2, 2, 10, 300));}
    {Pdefn(\freq5, 0)}
}, '1/fader5');

OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\end5, msg[1].linlin(-2, 2, 300, 800));}
    {Pdefn(\end5, 0)}
}, '1/Xfader5');

OSCFunc({|msg|
    if(msg[1]<=~range){
        Pdefn(\time5, msg[1].linlin(-2, 2, 0.2, 2));}
    {Pdefn(\time5, 0)}
}, '1/Zfader5');
)
(
Ppar([
    ~pattern1,
    ~pattern2,
    ~pattern3,
    ~pattern4,
    ~pattern5
], inf).play;
)

~pattern1.play; 
```