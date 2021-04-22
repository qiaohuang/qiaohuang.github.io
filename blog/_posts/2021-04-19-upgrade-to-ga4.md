---
layout: post
title: "Upgrade to Google Analytics 4"
category: Blog
comments: true
---
I just upgraded to Google Analytics 4 – the future of analytics! 🔮

In this post, I'll share a little of my experience.

## Introduction

Google Analytics announced [a new way to measure apps and websites together](https://blog.google/products/marketingplatform/analytics/new-way-unify-app-and-website-measurement-google-analytics/) in Jul 2019 and launched [the next generation Google Analytics](https://blog.google/products/marketingplatform/analytics/new_google_analytics/) in Oct 2020. Unlike the old University Analytics ([UA](https://support.google.com/analytics/answer/10220206)) properties, the new Google Analytics 4 ([GA4](https://support.google.com/analytics/answer/10089681)) properties can be used for a website, an app, or both together. GA4 is built with machine learning at its core to help deliver new insights and comes with better privacy handling.

## Prerequisites

- A Google Analytics Account - Log in to Google account and go to <https://analytics.google.com/>
- A Universal Analytics Property - [Set up Analytics for a website](https://support.google.com/analytics/answer/10269537)

## Upgrade

### Upgrade to GA4 using [Setup Assistant](https://support.google.com/analytics/answer/10312255)
- You can find **GA4 Setup Assistant** within the Admin console of your Universal Analytics property.
- Get started with Google Analytics 4 by auto-creating a new property using the wizard. Click **See your GA4 property**.
- Appears in the Admin section under the Property column, the [[GA4] Setup Assistant](https://support.google.com/analytics/answer/10110290) allows you to further customize your GA4 property and share settings from your Universal Analytics property.

### Get your GA4 global site tag ([gtag.js](https://support.google.com/analytics/answer/10220869))

- `GA4 Admin` --> `Data Streams` --> `Click your stream` --> `Add new on-page tag`

![gtag.js](/blog/assets/images/gtag.png)

### Add your tag directly to your web pages

- Copy the entire Analytics page tag into the `<head>` section of your HTML. It should look like [this](https://github.com/qiaohuang/qiaohuang.github.io/blob/master/_includes/google-analytics.html "This link takes you to my google-analytics source code").

```javascript
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-XXXXXXX');
</script>
```

The `gtag.js` tag that you added will collect data for your new GA4 property, which is linked to your Universal Analytics property. You now run the UA & GA4 in Parallel!

{: .box-warning}
Warning: Don't remove the old `analytics.js` tag. It will continue to collect data for your Universal Analytics property.

## Postscript

This simplified guide showed the GA4 upgrade configuration in an existing UA within a few steps. You can also use Google Tag Manager ([GTM](https://support.google.com/tagmanager/answer/6102821)) to implement a GA4 property, which would be a robust method. If you're new to Google Analytics and want to [set up your first Analytics](https://support.google.com/analytics/answer/9306384), it will automatically be GA4.

Check out [Google's walkthrough](https://support.google.com/analytics/answer/9744165) to learn more about adding a GA4 property. If you want to explore more, try the Google Analytics [demo account](https://support.google.com/analytics/answer/6367342) for free. For a more in-depth understanding of your new property type, take the [Google Analytics Skillshop course](https://skillshop.exceedlms.com/student/path/66729/).

If you're facing any issues in your upgrade or find any mistakes in this post, feel free to comment here. Thanks for reading. Cheers! 🍻