---
layout: post
title: Signal Post Processing 
date: 2017-08-04
tag: PureTech, Signal Processing
comments: true
---

### Post Processing

We can obtain binary files of all those analog signals sampled from all kinds of platforms (manned, robot based...). These signals could be sound or EM signals. 

## Import Binary Data

At first glance, a proper binary file should contain ***Header*** which include the configuration of the test/run, for example the *sample rate*, *bandwidth*, *pulse Pattern* and *pipe profile*

    The data import is done by a **ImportHandler** Class:
     ``` matlab
        im = ImportHandler;
        im.readFolder('The\Folder\Path')
    ```  

Now the basic configuration should be imported into the object *im* and we can generate the impulse by simply run

``` matlab
s = SignalGenerator(lower(im.header.pulsePattern), ...
                    im.header.sampleRate, ...
                    im.header.pulseLength/im.header.sampleRate, ...
                    im.header.fLow, ...
                    im.header.fHigh)
```


The rest of the binary file should be our data, e.g. Signal shots of transducer at certain time points. We can always store them into arrays for further computation. 

## Caliper Algorithm 












