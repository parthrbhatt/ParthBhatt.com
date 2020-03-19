---
title: "Node.js Microservice Scaffold"
date: 2020-03-05
categories:
- code
- projects
tags:
- node.js
- microservice
draft: False
thumbnailImagePosition: left
thumbnailImage: ../../media/node.js.png
---

In an earlier post we listed some items that one would do to get their microservice production ready.

<!--more-->

In this post, lets take a look at how one would go about implementing such a microservice. Specifically, in this post, we'll write a scaffold for a microservice in node.js

### Dependencies

- Koa (koa-bodyparser & koa-router)
- esm

### Makes use of multiple cores

```
$ npm start

> nodejs-microservice-scaffold@1.0.0 start /Users/parthbhatt/workspace/code/nodejs-microservice-scaffold
> node svc-start

37152: API Worker initialized. Listening for requests on 8999.
37154: API Worker initialized. Listening for requests on 8999.
37156: API Worker initialized. Listening for requests on 8999.
37153: API Worker initialized. Listening for requests on 8999.
37155: API Worker initialized. Listening for requests on 8999.
37158: API Worker initialized. Listening for requests on 8999.
37157: API Worker initialized. Listening for requests on 8999.
37159: API Worker initialized. Listening for requests on 8999.
```

### Logs All Requests and Responses

```
{
  type: 'request',
  id: '31aab2e1-1255-4cd3-87da-fbf2eeb1b65d',
  proc: 35096,
  url: '/api/v1/health',
  method: 'GET',
  headers: {
    host: '127.0.0.1:8999',
    'user-agent': 'curl/7.64.1',
    accept: '*/*'
  },
  query: [Object: null prototype] {},
  body: {}
}
```

```
{
  type: 'response',
  id: '31aab2e1-1255-4cd3-87da-fbf2eeb1b65d',
  proc: 35096,
  url: '/api/v1/health',
  method: 'GET',
  elapsed: 0,
  headers: [Object: null prototype] {
    'x-request-id': '31aab2e1-1255-4cd3-87da-fbf2eeb1b65d',
    'content-type': 'application/json; charset=utf-8',
    'x-request-time': '0'
  },
  body: { status: 'healthy' }
}
```

### Exposes a health APIs

You must have noticed that the service allows its health to be queried using api `/api/v1/health`

As mentioned before, you can find the code at the github repo: [parthrbhatt/pyShortUrl](https://github.com/parthrbhatt/pyShortUrl)
