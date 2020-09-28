---
layout: post
section-type: post
date: 2020-09-28
lastmod: 2020-09-28
title: How to Set Up Your Own Personal CDN & "Cloud" using GCP
slug: personal-gcp-cdn
url_slug: personal-gcp-cdn
category: tutorials
tags:
  - tutorial
  - javascript
postdescription: In this tutorial, I will show how to easily set up your own
  "cloud" for file hosting, using Google Cloud Storage and network services. I
  will also explain how to create a simple dashboard for it using firebase.
titletag: How to Set Up Your Own Personal CDN & Cloud using Google Cloud Storage
  and load balancing.
description: In this tutorial, I will show how to easily set up your own "cloud"
  for file hosting, using Google Cloud Storage and network services. I will also
  explain how to create a simple dashboard for it.
---
Recently, it occurred to me that it would be cool to have my own CDN subdomain and file sharing service. Of course, there are many easier options for hosting your files online, such as using something like Google Drive or one of the hundreds of free file sharing websites and services out there. However, having a personal cloud with only your own files and sending people links to "cdn.mydomain.com/file.txt" has got to be much cooler than sending people links to "sketchyfilewebsite.com/hashthatnoonewillremember.txt".

In this tutorial, we will:

1. Create a storage bucket with Google Cloud storage. This could also be done with Amazon s3 or another cloud storage service, but in this tutorial I will use Google Cloud storage.
2. (optionally) Set up a static IP, load balancing and cloud cdn to turn our bucket instance into an actual CDN
3. (optionally) Use Firebase and Node.js to create a very simple dashboard to upload and manage files (alternatively, you could just use the GCP dashboard).