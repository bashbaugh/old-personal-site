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
  - clouds
postdescription: In this tutorial, I will show how to easily set up your own
  "cloud" for file hosting, using Google Cloud Storage and network services. I
  will also explain how to create a simple dashboard for it using firebase.
titletag: How to Set Up Your Own Personal CDN & Cloud using Google Cloud Storage
  and load balancing.
description: In this tutorial, I will show how to easily set up your own "cloud"
  for file hosting, using Google Cloud Storage and network services. I will also
  explain how to create a simple dashboard for it.
---
Recently, it occurred to me that it would be cool to have my own CDN subdomain and file sharing service. Of course, there are many easier options for hosting your files online, such as using something like Google Drive or one of the hundreds of free file sharing websites and services out there. However, having a personal cloud with only your own files and sending people links to [cdn.mydomain.com/file.txt](https://cdn.benjaminashbaugh.me/hello.txt) has got to be much cooler than sending people links to "sketchyfilewebsite.com/file/hashthatnoonewillremember.txt". Fortunately, it's super easy to set one up!

In this tutorial, we will:

1. Create a storage bucket with Google Cloud storage. This could also be done with Amazon s3 or another cloud storage service, but in this tutorial I will use Google Cloud storage.
2. (optionally) Set up a static IP, load balancing and cloud cdn to turn our bucket instance into an actual CDN.
3. (optionally) Use Firebase and Node.js to create a very simple dashboard to upload and manage files (alternatively, you could just use the GCP dashboard).

You will need an active [Google Cloud Platform](https://console.cloud.google.com/) account to follow along. Before you begin, I would recommend that you create a new project (from the dropdown in the top menu bar) to isolate the cloud storage and networking instances from any other GCP projects you might have.

## Creating a Cloud Storage Bucket.

Before we start, I should talk about pricing. [Pricing for google cloud platform storage](https://cloud.google.com/storage/pricing) buckets are very cheap - it costs about $0.02 USD per gigabyte per month, plus networking costs which are unlikely to be more than a few cents per month (unless you're using your CDN for a hugely popular website with hundreds of thousands of downloads per month). And if you choose a single region, the first few gigabytes stored every month will be free. If you are worried about costs, you can set up threshold alerts under the 

In the GCP console, navigate to Products -> Storage -> Storage -> Browser. Click on "Create Bucket".

1. Choose a unique, but short, name for your bucket.
2. You can choose multi-region if you want to, but especially if you plan to add a load balancing CDN then this isn't really necesarry so you can just choose a single region that's closest to where you're located. 
3. Choose the standard storage class
4. For now, choose Uniform access control. If you want to add complex permissions later you can always change this setting within 90 days.
5. Make sure you've selected a google-managed key and finish by selecting "create bucket"!

Now we need to make it so that anyone can read from the bucket. Under the permissions tab for your newly created bucket, add a new permission. Choose "allUsers" under "New Members" and select "Storage Object Viewer" under role.

![Adding new permissions](/img/uploads/2020-09-28_13-28.png "Adding the new permission")

Congratulations! You've set up your first Cloud Storage bucket! If you navigate back to the Objects tab and select "upload files", you'll see a new entry appear with an option to "copy URL". Click this, open a new tab, and paste it into your address bar to visit the file you just uploaded! The URL should look something like <https://storage.googleapis.com/cdn-workshop/test.txt>. I would suggest bookmarking your bucket dashboard in your browser so you can easily find it.

![](/img/uploads/2020-09-28_13-33.png)

You could stop here if you wanted to - we now have a fully functioning storage bucket with the default GCP dashboard for uploading files and getting links. In the next step, we will go over setting up load balancing and a static IP, so that we can turn our storage bucket into an official CDN and add it to a subdomain on a website.

## Setting up load balancing + a CDN layer

GCP provides a very simple CDN layer through their load balancing service. To set it up, navigate to Networking -> Network Services -> Load Balancing. Select "create load balancer" and choose "HTTP(s) load balancing".
