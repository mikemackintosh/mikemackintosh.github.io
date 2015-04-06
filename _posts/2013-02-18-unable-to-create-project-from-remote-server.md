---
layout: post
permalink: /unable-to-create-project-from-remote-server
title: "Unable to create project from Remote Server"
category: "Gotcha"
tags: eclipse host ide remote remote-server site5 zend zend-studio
---
### Zend Studio and Remote Servers

Lately, [Zend Studio](http://bit.ly/XZodD6) has been my medium of choice when working on project. I personally feel that many of the small issues and bugs that arise in code due to a missing bracket, semi-colon, mis-typed namespaces or alike can be averted with the automatic error highlighting. The support for PHP5.4.x is also a must have if you have been programming PHP over the past year. Zend Studio is based off the [Eclipse](http://www.eclipse.org/) framework which has already proven itself. There is no comparison from a development standpoint to other tools such as Dreamweaver which I used for many years. Although it was nice to edit files directly on the remote server as a working copy, it was not enough to keep me.

Some other features which are incomparable to other IDE's (Integrated Development Environment) at the moment, are integrated support with GIT and the Remote Server connection handling. The GIT integration really provides you with the full control of your commits, repository and history. When working with a pre-production and production environment, it is almost necessary to upload your changes before commiting them. This is where pairing your workspace with a pre-production remote server comes in handy. There is only one gotcha I have run into while using this solution. Continue reading below for more information.

## The Error

The below error is experienced at times when configuring a Remote Server connection using the wizard.

    Connection Error - The folder you entered is not valid. Enter an existing folder and try again Operation failed. Security violation

## The Explanation

When entering in the directory, the the Remote Connection's initial directory must be owned by the user trying to connect. Also, as a result, you cannot skip a directory for the project. You must add it to a sub-directory.

    Enter an existing folder and try again Missing element for : ''

## The Resolution

Let's say that your project on the server exists here:

    /var/www/user/domain.com/path/to/project

Within the remote server connection settings you should use the following (assuming you have the correct permissions):

**Remote Connection initial directory:**

    /var/www/user/domain.com/

**Project Path:**

    /path/to/project

