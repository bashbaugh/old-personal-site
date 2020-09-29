---
layout: post
section-type: post
date: 2020-09-29
lastmod: 2020-09-29
title: How to Create a Simple React.js File Manager for your GCP bucket
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
postdescription: Using React.js, create a simple file dashboard for your Google
  Cloud Storage bucket.
titletag: How to Create a Simple React.js File Manager Dashboard for your Cloud
  Storage bucket
description: Using React.js, create a simple file dashboard for managing Google
  Cloud Storage CDN files using a Cloud Functions API, and host it on your
  bucket.
---
In the [previous tutorial](/tutorials/gcp-cdn), we set up a Google Storage bucket with a load balancer and custom subdomain to use as a personal CDN. In this tutorial, we will be making a file management dashboard for our storage bucket, as a simpler alternative to the GCP dashboard.

If you aren't interested in the code and just want to set up the dashboard, skip to the end of this tutorial. [GitHub Repo](https://github.com/scitronboy/cloud-storage-dashboard)

I should also mention that we could also do all this using Google's Firebase Storage, however, doing it manually allows for slightly more flexibility and customization of the storage bucket and CDN setup, for example (see the [previous tutorial](/tutorials/gcp-cdn)). Also, Firebase uses the same GCP components (storage, cloud functions, etc.) behind the scenes.

You will need a basic understanding of React.js in order to follow along (or, you can skip to the last step and just copy the code from the repo).

#### Introduction

1. We will create a single-page static react app, with controls allowing users to upload files and folders and see which files and folders are on the CDN.
2. We will use [Google Sign-In](https://developers.google.com/identity/sign-in/web/sign-in) to authenticate users in our web app
3. We will create some serverless Node.js functions that run on Google Cloud Functions, and use the Google Storage APIs to interact with our cloud bucket.
4. We will host this webapp in our bucket and rewrite the root URL of our CDN ([cdn.mywebsite.com](https://cdn.benjaminashbaugh.me/)) to the web app, so that we can easily access it.

Before starting, make sure you have node (10) and yarn (or NPM) installed.

## The React App

You can download the app [template from my CDN](https://cdn.benjaminashbaugh.me/file-dashboard-template.zip) (or [from github](https://github.com/scitronboy/cloud-storage-dashboard/tree/template). Basically, it's just `create-react-app` plus a few structural changes and a basic [React-Semantic-UI](https://react.semantic-ui.com/) UI with a sidebar and file cards.

Once you unzip it, `cd` into /app and run `yarn install` to install the dependencies, then yarn run to start the app.



## Making the API 

### Setting up cloud functions

_Before deploying cloud functions, make sure you [enable the Cloud Build API](https://console.cloud.google.com/marketplace/product/google/cloudbuild.googleapis.com)._


