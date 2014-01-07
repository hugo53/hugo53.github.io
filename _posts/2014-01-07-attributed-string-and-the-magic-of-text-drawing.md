---
layout: post
title: "Attributed String and the magic of text drawing"
description: "From IOS 6, there is a magical tool to make beautiful and colorful words for any purpose such as representing lovely text to girlfriend/boyfriend or to relations. It is called **Attributed String**. In IOS 7, we can freely to reveal text in our apps with many new supports."
category: ios
tags: [attributed string, text drawing]
---
{% include JB/setup %}
From IOS 6, there is a magical tool to make beautiful and colorful words for any purpose such as representing lovely text to girlfriend/boyfriend or to relations. It is called **Attributed String**. In IOS 7, we can freely to reveal text in our apps with many new supports.

In almost built-in UI Components, _attributed_ is beside _text_ property. If we set property _attributedText_, the label/UITextView/... will show _attributedText_ instead of _text_ property. Sometimes, we tend to set _text_ but do some extra code to convert _text_ to _attributedText_ to make the string be eye-catching.







## Open source
#### Customizing Attributed Text for Label
- [TTTAttributedLabel](https://github.com/mattt/TTTAttributedLabel) by [Mattt](http://mattt.me). A drop-in replacement for UILabel that supports attributes, data detectors, links, and more. We can use attributed string in label with many extend features included link detetors (underline and making color for URI text) or manually making URI string. Its author is also one of creators of [AFNetworking](https://github.com/AFNetworking/AFNetworking) - a powerful network library. 

- [OHAttributedLabel](https://github.com/AliSoftware/OHAttributedLabel) by [AliSoftware](https://github.com/AliSoftware). This library allows us to make label text with extended attributed string. Especially, it can customize text for hashtag (\#), mention (\@). Certainly, it also built link detector for emphasizing URI. Besides, there are many applications have used this library, including SoundClound, Wunderlist. See more [here](https://github.com/AliSoftware/OHAttributedLabel/wiki/They-use-this-class).

- [Nimbus Attributed Label](http://docs.nimbuskit.info/NimbusAttributedLabel.html) in [Nimbus](https://github.com/jverkoey/nimbus). Using NSAttributedString to render rich text labels with links using CoreText.

#### Attributed Text Parser
- [Slash](https://github.com/chrisdevereux/Slash) by [Chris Devereux](https://github.com/chrisdevereux). Slash is a simple, extensible markup language for styling NSAttributedStrings. The language is similar in appearance to HTML, however the meaning of each tag is user-defined. It can be refered as a mini-parser which can extract structured text and show text as its expected tag.


## Source may need when work with AttributedString
- [List all available fonts in iOS, from v.3.0 to v.7.0, for iPhone and Ipad](http://iosfonts.com/)




