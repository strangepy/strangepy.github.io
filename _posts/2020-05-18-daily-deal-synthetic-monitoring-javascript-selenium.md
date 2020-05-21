---
title: How to Automatically Monitor Daily Deals Pages
subtitle: without manually creating a hundred monitors
image: /img/justin-lim-JKjBsuKpatU-unsplash.jpg
tags: [web-performance, javascript]
---

Daily deals pages are one of the most important pages of your website. But they change every single day, or sometimes multiple times per day. How can you check that each one of these products is up and running without manually creating a new monitor each day? 

## Setup 
For today's task, you will need access to a synthetic monitoring solution that supports evaluating JavaScript on the page. Ideally, this will be Selenium or Katalon Studio. My examples will be based on Selenium, but with minor changes you should be able to port this code over to any other synthetic monitoring software. If you are interested in setting up Selenium with the Python bindings on your local machine, follow this guide on [Getting Started with Selenium Testing in Python](https://www.strangepy.com/2020-02-06-python-chrome-webdriver/).

## The Problem 
There are a few approaches to solving this problem. Some of the ones that first come to mind are: 
1. Manually configuring a synthetic single-page monitor for each deal of the day. 
2. Writing a script to automatically configure a synthetic single-page monitor for each deal of the day. 
3. Write a multi-page synthetic monitor that will visit the deal of the day page, and then dynamically visit each individual deal page. 

### Manual Configuration
Manually configuring a synthetic monitor for each product is not scalable and is a heavy time investment. If you want full coverage of all hours of the day, then you may need to update the monitors when the daily deals refresh (which is often at midnight or early in the morning). What about weekends? What about when products are sold out? This approach should be your last choice. 

### Automatic Creation with API
Automatically configuring the single page monitors is a big step in the right direction. You will need to be able to run a process to pull down a URL for each product included in the daily deals, but that is much easier than manually configuring it. This approach hinges on whether or not the synthetic monitoring solution you are using has an API for creating and deleting synthetic monitors. The ability to programatically delete a monitor is also important for products that have gone out of stock. The client I was working with in this case did not support creating and deleting synthetic monitors programatically, so we considered the next option. 

### Automatic Monitoring with JavaScript
Automatically pulling the URLs for each product on the deals page is another great approach. This will monitor the deals page itself, as well as each individual product. For bonus points, this script will also only include in stock products, since monitoring an out of stock product is pointless. This approach involves mapping up each product with a specific minute of the hour, so it will consistently hit one product each minute for the entire hour. If there are greater than 60 products offered on your deals page, then you can add additional logic for multiple synthetic locations as well. 

## The Solution

