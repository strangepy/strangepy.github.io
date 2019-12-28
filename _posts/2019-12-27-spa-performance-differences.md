---
layout: post
title: How SPAs are changing the face of web performance
subtitle: And why that may not be a bad thing. 
tags: [web-performance, web-development]
---

# What are SPAs? 
Broadly speaking, Single Page Applications (SPAs) loads a single page once, and dynamically updates that single page with content from the server as needed. The browser will download one webpage and then have hundreds of additional network request as the user interacts with the website. 

# How do they differ from traditional websites? 
Traditional websites will fully load a new page when the user clicks a button or uses a link. This means that plenty of content is re-downloaded each time the page loads. This duplicate downloading is somewhat mitigated by browser caching, but that only applies after the first page load of the user's visit. SPAs, on the other hand, only load the base website frame (HTML, CSS, JS templates) once and pull in additional data when necessary. This often results in a website with persistent headers and footers, with the main page content refreshing rather than the whole page. This often means a smoother user experience if implemented properly. 

# How is measuring web performance different? 
Web performance metrics for traditional websites are already established and easily defined. For SPAs, most of those web performance metrics only apply to the first load of the website (the "single page" part of SPAs). For SPAs, there are multiple means of defining and measuring the duration of a page load. Common methods include measuring the amount of time elapsed between two network requests, the amount of time between a click event and specific HTML elements rendering on the page, or custom timers that are implemented on the website to manually define performance metrics. So far, there is no single adopted industry standard, and this means that consistently analyzing your SPA's performance and comparing it to competitor's performance can be substantially more difficult. 

# How does this impact the web performance industry? 
Traditional websites are more established and have had plenty of development over the years. SPAs, on the other hand, are relatively new and gaining in popularity. As such, the built in web performance tools in browsers and web performance companies are geared for traditional websites. The web performance industry will slowly catch up to new technologies and define new metrics for SPAs, which overall will be good for website owners and consumers alike. 

<!-- Material/Angular/web performance company -->
