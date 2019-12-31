---
layout: post
title: Common CSP Deployment Methods
subtitle: The pros and cons of each. 
tags: [security, web-performance]
---

CSPs are typically deployed to any of these locations via a CDN (such as Akamai), an A/B provider (such as Sitespect), or on the server-side. Server-side deployment with Apache typically involves adding the CSP code to the httpd.conf in your VirtualHost or an .htaccess file. Server-side deployment with Nginix typically involves adding the header code to your server {} block.

# Meta Tag
## Pros
- No limit on the size of the CSP. This can be huge depending on the number of domains that your site needs to allow. 

## Cons
- Does not support the "frame-ancestors" or "report-uri" directive. 
- Report only mode is not supported in this implementation and the entire CSP will be ignored if you use this. 
- Urgent-loading of scripts or stylesheets (preload or push implementation) will bypass the CSP entirely and load regardless. 
- CSP must be added to the head tag before any script or link tags. This can be a strain operationally depending on your setup. 


# Response Header
## Pros
- Can be easier on the operational workflow, depending on the setup
- Depending on the setup, can be easier to update the CSP in the future
- HTTP headers take priority over meta tag elements
- HTTP headers are more easily stored in the cache (whihch can be a disadvantage in some instances, especially if you are trying to implement front end optimizations as well)

## Cons
- Depending on how long the CSP is, header size limits can be an issue (header size limits acan be imposed by both a CDN or the browser). 
- Entirely unrelated elements like cookies can contribute to this header size limit. 
- Headers are not usually compressed (except HTTP2 which supports header compression). 

# Both Meta Tag and Response Header

## Pros
- Combines the best of both approaches above
- Supports both "frame-ancestors" directive in the header, as well as a large CSP using the meta tag

## Cons
- The header portion of the CSP is still subject to header size limits. 
- This is the most operationally complex. 
- The list of domains wilmust be identical between the two. The header list of domains takes priority over the meta tag, but this can be extremely confusing. 


<!-- Samsung, Akamai, bam-x, etc. -->
