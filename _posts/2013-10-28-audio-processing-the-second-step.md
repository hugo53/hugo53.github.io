---
layout: post
title: "Audio Processing - The second step"
description: "In the previous post at [here](http://hugo53.github.io/ios%20development/2013/09/27/audio-processing-in-ios---an-initial-step/), I had shown you some basic terminologies in audio processing and several libraries to handle with audio in iOS. It is just a kick off! This post, I will provide in more details principle architect of audio (in iOS, certainly!) and give you some great documents to gain more audio knowledge."
category: iOS Development 
tags: [audio]
---
{% include JB/setup %}
In the previous post at [here](http://hugo53.github.io/ios%20development/2013/09/27/audio-processing-in-ios---an-initial-step/), I had shown you some basic terminologies in audio processing and several libraries to handle with audio in iOS. It is just a kick off! This post, I will provide in more details principle architect of audio (in iOS, certainly!) and give you some great documents to gain more audio knowledge.

I had an opportunity to work with audio, thus I must read documentation carefully. However, there are plenty of notions that I have not known ever, even listening about them has never been occured. What I should mention are _audio session_, _audio unit_, _audio category_, and so on. One time again, we must familiar with these definitions. Here we go!

## Basic Definitions
#### Audio Session


#### Audio Unit


#### Audio Category


## Audio Framework in iOS
If you search _audio framework_ in _Documentation and API_ in Xcode, you will get many audio frameworks which includes _Audio Toolbox_, _CoreAudio_, _OpenAL_, _AVFoundation_. So, you may be confused to determine what framework should be used. Generally, _Audio Toolbox_ is application-level service, _Core Audio_ is low-level API which used to communicate with hardware, _OpenAL_ is an opensource audio library and also an application-level API, _AVFoundation_ is an Objective-C library to play audio file.

