---
layout: post
title: Should you switch to a SPA or PWA?
subtitle: Potential risks and benefits of each structure.
image: /img/karl-pawlowicz-av6mpWu20_4-unsplash.jpg
tags: [web-performance, web-development]
---

# What are SPAs and PWAs? 
First off, let's clarify the classic website model. Typically, a website owner will generate a set of pages (such as home, about, contact, etc.) and the browser downloads the HTML of each page when the user clicks on a link. This can result in reloading and re-rendering of common elements, such as header and footer bars. 

SPA stands for Single Page Application. In essence, a single page application will contain you entire web application within a single page. This means that the user's browser will download one webpage with the application and all further navigation will trigger network requests to dynamically update content, but not reload the base page. SPAs are excellent for social networks, software-as-a-service platforms, and sites that do not rely on SEO. 

PWA stands for Progressive Web Application. Progressive Web Applications are a bit more difficult to define, as they do not have to use a specific architecture. One major feature of PWAs is the use of service workers as proxies between the server and the browser. Servie workers handle offline functionality, push notifications, and caching of website assets. So the bottom line is that PWAs do not have to be contained in one single page.

PWA Guidelines
- Use HTTPS
- Implement service workers
- Contain web manifest file with icons

To make matters a bit more confusing, Single Page Applications can be Progressive Web Apps. If a SPA uses service workers and other PWA guidelines, then it can be both an SPA and a PWA. PWAs, on the other hand, are often not single page applications. 

# Advantages
## SPA 
- Feels more like a native app, where there are persistent elements visible on the page while the app fetches new content
- Application can be largely pre-built using a common framework (Angular, React, Vue, and Ember)
- Navigation after the intiial page load is fast and responsive 
- Effective and robust local caching 
- Debugging  and troubleshooting tools built straight into Chrome
- Often cheaper and faster to develop than a native app

## PWA
- Can be more flexible
- Often easier to customize 
- Offline capabilities
- Push notifications 
- Ability to add home screen icon to mobile devices (this may or may not be important, depending on your business)
- Typically strong search engine optimization capabilities
- Often cheaper and faster to develop than a native app

# Disadvantages 

## SPA 
- May lower your site's search rankings (although Google has recently implemented crawling of dynamic pages)
- Initial page load can be slower due to the entire appication being contained in one page 
- If implemented poorly, can cause excessively large JavaScript libraries to load
- Relies on the framework for functionality and can be difficult to customize 
- Lack of default back button in the browser (implementing the history API can mitigate this)
- If initial page load is compromised by cross-site scripting (XSS) attacks, then the rest of the app is compromised
- Client must enable JavaScript (this is becoming less and less of an issue, but worth mentioning)

## PWA 
- Requires more construction upfront since application is not pre-built by library
- Service workers and PWAs are not supported on all devices (historically Apple devices)
- Fewer debugging tools available 
- Performance monitoring may be limited when the user is offline 
- PWAs may not work properly (or at all) on some browsers, such as Safari, Internet Explorer, and Edge

# Are they worth it? 
This decision depends on whether you are building a website from scratch or converting a pre-built website. If your business requires presence in a native app store, then these are not an option for you. However, if you would like to improve your website speed and user experience, then SPAs and PWAs are worth considering! 

# Wrap-up
The bottom line is that the decision to switch to a SPA or a PWA greatly depends on the structure of your current website and development team. Businesses can benefit from one approach, or another, or even a hybrid web application. For a personalized evaluation, please reach out to me and I would be happy to help!

<!-- Angular, Material, top frameworks -->
## Further Reading
https://www.simicart.com/blog/pwa-vs-spa/
https://love2dev.com/blog/pwa-spa/
https://developers.google.com/web/updates/2018/05/beyond-spa
https://rubygarage.org/blog/single-page-app-vs-multi-page-app
https://medium.com/@moqod_development/advantages-and-disadvantages-of-progressive-web-apps-6f019223cb17
