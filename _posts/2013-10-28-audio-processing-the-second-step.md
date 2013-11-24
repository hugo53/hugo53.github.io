---
layout: post
title: "Audio Processing - The second step"
description: "In the previous post at Audio Processing in iOS - an initial step, I had shown you some basic terminologies in audio processing and several libraries to handle with audio in iOS. It is just a kick off! This post, I will provide in more details principle architect of audio (in iOS, certainly!) and give you some great documents to gain more audio knowledge."
category: iOS Development 
tags: [audio]
---
{% include JB/setup %}
In the previous post at [here](http://hugo53.github.io/ios%20development/2013/09/27/audio-processing-in-ios---an-initial-step/), I had shown you some basic terminologies in audio processing and several libraries to handle with audio in iOS. It is just a kick off! This post, I will provide in more details principle architect of audio (in iOS, certainly!) and give you some great documents to gain more audio knowledge.

I had an opportunity to work with audio, thus I must read documentation carefully. However, there are plenty of notions that I have not known ever, even listening about them has never occured. What I should mention are _audio session_, _audio unit_, _audio category_, and so on. One time again, we must familiar with these definitions. Here we go!

## Basic Definitions
#### Audio Session

#### Audio Category

#### Audio Queue

#### Audio Unit
Audio Unit is defined as software-plugin service which carries some low-level tasks such as mixing, filtering, splitting or digital processing services as general term. If you want to do futher than just playback audio file, you must know audio unit, and use Audio Unit framework to complete your desire.

## Audio Services in IOS
Audio Service is a service which performs several specific tasks. Thus, a framework may contain more than one services. The image below shows three levels dividing services: Low-level, Mid-level and High-level (we usually do with two last levels). 

![alt text](http://hugo53.github.io/images/core_audio_layers_2x.png "leading")

## Audio Framework in iOS
If you search _audio framework_ in _Documentation and API_ in Xcode, you will get many audio frameworks which includes _Audio Toolbox_, _CoreAudio_, _OpenAL_, _AVFoundation_. So, you may be confused to determine what framework should be used. Generally, _Audio Toolbox_ is an application-level service, _Core Audio_ is a low-level API which used to communicate with hardware, _OpenAL_ is an opensource audio library and also an application-level API (and plus, usually used in Game), _AVFoundation_ is an Objective-C library to play audio file.

According to Apple iOS documentation:
{% highlight text %}
The Audio Toolbox framework (AudioToolbox.framework) provides interfaces for the mid- and high-level services in Core Audio. In iOS, this framework includes Audio Session Services, the interface for managing your application’s audio behavior in the context of a device that functions as a mobile phone and iPod.

The Audio Unit framework (AudioUnit.framework) lets applications work with audio plug-ins, including audio units and codecs.

The AV Foundation framework (AVFoundation.framework), available in iOS, provides the AVAudioPlayer class, a streamlined and simple Objective-C interface for audio playback.

The Core Audio framework (CoreAudio.framework) supplies data types used across Core Audio as well as interfaces for the low-level services.

The Core Audio Kit framework (CoreAudioKit.framework) provides a small API for creating user interfaces for audio units. This framework is not available in iOS.

The Core MIDI framework (CoreMIDI.framework) lets applications work with MIDI data and configure MIDI networks. This framework is not available in iOS.

The Core MIDI Server framework (CoreMIDIServer.framework) lets MIDI drivers communicate with the OS X MIDI server. This framework is not available in iOS.

The OpenAL framework (OpenAL.framework) provides the interfaces to work with OpenAL, an open source, positional audio technology.
{% endhighlight %}

In summary, we should focus on three frameworks (priority descending): AVFoundation, AudioToolbox, CoreAudio for making an awesome iOS app.

