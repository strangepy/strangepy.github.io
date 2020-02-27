---
title: Issues with Overwriting Object Prototypes
subtitle: Identification and resolution
image: /img/javardh-dZ4Ylj91F2M-unsplash.jpg
tags: [javascript, web-performance]
---

One service modifies the function to XMLHttpRequest.prototype.send() to intercept and listen to all the requests, and another service modifies the function to override the default functionailty. Unfortunately, this is not an uncommon issue and usually results in one service breaking or blocking the functionailty of the other service. 

[This post from Medium](https://medium.com/@gilfink/quick-tip-creating-an-xmlhttprequest-interceptor-1da23cf90b76) explains this concept at a very high level, but we will be digging into some of the details below. 
