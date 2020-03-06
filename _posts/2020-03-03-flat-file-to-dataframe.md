---
title: How to Format a Flat File into a Spreadsheet
subtitle: Using Python and Pandas
tags: python
image: /img/annie-spratt-rdVLN3UFmpU-unsplash.jpg
---

# Background
The general concept here is to take a flat text file, import it into Pandas, manipulate and reformat the data as needed, and then export it to an Excel spreadsheet. This specific example is to format the export of a jMeter crawl. The jMeter process, at a high level, checks a given list of a hotel's webpages for banned/forbidden words and phrases. The text file output contains all URLs that experienced an issue, and indicates whether that issue was an unavailable URL, URL with an error (such as a 404 response code), or a URL containing a banned term. 

*Note: If this is your first time working with a Python example, please check out my page on setting up a Python virtual environment.*

## Dependencies 
For the first iteration of this project, the dependencies are pretty simple. 
```
import pandas
```

# Processing Steps
At a high level, this process is broken down into four parts: 
1. Import the text file 
2. Separate it into desired data sets (unavailable, errors, banned)
3. Format each data set per individual requirements 
4. Export the data to spreadsheets
These steps are essentially the minimum viable product for this project. In the next post, we will go into some additional items added in to this process to make it more robust. 

## Importing a text file 
Python provides multiple methods to read in data, but in this case we will be using a simple `pandas.read_csv()` import command. 
```
IMPORT PATH = "C:\Users\strangePy\Downloads
raw_frame = pandas.read_csv(IMPORT_PATH, delimiter=",", header=None, names=['data'])
```

## Separating the text file
Now that we have the data in a pandas dataframe, we need to separate it into the desired lists. For this project, these lists are Errors, Unavailable, and Banned. Errors contains all URLs and HTTP response codes for links with an error. Unavailable includes all URLs and HTTP response codes for the unavailable links. Banned includes all of the URLs that contained a banned term, as well as the banned term and the hotel code. 
```
unavailable_frame = # raw frame rows that begin with C
banned_frame = # raw frame rows that begin with http
error_frame = # raw frame rows that do not begin with http or C (previously they began with R)
```

## Formatting the frames
Now the real meat of the project begins. There are a few formatting changes that need to be made to the unavailable and error frames, but the banned frame needs quite a few updates. 
```
# sort the frames
# drop unnecessary columns
# find and replace phrases in banned frame with commas and expand frame into additional columns 
```
Different formatting steps will be required depending on your project, but this entire section can be replaced with any formatting you need. 

## Exporting the frames 
For this project, the client wanted one spreadsheet for each frame. Typically, you would want to add multiple sheets to one report, which would be significantly simpler, but the general concepts below still apply. 
```
EXPORT_PATH = "C:\Users\strangePy\Downloads\"
unavailable_frame.to_excel(EXPORT_PATH + "Unavailable", engine='xlsxwriter', sheet_name='Unavailable', index=False)
```

And that's it!! You should now have your fully formatted spreadsheets ready to send off to another system or the final end-user! Next up: adding some additional layers of content to this process to make it a bit more robust. 
