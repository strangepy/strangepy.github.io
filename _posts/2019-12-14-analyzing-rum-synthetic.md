---
layout: post
title: Analyzing Real User vs Synthetic Data
subtitle: The advantages and disadvantages of each
tags: data-analysis
---

When measuring the performance of your website, your options are to observe load times for real users (humans who interact with the site naturally) or synthetic (automated testing with the sole purpose of observing your site) users. The data sets produced by these users are quite different, and tit is important to understand the advantages and disadvantages of each approach. 

# Real User Data
As mentioned before, "real users" are the regular people who browse your site every day and interact with it naturally. These users use a variety of device types, browsers, and operating systems and are located anywhere in the world.

## Advantages
- Includes all device types 
- Includes multiple operating systems and versions
- Includes numerous browsers and browser versions 
- Full coverage of geographies 
- Coverage across all pages of the website 
- No scripting (Selenium/Katalon) required
- Ability to track business metrics (conversion rate, revenue amount, orders)
- Automatically picks up new links and popular pages on your website

## Disadvantages
- Without user traffic, no measurements are recorded. This is particularly an issue during late night or early morning time periods. 
- Requires an analytics tag to track data. If the tag is removed, no data is recorded. 
- Data collection is not as comprehensive as synthetic data. Only the performance metrics are collected. 
- iframes and an early onload event (depending on the tag implementation) can interfere with data collection. 
- Dataset can be "noisy" since it may include interference from plugins or client-side software that you have no control over. 
- Performance metrics can be slower due to client-side hardware, software, plugins, cellular connection, etc. 
- Website changes can take hours to days to propagate, since the majority of real users will have browser-side caching. 

# Synthetic User Data
Synthetic users are essentially automated computer systems with instructions to visit specific pages of your site at regular time intervals. Synthetic data is excellent for clean browser tests and availability monitoring. 

## Advantages
- Provides consistent traffic for testing the site, even when there are no users visiting the site. 
- Excellent for baselining and benchmarking performance metrics. 
- Easier to compare to competitor's sites since the synthetic environment is exactly the same. 
- Robust reporting capabilities that can include screenshots, HAR files, console log data, as well as performance metrics. 
- Measurements are recorded from a clean, consistent setup and thus there is no interference from other software or plugins. 
- Site updates propagate quickly to synthetic tests since these agents usually clear the cache and begin a new session between each measurement. 

## Disadvantages
- Can be expensive to set up if your site requires synthetic scripting or a large number of pages. 
- Cannot measure checkout and order confirmation pages without a test account or test credit card. 
- Limited number of browsers, operating systems, and device types available. 
- Mobile device types and browsers are typically emulated, and do not load the website using mobile hardware. This can cause misleadingly low performance metrics for mobile devices. 
- Limited number of locations available. Typically these locations are in the same area as popular servers (particularly with Amazon) which can lead to misleadingly fast performance metrics. 
- Requires manual intervention to add in new URLs (such as the launch of a new product or new site) or modify existing URLs (such as site restructuring) and as such test may not be running against the most relevant pages at all times. 
- If the host of the synthetic agent goes offline, all synthetic data will cease. 

# The Bottom Line
You should use a combination of real user and synthetic data when analyzing your website's performance. Real users provide an excellent guide for the real-world performance of your site, while synthetics offer targeted, clean testing of a single page or small number of pages. 
