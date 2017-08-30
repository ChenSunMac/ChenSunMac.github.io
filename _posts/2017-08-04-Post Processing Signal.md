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

In order to the distance between the transducer to the pipe wall, we can make use of <a href="https://en.wikipedia.org/wiki/Cross-correlation">**cross correlation**</a> between emitted pulse and recorded transducer signal.

### Cross-correlation

Cross-correlation is a measure of similarity of two series as a function of the displacement of one relative to the other. This is also known as a sliding dot product or sliding inner-product.

![Cross-Correlation](https://upload.wikimedia.org/wikipedia/commons/thumb/2/21/Comparison_convolution_correlation.svg/600px-Comparison_convolution_correlation.svg.png "Cross-Correlation")

As an example, consider two real valued functions ***f*** and ***g*** differing only by an unknown shift along the x-axis. One can use the cross-correlation to find how much ***g***  must be shifted along the x-axis to make it identical to ***f*** . The formula essentially slides the ***g***  function along the x-axis, calculating the integral of their product at each position. When the functions match, the value of ***f*g***  is maximized.

In MATLAB, we have the handy tool by 
```matlab
            % Calculate the cross correlation between received pulse and
            % transmitted pulse. 
            [r, lags] = xcorr(receivedSignal(startIndex:end), obj.txPulse);
            % Find the index where the cross correlation is at its
            % maximum            
            [valueMax, indexMax] = max(r);
```

From the calculated index, we can then use simple algebra to calculate the distance
```matlab
            % distance to pipe wall from transducer d = (wave velocity * sample points)/( 2*Fs )
distance = single(obj.signal_delay) * obj.config.V_WATER / ( 2 * obj.config.SAMPLE_RATE );
            
```

## Noise and PSD

The power spectrum $S_{{xx}}(f)$ of a time series $x(t)$ describes the distribution of power into frequency components composing that signal. The statistical average of a certain signal or sort of signal (including noise) as analyzed in terms of its frequency content, is called its spectrum.

PSD can be calculated by FFT or DFT. Before doing so, one need to specify the noise part of the signal(instead of the valid emit and received sound signal). From the Caliper Algorithm, we can obtain the first and second Reflection Index. Then, the signal before the first reflection is just pure background noise. We can then segment that part for noise calculation.

There are many packages doing PSD, simple.

## Thickness Algorithm

Finally, we come to the thickness. The detection eventually want to find the abnormal part of the pipe thickness, whether there is a dent or some hole. The thickness calculation can be evaluated in time domain, as well as frequency one.




