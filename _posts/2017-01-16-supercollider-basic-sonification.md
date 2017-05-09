---
date: 2017-01-16
title: Basic Sonifications
categories:
  - Supercollider
description: Deploy your Jekyll site on Cloudflare
type: Document
---

## What is sonification:

Sonification is:

    `The auditory equivalent of scientific visualization – is gradually becoming more and more established, and the Internet plays an important role for this, since it allows us to share rendered sound examples and even real-time rendered interactive sonifications. For this reason, these websites contain lots of sonification examples to illustrate my sonification techniques and their uses`

That is, sonification is the process of taking data and instead of rendering it visually in a data visuliazation, we render it sonically in a musical composition. Data sonification is great for emersive experiences and bringing another layer of meaning or complexity to an installation or exhibit. 

Lets look at how to create a basic data sonification in supercollider. 

## Loading the data:

The first step of any sonification is loading the data into the platform. Here we are using the CSVFileReader class to read a CSV file from the desktop into supercollider 


```
CSVFileReader("/path/to/you/file.myfile.csv")
```

CSV file reader will parse you CSV for you to and load it into supercollider. However, if we were to print the results, we can see that there are multiple columns, much like we have in a traditional CSV. 

Next well take the results of CSVFileReader and flatten them. You can do this by setting CSVFile reader to a variable and calling '.flop' on the variable. 

```
~mycsv.flop;
```

This will transform the columns into the string of numbers that we want.

We now have the data set up correctly in our interface. We can now reference the data in each column by setting it to a variable. 

```
////ballora totalice (area + volume)
~trigrates=~data.at(3).normalize(7, 25);
~attacks=~data.at(3).normalize(0.02, 0.2);
~spreads=~data.at(3).normalize(0.25, 1);
~damps=~data.at(0).normalize(0.25, 1);
~roomsizes=~data.at(0).normalize(0.5, 0.7);
~freqscales=~data.at(0).reciprocal.normalize(0.5, 1.25);
~noiselevs=~data.at(0).normalize(0, 0.0025);
~levels=~data.at(0).normalize(0.5, 2);
```


Well probably also want to take the range of each column and map it to a new range. If we have data that ranges from 1-10000, and we want to use that range for the frequncy of a sing wave, its pretty clear that our sonification would sound particularly bad as the data reached anywhere above 10000.

We can use the '.normalize' to create new ranges from the data. Here, we're taking the first column of our CSV and bormalizing it to between 100 and 900. Later, we'll use these values for the frequency of our instument. Remember, the values of the columns start with 0, hence the '(0).normalize'

````
~rain = ~data.at(0).normalize(100, 9000)
````

The next step is to create our SynthDef. We'll make it flexable enough so that the frequency of the data that we brought in from our CSV can be mapped to the frequency of the synthdef.

```
(
SynthDef(\mysonificiaiton, {|freq=400, amp=1, pan=0, out=0
    var src, env;
    src=SinOsc.ar(freq)
    })
)

```


Notice the type of envelope that we're using. An ASR or "Attack Sustain Release" envelope allows us to play a continual tone throughout the composition.


Our last step is to use a task. This task cycles through each datapoint in our CSV table, and plays our synth at each step. This way we get at continual sound as our sonification progresses.


``` 

```


Well set our task to a variable to able to reference it in the end.
