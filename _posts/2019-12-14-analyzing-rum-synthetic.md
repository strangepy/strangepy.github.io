---
title: Analyzing Real User vs. Synthetic Data
subtitle: The advantages and disadvantages of each
tags: data-analysis, web-performance
---

When measuring the performance of your website, your options are to use real user (humans who are interacting with the site naturally) or synthetic (automated testing with the sole purpose of observing your site) data. The data sets produced by these users are quite different, and this post explains the advantages and disadvantages of each approach. 

# Real User Data
As mentioned before, "real users" are the regular people who browse your site every day and interact with it naturally. These users use a variety of device types, browsers, and operating systems and are located anywhere in the world. 
## Advantages
- Includes all device types 
- Includes multiple operating systems and versions
- Includes numerous browsers and browser versions 
- Full coverage of geographies 
- Traffic can be segmented through site analytics 
- Coverage across all pages of the website 
- No scripting (Selenium/Katalon) required
- Ability to track business metrics (conversion rate, revenue amount, orders)

## Disadvantages
- Without user traffic, no measurements are recorded. This is particularly an issue during late night or early morning time periods. 
- Requires an analytics tag to track data. If the tag is removed, no data is recorded. 
- Data collection is not as comprehensive as synthetic data. Only the performance metrics are collected. 
- iframes and an early onload event (depending on the tag implementation) can interfere with data collection. 
- Dataset can be "noisy" since it may include interference from plugins or client-side software that you have no control over. 
- Performance metrics can be slower due to client-side hardware, software, plugins, cellular connection, etc. 

# Synthetic User Data

## Advantages

## Disadvantages

# The Bottom Line
You should use a combination of real user and synthetic data when analyzing your website's performance. Real users provide an excellent guide for the real-world performance of your site, while synthetics offer targeted, clean testing of a single page or small number of pages. 
