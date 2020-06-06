---
title: Improving Your Site's Accessibility Score
subtitle: Using Lighthouse
published: false
---

Today, we will cover an often overlooked, but extremely important aspect of web performance: accessibility. Over 4 million people use screen readers when accessing websites in the United States, and simple modifications to your website make a big difference on their experience. This article obviously cannot be a full review of website accessibility, but instead will focus on the most prominent improvements: alt-text for images, tabbing between elements, and color contrast. 

https://web.dev/color-contrast/?utm_source=lighthouse&utm_medium=devtools
https://www.w3.org/TR/2008/NOTE-WCAG20-TECHS-20081211/working-examples/G183/link-contrast.html

updated font color for post-meta and a links in general under main.css and config.yml

Post-Meta

```CSS
.post-preview .post-meta,
.post-heading .post-meta {
  color: #666666;
  font-size: 18px;
  font-style: italic;
  margin: 0 0 10px;
}
```

Links 
```CSS
a {
  color: {{ site.link-col }};
}
a:hover,
a:focus {
  color: {{ site.hover-col }};
}
```

Config.yml 
```
# Personalize the colors in your website. Colour values can be any valid CSS colour

navbar-col: "#F5F5F5"
navbar-text-col: "#404040"
navbar-children-col: "#F5F5F5"
page-col: "#FFFFFF"
link-col: "#0066CC"
hover-col: "#0085A1"
footer-col: "#F5F5F5"
footer-text-col: "#666666"
footer-link-col: "#404040"
```

Config.yml previously ```footer-text-col: "#777777"``` and previously ```link-col: "#008AFF"```

may also update blog tags and theme-by and copyright-by text

Added alt text to some images to clear this up as well 

## Before 

![Lighthouse Accessibility Score Before Improvements](/img/lighthouse_accessibility_improvements_before.png "Accessibility Score Before Improvements")

## After

![Lighthouse Accessibility Score After Improvements](/img/lighthouse_accessibility_improvements_after_2.png "Accessibility Score After Improvements")

![Lighthouse Accessibility Audits After Improvements](/img/lighthouse_accessibility_improvements_after.png "Accessibility Audits After Improvements")


https://ux.stackexchange.com/questions/57340/percentage-of-screen-readers-users-in-usa#:~:text=Someone%20wrote%20a%20very%20detailed%20article%20as%20to%20why.&text=So%2088.5%25%20of%20326%20million,cannot%20see%20but%20are%20online.
