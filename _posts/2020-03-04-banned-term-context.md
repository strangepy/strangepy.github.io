---
title: Pulling Phrase Context from a Webpage
subtitle: using Python and Beautiful Soup
tags: [web-scraping, python]
image: /img/alana-harris-LH_Q8DBkgZ0-unsplash.jpg
---

# Background
Earlier this week, we built the minimum viable product to format a flat text file (such as a jMeter output) into fully formatted spreadsheets. However, we can do a bit better than that and the client asked for an additional two layers of analysis on the banned term crawl. So, this post covers pulling the context (typically the entire paragraph) for a banned/prohibited term from a webpage and adding that to the spreadsheet output. 

## Dependencies
Last week, we only needed the pandas library. This week, we are adding two libraries. We need the requests library to request the page (urllib or another library could be used instead), and the Beautiful Soup library to easily read and search the webpage result. 
```
import pandas
import requests
from bs4 import Beautiful Soup
```
Depending on your project, you may want to add `import re` to use regex, but this project is strictly looking for exact phrase matches so regex is not required. 

# Process Steps 
This week, we are adding a few extra steps to the formatting, as well as the process to scrape the context from the webpage. 
1. Scrape the context of the banned term and add it to the output
2. Check the context against an allowed phrase list 
3. Print out counter increment for each banned term pull 
4. Add a summary table to easily pull high-level totals into a report 

## Scrape Context
For the past few months, we have strictly been providing the banned term for each URL. The client recently decided to upgrade this project by pulling the context for each term (which is essentially the paragraph containing the banned term) so it is easier for them to find and remedy violating pages. 

So, I put together a small function to pull a URL's response content using the requests library, search the response content for the banned term, and then pull the surrounding text into a "Banned Term Context" column. 

```
def find_term_context(url, term): 
  response = request.open("GET", url)
  soup = BeautifulSoup(response.content)
  term_context = soup.findall(term)
  return term_context 
```

## Verify Context
Now that we have the process to pull the context for each term, we can improve it further by checking the context for an allowed phrase. For example, "wild animal" may be a banned term, unless it is within the phrase "see wild animals at the Toronto zoo". So we can just add a few lines to the function above in order to check the output. 


```
allowed_phrases = ["A", "B", "C"]
def find_term_context(url, term): 
  response = request.open("GET", url)
  soup = BeautifulSoup(response.content)
  term_context = soup.findall(term)
  if(term_context.contains(allowed_phrases)):
    return "False Positive. Allowed phrase found."
  else: 
    return term_context 
```

## Print Counter
Since we are using `dataframe.apply()` for a weighty function (each iteration of the function takes a few seconds), I wanted some way to provide feedback on which row of the dataframe was being processed. A full progress bar was not necessary and would take longer to implement, but printing one line for hundreds of rows can be tedious. So I chose the middle ground of updating the print statement with the current row number. 
```
print("Currently pulling banned term context for row number: 0000"
def find_term_context(url, term): 
  print("\b\b\b\b{4d}".format(row.index)
```

## Summarize Output
This last formatting update slightly improves the user experience by pulling the number of rows for each frame into a summary dictionary and printing it to the screen. It is also roughly added to a dataframe and saved to the same export path so it can be referenced in the future if needed. 
```
summary_data = {
  'Total number of unique errors found': len(errors_frame), 
  'Total number of unique unavailable links found': len(unavailable_frame), 
  'Total number of banned terms found': len(banned_frame)
}
summary_frame = pandas.dataframe(data=summary_data)
summary_frame.T
```

# Summary 
That's it! With the example above, you can pull context for a term from a webpage and verify it against a list of allowed phrases. 
