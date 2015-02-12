---
layout: post
permalink: /?p=1311
title: "OwnCloud: Creating Your Own Cloud Storage"
category: ["coding", "linux-system-administration", "owncloud-services", "php-coding", "services", "system-administration", "web-servers"]
tags: cloud drive dropbox online-storage owncloud web
---
# What is Cloud Storage?

By now, everyone is familiar with the services [Dropbox](http://db.tt/Yq7Epce3), and their competitor, [Google Drive](http://google.com/drive). Both of these tools are essentially a secure off-site backup drives. Personally, one of the main reasons I would use these services is to automatically back up all my pictures from my phone without needing to manually sync it. In the event the phone is lost or stolen and I haven't manually backed them up, Dropbox and Google Drive would have a copy safe and sound in the cloud.

# Downfalls to Cloud Storage

One of the downsides to using Dropbox and Drive are that they are hosted by Dropbox and Google directly. They are well known providers and simply for that fact are subject to frequent attacks. Your data could be at risk, or looked at by Big Brother. If you have a sense of paranoia over your data, a public solution may not be the right one for you.

# Creating Your OwnCloud

There are many services out there which support personal clouds. The one that stuck out to us was [OwnCloud](http://www.owncloud.com).

OwnCloud has a personal version which is available for download in Linux, Mac and Windows format. It is super easy to install, and works essentially right out of the box.

We will be installing OwnCloud on a Ubuntu box with a custom install of Apache for the sake of demonstration.

**Step 1** : Downloading the Package

To download OwnCloud, go to their [Downloads](https://owncloud.com/download) page. Instead of downloading it through a browser, you can also download it straight to your server using:

    wget http://download.owncloud.com/download/2012.4.5.9/owncloud-2012.4.5.9-enterprise.tar.bz2

The download should complete fairly quick as the package is only ~3.5M.

**Step 2** : Installing the Package

Once you have the file downloaded, you can extract it using:

    tar -xvjf owncloud-2012.4.5.9-enterprise.tar.bz2

If you want to extract it to `/usr/local/` for example, you can do:

    tar -xvjf owncloud-2012.4.5.9-enterprise.tar.bz2 -C /usr/local

**Step 3** : Configuration **Step 4** : Launching for the First Time

