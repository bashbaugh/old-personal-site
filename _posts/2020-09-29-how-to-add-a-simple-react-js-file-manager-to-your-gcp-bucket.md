---
layout: post
section-type: post
date: 2020-09-29
lastmod: 2020-09-29
title: How to Add a Simple React.js File Manager to your GCP bucket
slug: gcp-cdn-file-dashboard
url_slug: gcp-cdn-file-dashboard
category: tutorials
tags:
  - javascript
  - project
  - web-dev
  - tutorial
  - clouds
  - code
postdescription: Using React.js, create a simple file dashboard using a Cloud
  Functions API, and host it on your bucket.
titletag: How to Add a Simple React.js File Manager to your GCP bucket
description: Using React.js, create a simple file dashboard for managing Google
  Cloud Storage CDN files using a Cloud Functions API, and host it on your
  bucket.
---
In the [previous tutorial](/tutorials/gcp-cdn), we set up a Google Storage bucket with a load balancer and custom subdomain to use as a personal CDN. In this tutorial, we will be making a file management dashboard for our storage bucket, as a simpler alternative to the GCP dashboard.

If you aren't interested in the code and just want to set up the dashboard, skip to the end of this tutorial. [GitHub Repo](https://github.com/scitronboy/cloud-storage-dashboard)

I should also mention that we could also do all this using Google's Firebase Storage, however, doing it manually allows for slightly more flexibility and customization of the storage bucket and CDN setup, for example (see the [previous tutorial](/tutorials/gcp-cdn)). Also, Firebase uses the same GCP components (storage, cloud functions, etc.) behind the scenes.

#### Introduction

1. We will create a single-page static react app
2. We will use [Google Sign-In](https://developers.google.com/identity/sign-in/web/sign-in) to authenticate users in our web app
3. We will create some serverless Node.js functions that run on Google Cloud Functions, and use the Google Storage APIs to interact with our cloud bucket.
4. We will host this webapp in our bucket and rewrite the root URL of our CDN ([cdn.benjaminashbaugh.me](https://cdn.benjaminashbaugh.me/)) to the web app, so that we can easily access it.

### Setting up cloud functions

Before deploying cloud functions, make sure you [enable the Cloud Build API](https://console.cloud.google.com/marketplace/product/google/cloudbuild.googleapis.com)