To get started, I searched online for "daily deals" and the top result was [Best Buy's Deal of the Day page for the Electronics category](https://www.bestbuy.com/site/misc/deal-of-the-day/pcmcat248000050016.c). This page will work well for this example, but you can use any deals page. 

### Evaluating the Page

First, you should open up this page in your browser. There are a few things to look for on the page. At the top of the page, there is a featured deal of the day. 

![Featured Deal of the Day](/img/dealoftheday1.png)

If you scroll down a bit you can see there are quite a few "bonus deals" of the day that are being offered. There are also some "Shop Now" advertisement buttons that we do not want to include in this test, since those are not special deals of the day. 

![Bonus Deals and Shop Now](/img/dealoftheday2.png)

Scrolling down a bit further, we can see that there are even a few deals that have sold out. Since there are only a few hours left before these deals expire, this is entirely normal. As you get closer to the end of the day, some products should be sold out. So, we also want to exclude expired deals from the test. 

![Sold Out Deals](/img/dealoftheday3.png)

Remember to check for each of these items on your daily deals page. This will be important for the next step where we actually start writing a bit of code. 

### Writing the JavaScript 
Essentially the hardest part of this synthetic monitor is writing the JavaScript function to map each minute to a different product. But first, we need to pull the list of product URLs from the page, while excluding sold out products and products that are not part of the daily deal promotion. 

For our test page, initially we can start out with a selector for all Add to Cart buttons. 

```JavaScript
document.querySelectorAll('button.add-to-cart-button')
```

There are 14 buttons returned by this query selector. If we take a closer look at each one, the sold out button is included in the list. This seems counterintuitive, since a disabled button displaying "sold out" is not typically an add to cart button. The solution for this site is simple, we need to add a check to ensure that the add to cart button is not disabled. 

```JavaScript
document.querySelectorAll('button.add-to-cart-button:not(.btn-disabled)')
```

Now we can throw those buttons into a variable. Since we do not want to click on the Add to Cart button itself, we can use one element to test pulling the href for that product. 

```JavaScript
var addToCartButtons = document.querySelectorAll('button.add-to-cart-button:not(.btn-disabled)');
// for testing only
addToCartButtons[12].closest('div.row').querySelector('[id*=list-offer-header] > a');
```

Now that we have the path to the href for each product with a valid Add to Cart button, we can push those into a separate array with a forEach loop. 

```JavaScript
// empty array to receive links 
var productLinks = [];
// push each link into the array 
addToCartButtons.forEach(element => productLinks.push(element.closest('div.row').querySelector('div.info-block > h3 > a')))
```

Now that we have the array of links, the only piece of JavaScript left is to map each product to a minute and then click on the product for the current minute. For this part, we have assumed that there are up to 60 products on the daily deals page, so we want to expand the array above to a length of sixty. Keep in mind that it would be possible for the daily deal page to only have a single product, so in this case we want to repeat the `productLinks` array 60 times, flatten it, and then slice out the first 60 items. If you are scaling up your script to allow for more than 60 products, then simply replace "60" with the desired number and modify the click() function in the next step. 

```JavaScript
// expand the productLinks array into an array of 60 elements 
var expandedProductLinks = Array(60).fill(productLinks).flat().slice(0,60);
// if your page can have more than 60 products, fill your number in place of "length" below
var expandedProductLinks = Array(length).fill(productLinks).flat().slice(0,length);
```

From this point, the only bit of JavaScript left is to grab the current minute and actually click on one of the products! 

```JavaScript
// grab the current time 
var currentTime = new Date(); 
// click on the appropriate product for the current minute
expandedProductLinks[currentTime.getMinutes()].click();
```

The hard part is now done! We will strip out the whitespace and put it into a runScript or executeScript command, but the core script logic is complete. 

### Putting it into a Monitor
Adding this into a monitor is surprisingly easy. You need an open command to open the daily deals link, some validation to verify that page has loaded, and then a runScript command to evaluate the JavaScript snippet we wrote above. At the end of the script, you can add any kind of validation to verify that the product page has loaded properly. 

## Closing Thoughts
As I mentioned above, you should tailor the HTML selectors to your individual site. Keep in mind that Best Buy may not have the same site structure long-term, so the example above will not work forever. But, it should serve as an excellent starting point and an example to get your own monitoring script up and running. If you run into any issues or would like help, please let me know! 

### Future Enhancements
For some future enhancements, you may want to scale up this solution if your site offers greater than 60 deals in a day. This particular client did not anticipate releasing more than 60 deals per day, but I will be writing a post covering that functionality in the future. You may also want to add some additional success criteria for each product page, such as verifying a product image is visible and validating particular elements. 

### Full JavaScript Code 

```JavaScript 
// grab all buttons with enabled Add to Cart text
var addToCartButtons = document.querySelectorAll('button.add-to-cart-button:not(.btn-disabled)');

// create empty array to receive links 
var productLinks = [];

// push product link for each button into the array 
addToCartButtons.forEach(element=>
    productLinks.push(
        element.closest('div.row').querySelector('div.info-block > h3 > a')
    )
)

// expand the productLinks array into an array of 60 elements 
var expandedProductLinks = Array(60).fill(productLinks).flat().slice(0, 60);

// grab the current time 
var currentTime = new Date();

// click on the appropriate product for the current minute
expandedProductLinks[currentTime.getMinutes()].click();
```

### Full Python Code 

```Python
# Be sure to save the JavaScript above into a file that will be loaded in 
```
