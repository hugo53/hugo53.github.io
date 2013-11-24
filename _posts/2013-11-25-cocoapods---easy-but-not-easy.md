---
layout: post
title: "Cocoapods - Easy but not easy"
description: "Someone told that Cocoapods is easy to use at both aspect: integrate open third-party and publish open library for community. However, for newbie or even a experience developer, at the first time, it is not easy as a pie. This post is for them!"
category: ios 
tags: []
---
{% include JB/setup %}

Someone told that Cocoapods is easy to use at both aspect: integrate open third-party and publish open library for community. However, for newbie or even a experience developer, at the first time, it is not easy as a pie. This post is for them!




## Podspec Attributes

#### s.source\_files
It is to locate source files you want to publish. Thus, be sure that you provide the exact path for ```s.source_files``` attribute. A legal path is a path from the root directory which contains ```.git``` folder to destination file. For example, the below code show a path for ```HUChartDemo``` project (but its repository name is [HUChart](https://github.com/hugo53/HUChart), don't be confused here, we need project's root directory, not repository name). 

```
s.source_files  = 'HUChart', 'HUChartDemo/HUChart/*.{h,m}'
```

## Preparing git repository
### Tagging

{% highlight bash linenos %}
git tag -a 1.0.0 -m "Make v1.0.0"
git push origin --tags
{% endhighlight %}

## Validation podspec



## Send a pull request to [Specs](https://github.com/CocoaPods/Specs)
After that, we must send a pull request to ```Specs``` to make the pod is visible. To do that, follow these steps:

- Fork [Specs](https://github.com/CocoaPods/Specs) and clone it.

{% highlight bash %}
git clone https://github.com/CocoaPods/Specs.git
{% endhighlight %}

- Change directory (cd) to ```Specs``` folder.

{% highlight bash %}
cd Specs
{% endhighlight %}

- Assigns the original repo to a remote called "upstream".

{% highlight bash %}
git remote add upstream git@github.com:CocoaPods/Specs.git
{% endhighlight %}

- Create our own Pod folder, ex. 

{% highlight bash %}
mkdir -p HUChart/1.0.0
{% endhighlight %}

Note that ```1.0.0``` is version number which we set for ```s.version = "1.0.0"``` in podspec file.

- Copy .podspec file which we have already made some minutes ago to the version folder. In this case, version folder is ```Specs/HUChart/1.0.0```

{% highlight bash %}
cp path/to/.podspec-file Specs/HUChart/1.0.0 
{% endhighlight %}

- Do git add this file and commit with a clear comment like ```Added HUChart library to draw a semi circle chart.```. After that, push it to forked Specs repository.

{% highlight bash linenos %}
git add Specs/HUChart/1.0.0/*.podspec
git commit -m "Added HUChart library to draw a semi circle chart."
git push origin -u master
{% endhighlight %}

- Go to [Specs pull page](https://github.com/CocoaPods/Specs/pulls) and press onto ```New pull request``` and fill all require fields to create a pull request. Thereafter, wait for Specs' developers merge this request. Once they approve the request and merge it, we can use our lib as a Pod by Podfile.



