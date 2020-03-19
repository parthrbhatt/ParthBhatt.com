---
title: "PyShortURL – A Url Shortening Lib in Python"
date: "2012-04-29T19:22:03+11:00"
draft: false
categories:
- projects
tags:
- Python
- git
#thumbnailImagePosition: left
#thumbnailImage: ../../media/blog_pyshorturl.jpg
---

![pyShortURL](../../media/blog_pyshorturl.jpg)

Its been sometime since I got on to twitter. And every time I happen to tweet a link, I watch my incredibly long urls magically shrink to a few chars. A few days ago, I happened to do a quick google search on url shortening that revealed some interesting info and I soon found myself playing with it. Here’s what came out of it.
URL shorteners are used to transform huge, ugly looking, difficult to remember (and manage) urls into short urls. For instance take this url: http://maps.google.com/maps?q=-27.103602,-109.290791. When shortened using goo.gl, this turns into http://goo.gl/FDlHE. Any HTTP request to this short url gets redirected to the original url. It goes without saying that such small urls are extremely easy to handle.

It is very common to want to share a link on the internet today. Oftentimes, the link we want to share is on the domain that is not controlled by us. This makes it difficult to track the number of visits to the url you shared. In this case, you can create a short url for the original url, share the short url and let the url shortening service maintain the statistics for you. This is just one of the scenarios where url shortening comes handy. While url shortening does have other uses, it is absolutely necessary (read: inevitable) for micro-blogging sites like twitter which impose strict constraints over the max number of characters that may be used in a post.

If you are an application developer trying to build url shortening capability into your application, you will need to pick a url shortening service (trust me, there are more than you might imagine) and implement the APIs provided by that service. Alternatively, you could use [pyShortUrl](https://github.com/parthrbhatt/pyShortUrl).

[pyShortUrl](https://github.com/parthrbhatt/pyShortUrl) is a library, I wrote recently in python, that you can use from within your application to shorten your urls. I added initial support for 7 domains: [goo.gl](http://goo.gl/), [bit.ly](http://bit.ly/), [j.mp](http://j.mp/), [bitly.com](http://bitly.com/), [tinyurl.com](http://tinyurl.com/), [v.gd](http://v.gd/) & [is.gd](http://is.gd/). pyShortUrl will also allow you to get QR code for short urls created using most of these services.

Following is an example of how urls can be shortened with pyshorturl using goo.gl:

```
from pyshorturl import Googl, ShortenerServiceError

long_url = 'http://www.parthbhatt.com/blog/2012/'
service = Googl()
try:
    short_url = service.shorten_url(long_url)
    print short_url
except ShortenerServiceError, e:
    print 'Error: %s' %e
```

If you want to keep track of urls you shortened and are interested in statistics showing things like how many times your shortened url was accessed, you can specify an API key while creating the Googl object. pyShortUrl offers similar functionality with other supported url shortening services as well. For details about how to use pyShortUrl with other url shortening services, checkout the [README file](https://github.com/parthrbhatt/pyShortUrl/) for the project on github.

Depending upon the service you choose, you may be required to create an account with the service. Also, note that you will have to abide by the rate limiting constraints imposed by the url shortening service you choose. For all such details about various services supported by pyShortUrl, checkout this [wiki page](https://github.com/parthrbhatt/pyShortUrl/wiki/Detailed-Information-about-URL-shortening-services-supported-by-pyShortUrl).

Also, don’t forget to checkout the pyshorturl-cli.py utility that is shipped with the library. It is a command line utility that offers all the functionality of pyShortUrl. Following is a quick example of how you’d use pyshorturl-cli.py to shorten a url with bit.ly:

```
$ python pyshorturl-cli.py --service bit.ly \
      --api-key <bitly-api-key> --login <bitly-account> \
      --long-url http://www.parthbhatt.com/blog/2012/
http://bit.ly/IlLRrd

```

To find out how to get your <bitly-api-key> and <bitly-account> refer to this wiki page.
