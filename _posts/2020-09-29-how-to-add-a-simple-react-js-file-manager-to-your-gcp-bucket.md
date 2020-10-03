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

You will need a basic understanding of React.js in order to follow along. If you're not interested in the React tutorial or don't know any React, you can just skip to the last section and copy the code from [the GitHub repo](https://github.com/scitronboy/cloud-storage-dashboard).

I should also mention that we could also do all this using Google's Firebase Storage, however, doing it manually allows for slightly more flexibility and customization of the storage bucket and CDN setup, for example (see the [previous tutorial](/tutorials/gcp-cdn)). Also, Firebase uses the same GCP components (storage, cloud functions, etc.) behind the scenes.

#### Introduction

1. We will create a single-page static react app, with controls allowing users to upload files and folders and see which files and folders are on the CDN.
2. We will use [Google Sign-In](https://developers.google.com/identity/sign-in/web/sign-in) to authenticate users in our web app
3. We will create some serverless Node.js functions that run on Google Cloud Functions, and use the Google Storage APIs to interact with our cloud bucket.
4. We will host this webapp in our bucket and rewrite the root URL of our CDN ([cdn.mywebsite.com](https://cdn.benjaminashbaugh.me/)) to the web app, so that we can easily access it.

Before starting, make sure you have node (10) and yarn (or NPM) installed.

## The React App

You can download the app [template from my CDN](https://cdn.benjaminashbaugh.me/file-dashboard-template.zip) (or [from github](https://github.com/scitronboy/cloud-storage-dashboard/tree/template)). Basically, it's just `create-react-app` plus a few structural changes and a basic [React-Semantic-UI](https://react.semantic-ui.com/) UI with a sidebar and file cards.

Once you unzip it, `cd` into /app and run `yarn install` to install the dependencies, then `yarn start` to start the app in development mode.

After starting the app and navigating to [localhost:3000](http://localhost:3000/), you should see an empty dashboard with a sidebar and title:

![The empty dashboard](/img/uploads/dashboard_template.png "Empty dashboard template")

Before we can program the logic for fetching data from the bucket, we have to add authentication. 

## Setting it up in GCP

### Authentication

The google sign-in plugin in our web app will require users to sign into their google account, obtaining an OAuth token for the app to use in it's cloud function requests. The Google sign-in plugin needs to be registered with GCP to work.

1. First, we need to create an OAuth Client ID for the CDN dashboard. You can do this by navigating to the [API credentials](https://console.cloud.google.com/apis/credentials) page -> Create Credentials -> OAuth Client ID. You will be required to create a sign-in consent screen.
2. Choose "external user" type, enter a name like "my CDN dashboard", and enter the domain of your CDN for "authorized domains" and any other required URLs. **Do not** add any extra scopes, an icon, or extra domains, as that would necessitate a review by Google (which might take a while).
3. Once you create the consent screen, you might have to choose "Create Credentials" again to get to the application creation page.
4. Choose web application and add your CDN URL to the Authorized JavaScript origins.
5. Under "Authorized redirect URIs," add the URL to the page you want your file dashboard to be hosted at (on your CDN). This might be the root of your CDN, or it might be at `/admin`, for example - it's your choice. Finally, click "create". Copy your Client ID.

### Adding the Cloud functions

*Before deploying cloud functions, make sure you [enable the Cloud Build API](https://console.cloud.google.com/marketplace/product/google/cloudbuild.googleapis.com), the [Cloud Storage API](https://console.cloud.google.com/flows/enableapi?apiid=storage-api.googleapis.com), and the [IAM service account credentials API](https://console.developers.google.com/apis/library/iamcredentials.googleapis.com). These will allow your functions to build in the cloud, access cloud storage and generate signed upload URLs, respectively.*

Google's Cloud Functions are the ideal place to host the API for our file manager, as the Cloud Storage Node library will automatically connect to our bucket when hosted on Cloud Functions, and Cloud Functions allow us to easily add authentication rules to our functions so that only people signed into the app with an authorized Google account can access them.

#### Authorizing additional users

To allow someone else