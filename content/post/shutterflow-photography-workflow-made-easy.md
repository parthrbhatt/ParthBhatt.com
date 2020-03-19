+++
title = "Shutterflow Photography Workflow Made Easy"
date = "2011-09-24T19:01:11+11:00"
draft = false
categories = ["projects"]
tags = ["Java", "Java Swing", "JAI"]
image = "../../media/blog_shutterflow.jpg"
+++

![Shutterflow](../../media/blog_shutterflow.jpg)

So, it started with the thought of publishing some of the photos, I managed to click, online. The task would involve going thru the usual process of: Copy photos form my camera to my computer. Then watermark them. Then, possibly, add a frame around the image and finally publish to my favorite photo sharing service.

Since, I have been uploading some of my photos to flickr already, I needed a good way to batch edit my photos and push them to flickr. I looked around and found quite a few tools. Generally, the reason I did not end up using one of the available tools was either they were expensive or they did not seem to fit my exact requirements. So I thought I would try my hand at writing one of my own.

Initially, FWIW, the idea of going the [gimp](http://www.gimp.org/) plugin way did seem interesting. Another idea was to build an app around [ImageMagick](http://www.imagemagick.org/). Given that gimp and ImageMagick are both fantastic at what they do, either of these ideas would have taken away the burden of implementing anything that had to do with image editing.

However, what I was looking to create was kind of a photography workflow solution. Not quite an image editing application. As far as image editing goes, I would be fine with having a minimal set of filters, that may not be very hard to implement. What was more important was to have a feature set suited for photographers.

And hence I kicked off [shutterflow](http://www.parthbhatt.com/shutterflow/) !! The idea behind shutterflow is to build a tool from ground up that was targeted to make the tasks, a photographer performs on a regular basis, extremely easy. Shutterflow would contain a minimal set of image filters/transforms. It would allow you to save all filters/transforms that you apply to your images, so that you can re-apply them again to a different image. Moreover, it will let you define, what one would call, a workflow. A workflow would be set of tasks that will be performed in sequence on an image. A task inside a workflow could be to apply a watermark or brighten an image or crop an image based upon specified constraints, or even upload to flickr (or facebook or picasaweb/Google Photos for that matter). To batch edit photos, this workflow can then be applied to, say, a directory of images.

Of course, it’ll be a while before shutterflow becomes capable of doing all of this. As of today it can barely watermark images and upload to flickr. Batch editing still needs to be brought in. And so does support for more photo sharing services. I haven’t even started to think about raw image formats, etc.

So what is shutterflow built using? [Java Advanced Imaging (JAI)](http://java.sun.com/javase/technologies/desktop/media/jai/) seemed pretty cool. Plus, I knew a little bit of Java Swing. So it wasn’t hard to zero in upon Java. Project hosting on [sourceforge.net](http://www.sourceforge.net/), [git](http://git-scm.com/) for SCM and [apache ant](http://ant.apache.org/) for the build system.

Hopefully, in a matter of time, shutterflow will help scratch my itch as well as that of a few others ! Meanwhile, checkout the [shutterflow website](http://www.parthbhatt.com/shutterflow/) and keep watching the download section.
