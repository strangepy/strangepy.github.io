--- 
layout: post
title: Getting Started with Selenium Testing in Python
subtitle: in ten minutes or less 
image: /img/caspar-camille-rubin-fPkvU7RDmCo-unsplash.jpg
tags: python
--- 
Automated browser testing is incredibly useful. Once you learn how to use it in one project, it is easy to apply to other projects and automate all kinds of tasks. 
For numerous projects, I helped customers by using the full Selenium software or Katalon Studio. However, I found myself wanting to automate browser tests in Python due to the numerous advantages. There is quite a bit of literature to set up selenium on a standard development environment (a Linux flavour and Firefox), but what if you wanted to use a Chromium variant on Windows 10? 
The solution was surprisingly easy! Read the steps below to get started with a basic web search test. 
# Install Selenium 
If you are using a robust IDE, such as PyCharm, then you can install the package using the front-end and then write simply
``` import selenium ``` at the top of your file. Otherwise, typically opening the Command Prompt (in Windows) and running ``` pip install selenium ``` should do the trick. Selenium needs to be installed the first time you do web testing regardless of the Operating System / Browser combination you are using. 
# Download the WebDriver
We will get into the details of WebDrivers later on, but for now you need to be sure you install the proper driver. For Firefox, this is usually geckodriver. You can find the [geckodriver project details here](https://github.com/mozilla/geckodriver) and the [latest release of geckodriver at this link](https://github.com/mozilla/geckodriver/releases/latest). For Chrome, you can install  the Chromium ChromeDriver. You can find the [ChromeDriver project details here](https://chromedriver.chromium.org/) and the [latest version of ChromeDriver here](https://chromedriver.chromium.org/downloads). Whichever browser you choose, be sure to download the proper files for your operating system and architecture (32-bit or 64-bit). Once downloaded, extract the files and move them to a sensible location (typically in the Program Files or Program Files x86 folder). 
One of the reasons that I chose Chromium for this project is that geckodriver lists the [Microsoft Visual Studio redistributable runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) as a dependency for Windows 10. My setup called for a quick, easy install without the potential hassles of Visual Studio, so I decided that Chromium would be easier to set up than Firefox. 
# Add the WebDriver to Your System's PATH 
An optional, but really convenient step, is to add the location of geckodriver.exe or ChromeDriver.exe to your system's PATH. In Windows 10, this involves opening the "Advanced System Settings" page of the control panel, clicking "Environment Variables", and then "New" under the system environment variables settings. From that dialog window, simply paste in the file path to your WebDriver executable file and click "Apply". 
*Note: You may have to restart your computer for this change to take effect. If you are in a hurry, you can skip this step entirely.* 

# Use the WebDriver for the First Time 
Once you have downloaded the proper WebDriver and installed Selenium, you can get started with browser testing! The example below is just to get you started and to verify nothing went wrong with the installation. 
As a side note, if you skipped adding the WebDriver to your PATH variables, then you can easily add on the ```executable_path=``` attribute to the browser call. If you did set up the PATH variable, then that attribute is not necessary. 
```
from selenium import webdriver

# Configure the browser
options = webdriver.ChromeOptions()
options.add_argument('headless')
browser = webdriver.Chrome(executable_path="C:\\Program Files\\Google Chrome\\chromium_chromedriver\\chromedriver.exe", chrome_options=options)

# Open the browser and begin testing 
browser.get("https://www.duckduckgo.com/")
search_form = browser.find_element_by_css_selector('#search_form_homepage_top > input.js-search-input')
search_form.sendKeys('strangePy')
search_form.submit()

# Retrieve the results list and print one to the page 
search_results = browser.find_element_by_css_selector('div.results_links_deep')
print(search_results[0])

# Don't forget to close it out when you are finished!
browser.close()
```
And that's it! We could go through additional examples and dig in further, but I wanted to keep this as lightweight as possible so you can get set up and running quicky! 
