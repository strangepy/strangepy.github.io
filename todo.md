---
layout: page
title: To Do
subtitle: Notes
type: info
---

## Posts
- Second half of https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-01-08-weighted-domain-report.md
- Second half of https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-01-23-scraping-highcharts-data.md
- Additional info https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-01-30-common-csp-issues.md
- Most of https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-02-05-DEV-percentile-disc-cont.md
- Code examples https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-02-13-navigation-link-tests.md
- Code examples https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-02-26-object-prototype-conflicts.md
- Few code examples https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-03-03-flat-file-to-dataframe.md
- Most of https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-03-29-get-your-site-on-google.md
- Code examples https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-04-01-selenium-scraping-class.md
- Most of https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-04-08-image-timestamp-preprocess.md
- Testing https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-05-01-defer-offscreen-images.md
- Additional info https://github.com/strangepy/strangepy.github.io/blob/master/_posts/2020-05-15-creating-requests-class.md

## Other
- New post for multithreading selenium class 
- New post for requests class
- Post for API client? 

## Unpublished Posts 
<div class="aside">
<div class="posts-list">
  {% for post in paginator.posts %}
  {% if post.published == false %} 
    <a href="{{ post.url | relative_url }}">
	  <h2 class="post-title">{{ post.title }}</h2>
    </a>
    <p class="post-meta">
      Posted on {{ post.date | date: site.date_format }}
    </p>
  {% endif %}
  {% endfor %}
</div>
</div>
