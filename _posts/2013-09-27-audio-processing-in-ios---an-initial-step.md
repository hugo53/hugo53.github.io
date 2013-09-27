---
layout: post
title: "Audio Processing in iOS - an initial step"
description: "Performance with audio is always a challenge for any developer by reason of its very low level. By chance, I got some opportunities to do with audio included playing, recording and pre (and/or) post processing. This post tends to cover as much as possible basic knowledge in Audio aspect for two reasons: reminding myself and sharing with other enthusiastic guys."
category: iOS Development 
tags: [audio]
---
{% include JB/setup %}
> Performance with audio is always a challenge for any developer by reason of its very low level. By chance, I got some opportunities to do with audio included playing, recording and pre (and/or) post processing. This post tends to cover as much as possible basic knowledge in Audio aspect for two reasons: reminding myself and sharing with other enthusiastic guys.

In the first words, I want to warn that I am working on iOS platform at the momment. Therefore, all of knowledge appear here is of iOS or related to iOS by default. 

## Basic
Before dig into details, we should know a litle bit about audio format, audio codec and what format is preferred in iOS. 

### Audio Format and Codec
General speaking, there are two elements of an audio file. It's weird, huh? Don't worry! First, codec and second, file format. Codec is the way audio file is encoded (when record) or decoded (when playback). We must know the codec of audio file to perform some processing task such as uncompressing to play or mixing to make effects. The latter one, format is the way audio file is organized. 

### Bit rate

### Preferred Audio Formats (in iOS, certainly)



## Frameworks


## Libraries


## Conclusion



## References
- [Audio Tutorial for iOS: File and Data Formats](http://www.raywenderlich.com/204/audio-tutorial-for-ios-file-and-data-formats) by RAYWENDERLICH
- Apple's Docummentation

## Open sources which are mentioned above
- [The Amazing Audio Engine](http://theamazingaudioengine.com/doc/)
- [Open AL](http://kstenerud.github.io/ObjectAL-for-iPhone/documentation/index.html)
- [Sound Engine - an OpenAL Example](https://github.com/alexrestrepo/SoundEngine)