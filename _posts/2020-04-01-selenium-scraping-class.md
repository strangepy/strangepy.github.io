---
title: How to Create a Selenium Class
subtitle: to automatically evaluate a list of URLs 
image: /img/harrison-broadbent-raLeFIxXgDY-unsplash.jpg
tags: [python, web-scraping]
---

If you've ever been handed a huge list of URLs and been asked to evaluate each one, you know how amazing the Selenium library can be. In my previous project on checking for particular substrings in each URL, we went through how to generally use Selenium in this kind of process. This week, we are going to put that process into a reusable class and I have a few more examples of how useful this analysis can be. 

With all of the recent Magecart attacks, a lot of emphasis has been put on Content Security Policies. So today, we will build this class with the intention of applying logic to perform a few checks: 
1. Does the page for this URL contain a <meta http-equiv="content-security-policy> tag?
2. Do the response headers for this URL contain "content-security-policies"?
3. Does the CSP contain ".com" or a similar substring to verify it is a properly structured CSP? 

## Future Enhancements 
In a future post, we will be building a similar class tto use the requests library instead of selenium. 
This project did not make use of multithreading or multiprocessing, but in a future post we will be exploring these options and the associated time improvements.
