---
layout: page
title: "Hackjekyll"
description: ""
---
{% include JB/setup %}
Jekyll is a up-to-date blog framework in the today's world. It is increasing as the tool come in handy for many blogger, especially developers who desire sharing their experience. This page concentrates on showing basic techniques to construct a blog for all.

### Twitter Feed on Jekyll Blog
Unfortunately, at the time when this page is established, there is no offical support from both Jekyll and JekyllBoostrap for adding a twitter feed box into this kind of blog. However, it is exist several works of some enthusiasm tech guys who also need to putting their tweets on Jekyll's posts. In my site, I ulitize [this source](http://tweet.seaofclouds.com/) for my need.

There is the way I apply for my blog.

##### Download libraries
- Download jquery 1.9.3v
- Download tweets library from [here](https://github.com/seaofclouds/tweet/zipball/master).


##### Add custom source code
- Add the below code at the end of ``` _layouts/post.html```.


```
<script src="{{ BASE_PATH }}/assets/themes/jquery-1.9.1.min.js"> </script>
```
```
<script src="{{ BASE_PATH }}/assets/tweetfeed/tweet/jquery.tweet.js" type="text/javascript"></script>
```

```
<
script type="text/javascript">
jQuery(function($){
    $(".tweet").tweet({
          join_text: "auto",
          username: "mrhoangnm",
          avatar_size: 48,
          count: 5,
          auto_join_text_default: " we said, ",
          auto_join_text_ed: " we ",
          auto_join_text_ing: " we were ",
          auto_join_text_reply: " we replied ",
          auto_join_text_url: " we were checking out ",
          loading_text: "loading tweets..."
        });
      });
<
/script>
```

```
hihi
haha
hoho
```

and

```
<div class="tweet"></div>
```

- Add the below code at the ``` head ``` of ```_includes/themes/twitter/default.html ``` (I use twitter theme for this site, you can change this path if another theme is applied). 

```
<link href="{{ BASE_PATH }}/assets/tweetfeed/tweet/jquery.tweet.css" media="all" rel="stylesheet" type="text/css"/>
```


OK, well done. You already have tweet feed in your site now!
