---
title: How to Automatically Monitor Daily Deals Pages
subtitle: without manually creating 154 monitors
image: /img/justin-lim-JKjBsuKpatU-unsplash.jpg
tags: [web-performance, javascript]
---

Daily deals pages are one of the most important pages of your website. But they change every single day, or sometimes multiple times per day. How can you check that each one of these products is up and running without manually creating a new monitor each day? 

# Setup 
For today's task, you will need access to a synthetic monitoring solution that supports evaluating JavaScript on the page. Ideally, this will be Selenium or Katalon Studio. My examples will be based on Selenium, but with minor changes you should be able to port this code over to any other synthetic monitoring software. 

## The Problem 
There are a few approaches to solving this problem. Some of the ones that first come to mind are: 
1. Manually configuring a synthetic single-page monitor for each deal of the day. 
2. Writing a script to automatically configure a synthetic single-page monitor for each deal of the day. 
3. Write a multi-page synthetic monitor that will visit the deal of the day page, and then dynamically visit each individual deal page. 

Manually configuring a synthetic monitor for each product is not scalable and is a heavy time investment. If you want full coverage of all hours of the day, then you may need to update the monitors when the daily deals refresh (which is often at midnight or early in the morning). What about weekends? What about when products are sold out? This approach should be your last choice. 

Automatically configuring the single page monitors is a big step in the right direction. You will need to be able to run a process to pull down a URL for each product included in the daily deals, but that is much easier than manually configuring it. This approach hinges on whether or not the synthetic monitoring solution you are using has an API for creating and deleting synthetic monitors. The ability to programatically delete a monitor is also important for products that have gone out of stock. The client I was working with in this case did not support creating and deleting synthetic monitors programatically, so we considered the next option. 

Automatically pulling the URLs for each product on the deals page is another great approach. This will monitor the deals page itself, as well as each individual product. For bonus points, this script will also only include in stock products, since monitoring an out of stock product is pointless. This approach involves mapping up each product with a specific minute of the hour, so it will consistently hit one product each minute for the entire hour. If there are greater than 60 products offered on your deals page, then you can add additional logic for multiple synthetic locations as well. 

To get started, I searched online for "daily deals" and the top result was [Best Buy's Deal of the Day page for the Electronics category](https://www.bestbuy.com/site/misc/deal-of-the-day/pcmcat248000050016.c). This page will work well for this example, but you can use any deals page. 

## Writing the JavaScript 
Essentially the hardest part of this synthetic monitor is writing the JavaScript function to map each minute to a different product. 
