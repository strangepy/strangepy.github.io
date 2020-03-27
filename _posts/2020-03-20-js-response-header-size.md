---
title: How to Monitor HTTP Response Header Size
subtitle: Using only JavaScript and Selenium
img: /img/vincent-ghilione-b3s2P1VHVcU-unsplash
tags: [javascript, security]
---

# Background 
When I first received this request, my first thought was: why would anyone want this? If you own the website, then your servers already have access to the HTTP response header size. If you do not own the website, then you likely are not interested in monitoring the response header size. 
But, some clients want to make sure that the response headers are under a given threshold on the client side. You can test for this using a selenium script run on a fully-fledged browser. Response headers are increasingly important for Content Security Policies (CSP) that are injected via the response header rather than in the <meta> tag. 

# Constraints 
For this request in particular, the client wanted to be able to use their third-party software to send an alert if the response header went over a certain size. They submitted a feature request to this third-party provider, but were interested in an interim solution. The synthetic script solution in this third-party platform did allow for a limited number of selenium commands, including executeScript/runScript. So, this functionality will be put together almost entirely in JavaScript. 

# Functionality 
To pull the response header size, we'll need to break it down into two parts. 
1. Request all of the response headers of the current page view. 
2. Encode the headers and calculate the size (in bytes) of that encoded string. 
Once we have the size of the current response headers, we can compare it to the threshold amount. Here, we will be using 80% of the Apache server maximum response header size, but you can use any threshold you want.  If the response header is currently below that threshold, then we can create a new HTML element with a given ID and check for it with the selenium script. If the current response header is above the threshold, then we can write a different element to the page and the selenium script will fail. The third-party software will generate an alert for each script failure, so all we need here is to cause the script to fail. 

## Request response headers for the current page view 
First off, we should create a new [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) request: 
```var request = new XMLHttpRequest();```

Then, we want to initialize the request and grab the current page (document.location). We can set 'async' to false here as well. 
```
request.open('GET', document.location, false);
request.send(null)
```

Finally, we can use the [getAllResponseHeaders()](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/getAllResponseHeaders) method of XMLHTTPRequest and assign that data to a variable. 
```
var headers = request.getAllResponseHeaders().toLowerCase();
```
Now that we successfully grabbed the response header data, we need to encode it and calculate the size. 

