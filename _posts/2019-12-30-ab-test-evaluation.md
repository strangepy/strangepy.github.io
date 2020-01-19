---
layout: post
title: Best practices for A/B testing
subtitle: and pitfalls to avoid 
image: /img/john-mark-arnold-WItJTJsW97w-unsplash.jpg
tags: [web-performance, data-analysis]
---
Every website owner should be familiar with A/B testing and best practices to ensure success. In today's post, we break down some of the best practices and pitfalls associated with A/B testing. 

# What is an A/B test? 
First off, let's make sure we're all on the same page about what exactly an A/B test is and is not. An A/B test for web performance involves splitting your website's traffic into two groups, commonly referred to as A and B. This allows you to test a particular website change on some portion of your website traffic, while maintaining a control group. The control group does not receive the website change, so this makes it extremely easy to isolate the impact of the change on your website's business metrics (conversion rates, revenue, etc.).

# What are some best practices to implement? 
Ensure you have a provider who can split traffic across your site evenly, or at least at a ratio appropriate to your site's traffic levels. For example, a 80%-20% split may be sufficient for a highly trafficked website to collect statistically signficant data, but smaller websites are best served by a 50%-50% split.
When analyzing the data, make sure you evaluate the results for as many outside influences as possible. This usually involves decomposing your time series into separate components and evaluating each one. Otherwise, you may end up attributing an increase in revenue to one of the A/B segments, when in reality it may be due to seasonal changes or a holiday. 
Do your best to test only one variable at a time. If you mix in too many changes, then it will be hard to isolate exactly which change is responsible for any difference in business metrics. 

# What are common pitalls to avoid?
A common issue in A/B testing is implementing the website change on too small of a user group. Essentially, if one segment of your A/B test is low enough, then you will not be able to collect enough data for statistically significant results. Keep in mind that A/B test providers typically apply a split ratio to user sessions, rather than individual page views. This means that lower-traffic pages, such as checkout and order confirmation, will effectively receive even less traffic than your inteded split percentage.
Don't be afraid to stop a change if it is negatively impacting your users. All too often, a service will promise an increase in conversions or revenue and stall for time if those improvements never materialize. Even if you have put time and effort into the changes, avoid rolling them out to your entire user base until it is ready and you can confirm the advantages.
Avoid changing the success criteria after the test. Make sure you know beforehand what results are considered a success. For example once an A/B test is finished, some providers will try to convince you to switch metrics to another one that makes their service look better. Do not fall prey to this sales tactic, since it may not be what is best for your users. 


# Wrap-up
The bottom line is that you should not be afraid to run A/B tests on your site. Even if the results are not always what you are hoping for, it is an invaluable source of information. Make sure you have a provider and data analyst who you can trust and who can help you improve your website's experience. 

<!-- top AB Services and other validity tests -->
<!-- potentially add service for AB test analysis and consulting -->
