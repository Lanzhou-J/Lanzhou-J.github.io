---
title: Set Up Google Analytics (Universal Analytics & GA4) for my tech blog
layout: post
subtitle: null
date: '2021-06-29 20:04:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- blog
- data
- google
- English
---

### What is Google Analytics?

![GoogleAnalytics](/img/in-post/google_analytics.png)

Google Analytics is a platform that can help developers and business owners to collect and analyze data. Take my tech blog as an example. I am most interested in data related to:

- Which article has the highest click-through rate? 
- People in which area are most interested in my article? 
- Which page did my readers spend most time on? 

For business owners running online business, the data that they are interested in might be: 

- Which product page gets the most clicks? 
- Which is the best selling product? 
- Which is the most popular referral site of the online store? 

All these data can be collected through Google Analytics, and generate reports for display. 

### Why choose Google Analytics?

Because it is free!
Google offers both free and paid version of Google Analytics. Small businesses (or my tech blog) can choose the service without paying any recurring fee. But if users want more advanced services, they need to choose premium version Google Analytics 360.

### Universal Analytics(GA3) or GA4?
Google Analytics 4 is not an upgrade of Universal Analytics. It is a completely new version of Google analytics with different sets of reports.
Google Analytics 4 is the future of Google Analytics!

*5 Key Advantages of GA4 compared with GA3:*
1. Machine learning -- GA4 can generate automatic insights and predictable insights based on Google's new machine learning technology.
2. Cross-device/platform -- More robust and reliable cross-device and cross-platform tracking.
3. Event tracking automation -- GA4 automatically tracks for certain types of events (e.g. scroll, video engagement, site search etc.).
4. BigQuery -- GA4 comes with a Free connection to BigQuery(Cloud data warehouse).
5. Built-in IP anonymization feature -- GA4 pays more attention to end user privacy.

However, there are also comments showing that the user interface and data reports of GA4 are more complicated. For users who only host a website without an APP, some new features are not useful to them at all. So if you are already familiar with GA3 user interface, I would suggest you think twice before replacing GA3 with GA4. In summary, whether you want to replace GA3 with the new version GA4 depends on your personal needs and interests.

### Set up Google analytics for the blog

For learning purpose I set both GA4 and GA3 for this blog. 

#### Set up GA4

I first set up GA4 following the guide in this article: [[GA4] Set up Analytics for a website and/or app](https://support.google.com/analytics/answer/9304153).

On the website -- [https://marketingplatform.google.com/about/analytics/](https://marketingplatform.google.com/about/analytics/), click 'Start for free'.

1. Create an Analytics account
2. Create a Google Analytics 4 property
3. Add a data stream
4. Set up data collection
  - Global site tag(gtag.js) can be found under "Tagging Instructions".
  - Example:
    1. Add the tag in footer.html file
        ```html
        <!-- Global site tag (gtag.js) - Google Analytics -->
        <script async src="https://www.googletagmanager.com/gtag/js?id=site.ga_measurement_id"></script>
        <script>
          window.dataLayer = window.dataLayer || [];
          function gtag(){dataLayer.push(arguments);}
          gtag('js', new Date());

          gtag('config', "site.ga_measurement_id");
        </script>
        ```
       （Wrap ```site.ga_measurement_id``` with double curly bracket.） 
    2. Add GA4 measurement ID in config YAML file.
      ```
      # Google Analytics GA4
      ga_measurement_id: 'G-XXXXXXXXXX'
      ```
5. Set up IP address filters (optional)
  - IP adress filters can help you exclude internal traffic.
  - Reference article: [How To Set Up Address Filters in Google Analytics 4(GA4)](https://witneyseoguy.co.uk/blog/set-up-ip-address-filters-in-google-analytics-4/)
    1. Define your internal IP address in 'Admin' - 'Data Streams' - 'More Tagging Settings' - 'Define internal traffic' - 'Create'.
    ![GoogleAnalyticsIPRule](/img/in-post/google_analytics_IPRule.png)
    2. Create IP filter in 'Admin' - 'Data Settings' - 'Data Filters' - 'Create Filter' - 'Internal Traffic' - set 'Testing' Filter state - 'Create'.
    3. Test the IP filter in 'Reports' - 'Real-time'.
    4. Activate the IP filter - 'Save'.

GA4 user interface:
 ![GoogleAnalyticsUI](/img/in-post/google_analytics_UI.png)


#### Set up Universal Analytics (GA3)
After setting up GA4, I set up Universal Analytics and its views with filters under the same Analytics Account following these tutorials: [Google Analytics for Beginners: 1.3 Google Analytics setup](https://analytics.google.com/analytics/academy/course/6/unit/1/lesson/3), [Google Analytics for Beginners: 1.4 How to set up views with filters](https://analytics.google.com/analytics/academy/course/6/unit/1/lesson/4).

You can also follow the instructions in this article: [Set up Analytics for a website (Universal Analytics)](https://support.google.com/analytics/answer/10269537?hl=en)

GA3 user interface:
 ![GoogleAnalyticsGA3UI](/img/in-post/google_analytics_GA3UI.png)

