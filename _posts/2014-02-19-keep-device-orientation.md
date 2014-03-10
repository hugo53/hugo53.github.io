---
layout: post
title: "Keep device orientation"
description: "Sometimes we need to keep only one orientation for device regardless how it is rotated. Because of framework API change and the vary of UI Container elements, it is not easy to implement this task in several minutes. This post aims to cover two of these complex cases: app using UINavigationController and app using UITabbarController, and focus on iOS 5.x and iOS 6.x and later."
category: ios
tags: []
---
{% include JB/setup %}
Sometimes we need to keep only one orientation for device regardless how it is rotated. Because of framework API change and the vary of UI Container elements, it is not easy to implement this task in several minutes. This post aims to cover two of these complex cases: app using UINavigationController and app using UITabbarController, and focus on iOS 5.x and iOS 6.x \& later.

## App using UINavigationController

The root view controller is must inherited by UINavigationController and implement 4 functions: 1 for supporting iOS 5.x and 3 for supporting iOS 6.x.



