---
layout: post
title: "Cocoapods - Easy but not easy"
description: "Someone told that Cocoapods is easy to use at both aspects: integrate open third-party into own project and publish open library to community. However, for newbie or even a experience developer, at the first time, it is not easy as a pie. This post is for them!"
category: ios 
tags: []
---
{% include JB/setup %}

Someone told that Cocoapods is easy to use at both aspects: integrate open third-party into own project and publish open library to community. However, for newbie or even a experience developer, at the first time, it is not easy as a pie. This post is for them. Here we go!


## Preparing git repository
### Pushing source code
You should push source code to remote repository first.

{% highlight bash linenos %}
git add .
git commit -m "Initial commit"
git push origin -u master
{% endhighlight %}

### Tagging
A tag should be created for the source code because we will need tag version to config podspec file later.

{% highlight bash linenos %}
git tag -a 1.0.0 -m "Make v1.0.0"
git push origin --tags
{% endhighlight %}

## Create pod
Change directory to project which you want to create a pod. In my case, I will go to ```HUChartDemo```:

{% highlight bash %}
cd HUChartDemo
{% endhighlight %}

Next, create a podspec file:

{% highlight bash %}
pod spec create HUChart    // May need sudo
{% endhighlight %}

Now, it's time to config podspec file. See [Pod specification](https://github.com/CocoaPods/CocoaPods/wiki/A-pod-specification) to get further information. Just do like the example, it is OK. Note that ```s.version``` should be set by a tag version like:

```
s.version = "1.0.0"
```

{% highlight bash %}
open -e HUChart.podspec
or
vi HUChart.podspec
{% endhighlight %}

Thereafter, do a validation for podspec:
{% highlight bash %}
pod spec lint HUChart.podspec    // May need sudo
{% endhighlight %}

At the end of terminal screen, ```pod-name.podspec passes validation``` appears let you know that your pod is OK to publish. 

![alt text](http://hugo53.github.io/images/huchart/hucharPodValidation.png "pod validation top")

Otherwise, check again all fields in podspec file to ensure that all is accurate. 

Next, go to do the last task, send a pull request (PR).

##### A note for s.source\_files attribute in podspec file
It is to locate source files you want to publish. Thus, be sure that you provide the exact path for ```s.source_files``` attribute. A legal path is a path from the root directory which contains ```.git``` folder to destination file. For example, the below code show a path for ```HUChartDemo``` project (but its repository name is [HUChart](https://github.com/hugo53/HUChart), don't be confused here, we need project's root directory, not repository name). 

{% highlight bash  %}
s.source_files  = 'HUChart', 'HUChartDemo/HUChart/*.{h,m}'
{% endhighlight %}

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

- Do git add this file and commit with a clear comment. After that, push it to forked Specs repository.

{% highlight bash linenos %}
git add Specs/HUChart/1.0.0/*.podspec
git commit -m "Added HUChart library to draw a semi circle chart."
git push origin -u master
{% endhighlight %}

- Lastly, go to [Specs pull page](https://github.com/CocoaPods/Specs/pulls) and press onto ```New pull request``` and fill all require fields to create a pull request. Thereafter, wait for Specs' developers merge this request. Once they approve the request and merge it, we can use our lib as a Pod by Podfile.

It is so easy, isn't it? But for me, the first time I created a pod, I wonder how pod work and how many necessary steps to make a pod successfully. Now, you have an own pod. Congratulations! Take time to share it as far as possible! 

<script data-gittip-username="mrhoangnm"
        data-gittip-widget="button"
        src="//gttp.co/v1.js">
        </script>
