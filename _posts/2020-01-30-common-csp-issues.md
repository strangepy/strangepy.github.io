--- 
title: Common issues with a CSP manager 
subtitle: and effective remedies 
tags: [web-performance]
--- 

# Sampled Data 
I intend to write a full post about verifying that all of your third party providers do not sample data or at least tell you that they sample data. But this is a common issue with CSPs. A provider will tell you that they collect 100% of page views and think that is the end of the story. But CSP violations are typically reported as JavaScript errors in the browser console. Is the provider equipped to collect those errors? Do they collect those errors at 100%? Do they parse the errors out and make them easier to read? 

# Version Control 
Some providers do not have integrated version control in their CSPs, so you may end up deploying a version that is not ready. This is not super common, but I have seen it happen. 

# Broken Functionality 
One customer ended up wanting to set up a CSP and was getting errors for an approved domain. They could not understand why that domain was getting blocked. It turns out that the domain was serving some content over HTTP instead of HTTPS. They had set HTTP requests to "allow" in their third party's CSP manager, but apparently that setting did not actually work since it was not fully tested. You should have a set of unit tests and a developer verify functionality in a CSP and perform regression tests on new versions. 
