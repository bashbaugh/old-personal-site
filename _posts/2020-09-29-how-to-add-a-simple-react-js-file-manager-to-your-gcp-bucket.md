---
layout: post
section-type: post
date: 2020-11-18
lastmod: 2020-11-18
title: Adding a Lightweight React.js File Management Dashboard to your GCP
  Storage Bucket & CDN
top-image: /img/uploads/filemanager1.png
slug: gcp-cdn-file-dashboard
url_slug: gcp-cdn-file-dashboard
category: code
tags:
  - javascript
  - project
  - web-dev
  - tutorial
  - clouds
  - code
postdescription: How to add my open-source lightweight React.js file management
  dashboard to your Google Cloud Storage bucket and CDN.  This is the sequel to
  my previous article on setting up a simple CDN with google cloud.
titletag: Adding a Lightweight React.js File Management Dashboard to your GCP
  Storage Bucket & CDN
description: How to add my open-source lightweight React.js file management
  dashboard to your Google Cloud Storage bucket and CDN.  This is the sequel to
  my previous article on setting up a simple CDN with google cloud.
---
In the [previous tutorial](/tutorials/gcp-cdn), I showed you how to set up a Google Cloud Storage bucket with a load balancer, cloud CDN (or Cloudflare) and custom subdomain to use as a personal CDN and file storage system. In this tutorial, I will show you how to add my [lightweight open-source file management dashboard](https://github.com/scitronboy/cloud-storage-dashboard) to your CDN.

### Introduction

Before starting, make sure you have node (at least version 10) and yarn (or NPM) installed, so that you can build the app to deploy to your CDN.

## The React App

## Setting it up in GCP

### Authentication

The google sign-in plugin in the web app requires users to sign into their google account, obtaining an OAuth ID token that gets sent to the cloud functions API where it's used to verify the user's email. In order for it to work we need to register an OAuth client on GCP.

1. First, create an OAuth Client ID. You can do this by navigating to the [API credentials](https://console.cloud.google.com/apis/credentials) page -> Create Credentials -> OAuth Client ID. You will be required to create a sign-in consent screen as well. You don't really have to worry about messing any of this up, as you're the only one who (should) be accessing the app.
2. Choose "external user" type, enter a name like "my CDN dashboard", and enter the domain where you want to host the dashboard for all the required URL fields. ** I recommend using another domain for the dashboard** that's separate from the CDN domain, so that files and caching rules on your CDN don't interfere with the dashboard. **Do not** add any extra scopes, an icon, or extra domains, as that would necessitate a review by Google.
3. Once you create the consent screen, you might have to choose "Create Credentials" again to get to the application creation page.
4. Choose web application and add your dashboard domain to the "Authorized JavaScript Origins". Under "Authorized redirect URIs," add the URL to the page you want your file dashboard to be hosted at. This might be the root of a subdomain, or it might be at `/admin`, for example. Finally, click "create". You will need your client ID for the next steps.

### Adding the Cloud Functions

Google's Cloud Functions are the ideal place to host the API for our file manager, as the Cloud Storage Node library will automatically connect to our bucket when hosted on Cloud Functions (in the same project).

Before deploying cloud functions, make sure you [enable the Cloud Build API](https://console.cloud.google.com/marketplace/product/google/cloudbuild.googleapis.com), the [Cloud Storage API](https://console.cloud.google.com/flows/enableapi?apiid=storage-api.googleapis.com), and the [IAM service account credentials API](https://console.developers.google.com/apis/library/iamcredentials.googleapis.com). These will allow your functions to build in the cloud and access the cloud storage API.

Additionally, you may need to add the Service Account Token Creator role to the service account used by your cloud functions. This will give them permission to generate signed URLs for file uploads and downloads. To do this, visit the [IAM (identity and access management) page](https://console.cloud.google.com/iam-admin/iam) in the cloud console. In the list of service accounts, search for the account named "App engine default service account". Then, select "add" at the top of the page and under "New members", start typing and select the account you just found. This is the account Cloud Functions will use to sign URLs. Finally, select `Service Account Token Creator` for the role and click save.

![The IAM page](/img/uploads/iam-tokencreator.png "The IAM page")

As of the first version of the Cloud Storage dashboard, there is only one function that you'll need to upload to Cloud Functions. 

#### Environment Variables

+ `OAUTH_CLIENT_ID` - Your app's OAuth client ID.
+ `CDN_ADMIN` - The email of your google account. You will be able to add additional CDN admins through the settings page in the app later.
+ `CDN_BUCKET_NAME` - The cloud storage bucket (in the same GCP project) where your CDN's files are stored
+ `CDN_URL` - The base URL to the CDN frontend for your bucket (without an ending slash): `https://cdn.example.com`