## Calculate the size in bytes of the response headers 
So there are a lot of different methods to calculate the size of response headers. The main struggle here is that different browsers can use different kinds of character encoding. Fortunately, this is going to be put into a synthetic script with full control over the browser, and we happen to know that the scripts run on nearly the current version of Chrome (version 79, with version 80 being the latest). 
For newer browsers, we can accomplish this natively by creating a new [TextEncoder](https://developer.mozilla.org/en-US/docs/Web/API/TextEncoder) object. 
```
var encoder = new TextEncoder();
```

Now that we have an encoder object, we can call [the encode() method](https://developer.mozilla.org/en-US/docs/Web/API/TextEncoder/encode) to encode our headers data. 
```
var headersEncoded = encoder.encode(headers);
```

We are almost done the hard part! We now have the headers data encoded in [UTF-8](https://www.w3schools.com/charsets/ref_html_utf8.asp). All we have to do now is grab the size in bytes of the UTF-encoded response header data. 
```
var headersBytes = headersEncoded.length; 
```
The size in bytes of the response header data is now stored in the variable ```headersBytes``` and we can move on to the next step. 

## Compare the current response header size to the allowed limit 
First off, we want to create some variables to store the results: 
```
var customId = "";
var customMessage = "";
var threshold = 6552;
```
You should enter the threshold that you want to use here. For an example, we'll go with the [Apache HTTP Server documentation](http://httpd.apache.org/docs/2.2/en/mod/core.html#limitrequestfieldsize) which lists the maximum size as 8,190 bytes. This is configurable and can be set larger, but 8,190 bytes should suffice as a starting point. In this case, you may not want to be notified *after* the response header passes the allowed size, since that can degrade your site performance and user experience. Instaed, we will be using 80% of the maximum, so the threshold has been set to 6,552 bytes. 

Next, we'll compare the current response header size to the allowed threshold. The customId and customMessage should be set differently depending on the result, so we have a pass, a fail, and an error result. The script should only fail on the fail and error. However, we also set a custom message that will be added to the HTML in the next step. You may not want the customMessage printed to the screen, so feel free to remove this part, but we will have it here so the result is captured in the screenshot. 
```
if(headersBytes < threshold){
    customId = "response-header-size-check-passed";
    customMessage = "Pass. Observed response header size " + headersBytes + "is less than the threshold of " + threshold;
} else if(headersBytes > threshold){
    customId = "response-header-size-check-failed";
    customMessage = "Fail. Observed response header size " + headersBytes + "is greater than the threshold of " + threshold;
} else{
    customId = "response-header-size-check-error";
    customMessage = "Error. Encountered an error while retrieving response header data";
}
```
You may be tempted to make the id something shorter, and that might be totally fine depending on the site you visit. I made it a bit longer and more explicit to reduce the chance that this id will already be on the page or conflict with any other code on the customer's site, so feel free to use whichever method is best for you. 

## Create an HTML element for the selenium script to check
This is the shortest part of the project, we are just going to create a new node, create a heading with the custom id and custom message, and then append that node to the first child in the body. You can really append the elemnt to any part of the page, but we want it to be high enough up to be captured in the screenshot, so I chose the first child of the body. 
We also added some optional styling here to set the color, border, width, and padding of the message box to ensure it stands out and has some space from the other elements on the page. You can customize this to your liking or remove it altogether. 
```
var messageNode = document.createElement("div"); 
messageNode.innerHTML = "<h2 id='" + customId + "' style='color:red;border: 5px solid red;width: 800px; padding: 5px;'>" + customMessage + ".</h2>";
document.querySelector("body").firstElementChild.appendChild(messageNode);
```
You should be able to run the full code in your browser and see a result printed to the top of the page. 

# Wrap-up 
With all that JavaScript out of the way, all you need to do is create a three-step selenium script. 
1. ```open``` the URL you want to test 
2. ```executeScript``` or ```runScript``` with the JavaScript code above 
3. ```verifyElementPresent``` with ```id=response-header-size-check-passed``` 

And that's it! If the proper id is not found on the page, then either the response header size is too large or the script encountered an error pulling the header size. Either way, the script will fail and send an alert to the client. With this configuration, you could add additional logic to send a separate alert type for errors than for failures, but this was just a quick one-hour request so we will leave it here for today. 

## Further Reading
[There is an excellent StackOverflow response in this thread.](https://stackoverflow.com/questions/220231/accessing-the-web-pages-http-headers-in-javascript)

## Full Code
```
// get all response headers for the current page 
var request = new XMLHttpRequest();
request.open('GET', document.location, false);
request.send(null)
var headers = request.getAllResponseHeaders().toLowerCase();

// calculate the size of the response header data
var encoder = new TextEncoder();
var headersEncoded = encoder.encode(headers);
var headersBytes = headersEncoded.length; 

// compare the current size of the response headers to the threshold
var customId = "";
var customMessage = "";
var threshold = 6552;

if(headersBytes < threshold){
    customId = "response-header-size-check-passed";
    customMessage = "Pass. Observed response header size " + headersBytes + "is less than the threshold of " + threshold;
} else if(headersBytes > threshold){
    customId = "response-header-size-check-failed";
    customMessage = "Fail. Observed response header size " + headersBytes + "is greater than the threshold of " + threshold;
} else{
    customId = "response-header-size-check-error";
    customMessage = "Error. Encountered an error while retrieving response header data";
}

// create the HTML element for the selenium script to check
var messageNode = document.createElement("div"); 
messageNode.innerHTML = "<h2 id='" + customId + "' style='color:red;border: 5px solid red;width: 800px; padding: 5px;'>" + customMessage + ".</h2>";
document.querySelector("body").firstElementChild.appendChild(messageNode);
```
