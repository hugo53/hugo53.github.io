---
layout: post
title: "Loading large image"
description: ""
category: translation 
tags: []
---
{% include JB/setup %}

Share experience with load large pdf file in BaseStone: use the same PhotoScroller


Apple recommends not use image larger than 1024*1024 pixels (need refer source)

You need to handle the image loading using UIImage which doesn't actually load the image into memory and then create a bitmap context at the size of the resulting image that you want (so this will be the amount of memory used). Then you need to iterate a number of times drawing tiles from the original image (this is where parts of the image data are loaded into memory) using CGImageCreateWithImageInRect into the destination context using CGContextDrawImage.


https://developer.apple.com/library/content/samplecode/LargeImageDownsizing/Introduction/Intro.html#//apple_ref/doc/uid/DTS40011173-Intro-DontLinkElementID_2



Large images don't fit in memory. So loading them into memory to then resize them doesn't work.

To work with very large images you have to tile them. Lots of solutions out there already for example see if this can solve your problem:

https://github.com/dhoerl/PhotoScrollerNetwork

I implemented my own custom solution but that was specific to our environment where we had an image tiler running server side already & I could just request specific tiles of large images (madea server, it's really cool)

The reason tiling works is that basically you only ever keep the visible pixels in memory, and there isn't that many of those. All tiles not currently visible are factored out to the disk cache, or flash memory cache as it were.

	

The GPUs in the iPhone 3G and iPod Touch 1G/2G only have room for 1k by 1k textures, which seems to also be used for 2D image rendering. Anything larger needs to be tiled to fit the hardware graphics renderer.

The 3GS and newer have a different GPU which can support larger textures, and thus images.

One method to scale image: (https://stackoverflow.com/a/3679919)
UIImage *image = // ...;
if (image.size.width * image.size.height > MAX_PIXELS) {
    // calculate the scaling factor that will reduce the image size to MAX_PIXELS
    float actualHeight = image.size.height;
    float actualWidth = image.size.width;
    float scale = sqrt(image.size.width * image.size.height / MAX_PIXELS);

    // resize the image
    CGRect rect = CGRectMake(0.0, 0.0, floorf(actualWidth / scale), floorf(actualHeight / scale));
    UIGraphicsBeginImageContext(rect.size);
    [image drawInRect:rect];
    UIImage *imageToDraw = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();

    // imageToDraw is a scaled version of image that preserves the aspect ratio
}



Reference
https://developer.apple.com/library/content/samplecode/LargeImageDownsizing/Introduction/Intro.html#//apple_ref/doc/uid/DTS40011173-Intro-DontLinkElementID_2

https://www.built.io/blog/improving-image-compression-what-we-ve-learned-from-whatsapp
https://gist.github.com/akshay1188/4749253#file-whatsapp-image-compression

session 104, it's called "Designing Apps with Scroll Views".