---
layout: post
title: "Audio Processing in iOS - an initial step"
description: "Performing with audio is always a challenge for any developer by reason of its very low level. By chance, I got some opportunities to do with audio included playing, recording and pre (and/or) post processing. This post tends to cover as much as possible basic knowledge in Audio aspect for two reasons: reminding myself and sharing with other enthusiastic guys."
category: iOS Development 
tags: [audio]
---
{% include JB/setup %}
> Performance with audio is always a challenge for any developer by reason of its very low level. By chance, I got some opportunities to do with audio included playing, recording and pre (and/or) post processing. This post tends to cover as much as possible basic knowledge in Audio aspect for two reasons: reminding myself and sharing with other enthusiastic guys.

In the first words, I want to warn that I am working on iOS platform at the moment. Therefore, all of knowledge appear here is of iOS or related to iOS by default. 

## Basic
Before dig into details, we should know a litle bit about audio format, audio codec and what format is preferred in iOS. 

#### Audio Format and Codec
General speaking, there are two elements of an audio file. It's weird, huh? Don't worry! First, codec and second, file format. Codec is the way audio file is encoded (when record) or decoded (when playback). We must know the codec of audio file to perform some processing tasks such as uncompressing to play or mixing to make effects. The latter one, format is the way audio file is organized. 

#### Bit rate

#### Preferred Audio Formats (in iOS, certainly)
Just recall that there are two types of audio quality: uncompressed audio and compressed audio. In short words, uncompressed audio is always better than the remain kind but it is true for professional listener not for all people. Because of two these existing types, Apple also defines two preferred formats respectively. 

For uncompressed (highest quality) audio, use *16-bit*, *little endian*, *linear PCM* audio data packaged in a *CAF* file. You can convert an audio file to this format in Mac OS X using the afconvert command-line tool, as shown here:

> /usr/bin/afconvert -f caff -d LEI16 {INPUT} {OUTPUT}

For compressed audio when playing one sound at a time, and when you do not need to play audio simultaneously with the iPod application, use the *AAC* format packaged in a *CAF* or *m4a* file.

For less memory usage when you need to play multiple sounds simultaneously, use *IMA4 (IMA/ADPCM) compression*. This reduces file size but entails minimal CPU impact during decompression. As with linear PCM data, package IMA4 data in a *CAF* file.

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