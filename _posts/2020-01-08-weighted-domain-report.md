---
layout: post
title: Building a Waterfall Graph
subtitle: using Python API requests
image: /img/luke-chesser-JKUTrJ4vK00-unsplash.jpg
tags: [python, web-performance]
--- 
A couple weeks ago, a client approached me and asked about the possibility of building a custom report. Essentially, this report would use a waterfall graph of file load times and output a weighted list of domains to focus on in order to improve web performance. If you don't quite understand what that means, no worries! Everything will be explained below! 
# What is a waterfall graph and how is it useful? 
If you open up your browser's developer tools and click the "Network" tab, you should be able to see the cascading load time of files. An example is in the image below. 
Waterfall graphs can be essential to analyzing web performance, especially when they are aggregated together to form an overall view. Domains earlier in the load will have the highest impact on the user's perceived page load time. 
# Why do you need a custom report? 
After learning what a waterfall graph is, you may be wondering, "Well, that sounds great! Why wouldn't they just look at that to determine which domains are hurting web performance the most?" And in most cases, you would be correct. However, this particular customer has one website for each geographical region. As a company with nearly 10 billion dollars in market capitalization, they do business all over the world and have more than a dozen of geographical regions. With so many different sites, you may not want to load and review that many waterfall graphs, hence the desire for a custom report. 
# How can I build a custom report? 
Unfortunately, I am not at liberty to share the exact code of this custom report, since the API endpoint is propreitary. However, I can go through the methodology and broad strokes of building this report in this post, and there are other posts around making API requests in Python and exporting the data to Excel. 
## Report Requirements
This client was interested in obtaining either a weighted list of domains loading content on each website. The weighted list would apply one multiple to domains loading content before Time to DOM Interactive (to weigh those domains most heavily), a second multiple to domains loading content between Time to DOM Interactive and Onload (to weigh those domains somewhat), and no multiple to domains loading content after onload (to include these, but not apply any weight). Alternatively, three separate lists would also be acceptable for this client, but rolling it all up into one configurable report is best. 

## Making the API Request
As mentioned before, the exact API endpoint is propreitary, but a similar format of the request is below. 
```
import requests

# Set headers
api_headers = {
  'api-username': 'strangepy@gmail.com', 
  'api-key': 'hashhashhashhashhashhash',
  'content-type': 'application/json'
}

# Define API endpoint and environment
API_BASE = 'https://production.strangepy.com/'
API_ENDPOINT = 'web-performance'
API_URL = API_BASE + API_ENDPOINT

# Request JSON 
payload = {
  'website': geographic_site, 
  'startTime': start_epoch, 
  'endTime': end_epoch, 
  'metrics': ['averageStart', 'averageDuration', 'fileSum'],
  'group': 'domain', 
  'limit': request_limit, 
  'minimumSamples': min_sample
}

# Send request
req = requests.post(url= API_URL, headers= api_headers, json= payload)
response = req.text
```
In this case, many of the variables above are read in from a filters file or looked up dynamically based on today's date. Most clients request reports to be run weekly, starting on a particular weekday (such as Monday through Sunday or Saturday through Sunday). 

## Parsing the Data
Once the data comes back from the API, we need to parse it out into the three buckets. 
1. Domains with content before Time to DOM Interactive
2. Domains with content after Time to DOM Interactive, but before Onload
3. Domanis with content after onload (but before the collection cutoff, typically at 60 or 120 seconds)

## Weighting the Domains
Once the data has been separated into three buckets, we can apply the weighting multiples that the customer desires. 

## Exporting the Results 
This project has benefitted from the pandas library, and we are going to use it one more time. All of the data has been assembled into a dataframe (or possible multiple dataframes, if you want one site per Excel tab), and now we just need to write it to an Excel file. There are numerous ways to do this, but in this case we went with xlsxwriter. Alternatively, you could write the dataframe to HTML and then upload it to a separate site or even send it out in an email as part of an automated process. 
