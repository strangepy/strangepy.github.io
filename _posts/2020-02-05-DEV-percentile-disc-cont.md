---
title: Discrete versus Continuous Percentile
subtitle: and how to choose the right one for you 
image: /img/andrej-lisakov-V2OyJtFqEtY-unsplash.jpg
tags: [data-analysis, web-performance]
---

# Background
Before we dive into which method is best for your application, let's first refresh the basics of percentiles. For this project, percentiles had been implemented in one database (Snowflake) and we needed an identical calculation in another database (memSQL). In memSQL, percentiles are window functions. 

# What is a Percentile Distribution? 
text

## Continuous 
Continuous percentile functions assume a continuous distribution. If there are any gaps in the distribution, then the function will perform a linear interpolation. [memSQL documetnation for PERCENTILE_CONT](https://docs.memsql.com/v6.8/reference/sql-reference/window-functions/percentile_cont/)

## Discrete
Discrete percentile functions assume a discrete distribution. If there are any gaps in the distribution, then the function will choose the lowest non-null value within that percentile. [memSQL documentation for PERCENTILE_DISC](https://docs.memsql.com/v6.8/reference/sql-reference/window-functions/percentile_disc/)

# Examples
text

# How to choose the best one for your application
text

