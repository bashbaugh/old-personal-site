---
layout: post
section-type: post
date: 2020-09-28
lastmod: 2020-09-28
title: How to Set Up Your Own Personal CDN & "Cloud" using GCP and Cloudflare
image: /img/uploads/cdn_xml_thumbnail.png
slug: gcp-cdn
url_slug: gcp-cdn
category: tutorials
tags:
  - tutorial
  - javascript
  - clouds
postdescription: In this tutorial, I will show how to easily set up your own
  "cloud" for file hosting, using Google Cloud Storage, network services and
  Cloudflare. I will also explain how to create a simple dashboard for it using
  Node.js.
titletag: How to Set Up Your Own Personal CDN & Cloud using Google Cloud Storage
  and Load Balancing.
description: In this tutorial, I will show how to easily set up your own "cloud"
  for file hosting, using Google Cloud Storage, network services and Cloudflare.
  I will also explain how to create a simple dashboard for it using Node.js.
---
Recently, it occurred to me that it would be cool to have my own CDN subdomain and file sharing service. Of course, there are many easier options for hosting your files online, such as using something like Google Drive or one of the hundreds of free file sharing websites and services out there. However, having a personal cloud with only your own files and sending people links to [cdn.mydomain.com/file.txt](https://cdn.benjaminashbaugh.me/hello.txt) has got to be much cooler than sending people links to "sketchyfilewebsite.com/file/hashthatnoonewillremember.txt". Also, using the CDN layer will also allow you to efficiently host things such as images and JavaScript libraries for your websites, etc. Fortunately, it's super easy to set one up!

In this tutorial, we will:

1. Create a storage bucket with Google Cloud storage. This could also be done with Amazon s3 or another cloud storage service, but in this tutorial I will use Google Cloud storage.
2. (optionally) Set up a static IP, load balancing and cloud cdn to turn our bucket instance into an actual CDN. **OR** Connect our bucket to Cloudflare and use their CDN.
3. (optionally) Add our CDN/storage bucket to a custom subdomain or domain.
4. (optionally; coming soon) In the next part, use Node.js to create a very simple dashboard to upload and manage files. 

You will need an active [Google Cloud Platform](https://console.cloud.google.com/) account to follow along. Before you begin, I would recommend that you create a new project (from the dropdown in the top menu bar) to isolate the cloud storage and networking instances from any other GCP projects you might have.

If you run into any issues or have any questions throughout this tutorial, make sure to leave a comment down below and I'll try to reply within a day or two.

## Creating a Cloud Storage Bucket.

Before we start, I should mention pricing. [Pricing for google cloud platform storage](https://cloud.google.com/storage/pricing) buckets is very cheap - it costs about $0.02 USD per gigabyte per month, plus networking costs which are unlikely to be more than a few cents per month (unless you're using your CDN for a hugely popular website with hundreds of thousands of downloads per month). And if you choose a single region, the first few gigabytes stored every month will be free. If you are worried about costs, you can set up threshold alerts under the billing tab in GCP.

In the GCP console, navigate to Products -> Storage -> Storage -> Browser. Click on "Create Bucket".

1. Choose a unique name for your bucket. **Important:** If you DO want to use a custom subdomain but don't care about CDN or HTTPS (and won't be using a load balancer) (see the next step), you HAVE to set the bucket name to that of the subdomain you will be using; for example `cdn.mywebsite.com`. You will also have to verify your ownership of the domain, [as described here](https://cloud.google.com/storage/docs/domain-name-verification#verification).
2. You can choose multi-region if you want to, but especially if you plan to add a load balancing CDN then this isn't really necesarry so you can just choose a single region that's closest to where you're located. 
3. Choose the standard storage class
4. For now, choose Uniform access control. If you want to add complex permissions later you can always change this setting within 90 days.
5. Make sure you've selected a google-managed key and finish by selecting "create bucket"!

Now we need to make it so that anyone can read from the bucket. Under the permissions tab for your newly created bucket, add a new permission. Choose "allUsers" under "New Members" and select "Storage Object Viewer" under role.

![Adding new permissions](/img/uploads/2020-09-28_13-28.png "Adding the new permission")

Congratulations! You've set up your first Cloud Storage bucket! If you navigate back to the Objects tab and select "upload files", you'll see a new entry appear with an option to "copy URL". Click this, open a new tab, and paste it into your address bar to visit the file you just uploaded! The URL should look something like <https://storage.googleapis.com/cdn-workshop/test.txt>. I would suggest bookmarking your bucket dashboard in your browser so you can easily access it.

![](/img/uploads/2020-09-28_13-33.png)

You could stop here if you wanted to - we now have a fully functioning storage bucket with the default GCP dashboard for uploading files and getting links. In the next step, we will go over setting up load balancing and a static IP, so that we can turn our storage bucket into an official CDN and add it to a subdomain on a website.

## Setting up load balancing + a CDN layer

*Note: **this step and all steps after this require you to have a domain you can use for your CDN**. I use my personal domain at cdn.benjaminashbaugh.me.*

**Pricing**: The [pricing for this step](https://cloud.google.com/vpc/network-pricing) is relatively expensive, and costs about $2.90 USD at minimum per month for the static IP and [CDN costs](https://cloud.google.com/cdn/pricing), plus about $18 a month for the [load balancing](https://cloud.google.com/vpc/network-pricing#lb). I'm not completely sure why Google doesn't offer a free tier on load balancing, but if you're just making a personal CDN and this is too expensive for you, you an either skip the CDN layer altogether and just access your bucket through `storage.googleapis.com`, or you can use a CNAME record to connect your subdomain across HTTP, or you can route your bucket through Cloudflare's (free) CDN (as described in the next section) - which [won't perform quite as well as the GCP CDN](https://www.cdnperf.com/), but will still be ideal especially for static website content.

To set up the Google Cloud CDN and load balancer, navigate to Networking -> Network Services -> Load Balancing. Select "create load balancer" and choose "HTTP(s) load balancing".

1. Choose "From internet to my VMs"
2. Give the load balancer a name and choose "backend configuration"
3. Then choose "create or select..." -> "backend buckets" -> "create a backend bucket" and select the bucket we created last step. The key here is to select "Enable cloud CDN"!

![Setting up the CDN backend](/img/uploads/2020-09-28_14-05.png "CDN backend")

Leave "host and path rules" untouched for now, but in "frontend configuration" we need to set up a static IP for our CDN, so that we can connect it to our website. Under "New frontend IP and port", name it something like "cdn-https" and select HTTPS for the protocol. Choose whichever network service tier you want, and under "IP address" reserve a new static IP. Under Certificate, choose "Create a new certificate" and select "Create google-managed certificate". Make sure you enter the domain/subdomain(s) you want to host your CDN at, and Google will automatically provision an SSL certificate for it. 

![Frontend setup](/img/uploads/cdn_frontend_setup.png "Frontend setup")

You can create a second frontend configuration, if you wish, using the same IP but HTTP instead of HTTPS, if you want your CDN to be accessible from HTTP as well.

Finally, click "create" to finish setting up your load balancer! 

Note that you will get an error message if you visit `http://CDN-IP` if you have not set up an HTTP frontend, and visiting `https://CDN-IP` will also give an error (`ERR_SSL_VERSION_OR_CIPHER_MISMATCH`) initially, because it takes about 20 minutes to provision the SSL certificate. Even after the SSL cert is provisioned, you might still get a warning because you're visiting the site through the IP rather than the domain listed in the certificate.

Next up - adding the CDN to a custom subdomain!

## Adding DNS entries to your subdomain.

In this step, we will add DNS entries for our CDN so that we can access it from a subdomain on our website. 

Navigate to your domain's DNS record settings (if you haven't changed the default nameservers for your domain, you should find these in the control panel of your domain registrar).

### If you are using the load balancer

Create a new `A` record. The name should be the same domain or subdomain that you used when creating your SSL certificate. The value should be set to the static IP you reserved for your load balancer (**without the port**). Wait a few minutes for the DNS changes to propagate, then you should be able to visit the subdomain in your browser. Make sure you add `https://` to the beginning of the URL in order to access the secure version of your CDN!

![DNS settings](/img/uploads/cdn-dns-settings.png "DNS settings")

### [If you aren't using a load balancer](https://cloud.google.com/storage/docs/request-endpoints#cname)

Create a new `CNAME` record. The name should be the subdomain you want to host your CDN at (this should be the same as your bucket name). Set the value to `c.storage.googleapis.com.` (note the final dot). Wait a few minutes for the DNS changes to propagate, then you should be able to visit the subdomain in your browser. 

![CNAME DNS settings](/img/uploads/cdn-cname-dns-settings.png "DNS settings")

**After adding your DNS record**, you should see an XML document listing all the files you have uploaded to your storage bucket. You can access them by appending a filename to the URL in your browser, for example [https://cdn.mywebsite.com/hello.txt](https://cdn.benjaminashbaugh.me/hello.txt)

![The XML index](/img/uploads/cdn_xml_thumbnail.png "The XML index")

Congratulations! You've now added your CDN to a custom subdomain on your website!

Hopefully you didn't run into any difficulties setting up your very own CDN (if you did, leave a comment). In the next part, (coming in a few days hopefully) I'll explain how to set up a simple Node.js dashboard and add better forwarding rules to your load balancer. Until then, have fun uploading files to your CDN through the GCP dashboard!