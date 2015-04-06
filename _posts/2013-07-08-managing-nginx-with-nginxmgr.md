---
layout: post
permalink: /managing-nginx-with-nginxmgr
title: "Managing Nginx With nginxmgr"
category: "How To"
tags: http https nginx ruby-2 web-server
---
### Making Nginx Easier To Manage

When working with nginx, to help reduce the time required to manually create sites, enable, disable and delete them, we created a utility called, [nginxmgr](https://github.com/mikemackintosh/nginxmgr). This project is hosted on GitHub and is licensed under GNU-GPL v2. This project was written in Ruby thanks to it's quick and agile language syntax and performance footprint. Although we are mainly a PHP shop here, we are extremely open-minded and understand that ever language has their pro's and con's, and we felt Ruby was a great choice for this project.

Right now, a simple template is included in the package which will create a basic site for you. You can change the template as needed, or wait for the next version which will allow for multiple included templates for things like SSL, Ruby vs PHP FastCGI's, and more.

#### Installation:

Before you install `nginxmgr` make sure you have the basic requirements. This includes an installed version of Ruby `>= 1.9.1`, a gem named `micro-optparse` and `git`. Once you confirmed these, you can continue on.

This installation example will use `/usr/local` as the basedir (You can change it to whatever you would prefer):

    cd /usr/local
    sudo git clone https://github.com/mikemackintosh/nginxmgr
    cd nginxmgr
    sudo chmod +x nginxmgr
    sudo ln -s /usr/local/nginxmgr/nginxmgr /usr/bin

**Congratulations!** Installation has been completed!

### Usage:

Here is a copy of the usage info:

    # nginxmgr -h
    nginxmgr: by Zyp.io
        -i, --ip * set virtual host ip address
        -c, --create [] create a site supplying the server name string
        -e, --enable enable a site supplying the server name string
        -d, --disable disable a site supplying the server name string
        -D, --delete delete a site supplying the server name string
        -h, --help Show this message
        -v, --version Print version

### To Create a Site

To use the utility, you would run the following command:

    splug@dev.ve.zyp.io:~$ sudo nginxmgr -c "www.example.com,example.com,*.example.com"
    Creating site: www.example.com
    [Completed] Site has been created
    Enabling site: www.example.com
    Testing nginx configuration: nginx.
    Restarting nginx: nginx.
    [Completed] This site has been enabled and nginx reloaded
    Thank you and have a nice day!

This would create a directory in `/var/www/www.example.com` (your site name is the first fqdn that you pass) and skeleton directories of `web` and `logs` within in. A template for nginx will also be placed within `/etc/nginx/sites-available` for your domain.

**_Note:_** By default, sites that are newely created are automatically

By default the listening IP address and port in `*:80`. You can change this by passing option `-i ip.ad.dr.ess`.

### To Enable a Site

This utility will create a symbolic link from the `sites-available` dir to `sites-enabled`, run `configtest` on nginx, and restart nginx:

    splug@dev.ve.zyp.io:~$ sudo nginxmgr -e www.example.com
    Enabling site: www.example.com
    Testing nginx configuration: nginx.
    Restarting nginx: nginx.
    [Completed] This site has been enabled and nginx reloaded
    Thank you and have a nice day!

### To Disable a Site

This utility will delete the symbolic link from the `sites-enabled` dir, run `configtest` on nginx, and restart it:

    splug@dev.ve.zyp.io:~$ sudo nginxmgr -d www.example.com
    Restarting nginx: nginx.
    [Done!]
    Thank you and have a nice day!

**_Note:_** This does not delete any userdata, only the link from `sites-available` to `sites-enabled`.

### To Delete a Site

The `-D`, `--delete` switch will remove your domains configuration from nginx, restart if it is currently active, and move the directory `/var/www/domain` to `/var/www/domain.old`

    splug@dev.ve.zyp.io:~$ sudo nginxmgr -D www.testing2.com
    The www directory for this site has been moved to: '/var/www//www.testing2.com.old'
    [Done!]
    Thank you and have a nice day!

### Errors

We have been running this utility for a long time and have yet to experience a bug or an issue. If you happen to run into one, let us know and we will get you straightened out. We have done some basic usage testing on platforms that don't meet the required dependencies, and have come up with the following gotchas.

If you receive the following error:

    /usr/lib/ruby/1.9.1/rubygems/custom_require.rb:36:in `require': cannot load such file -- micro-optparse (LoadError)
        from /usr/lib/ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
        from /usr/bin/nginxmgr:5:in `<main>'

Make sure you execute `sudo gem install micro-optparse`. This will install the missing dependency.

### Summary

We have been using this utility in production for a few months, and it really makes life easier. Everyone is guilty that when out of desperation of a deadline, you fat finger a setting or miss a `;`. `nginxmgr` helps prevent issues like this in the future.

Like we stated earlier, the next version is slated to receive config templates. We will add some options to enable `ssl` and `webdav` by default, and even a way to implement WWWBasicAuthentication.

Stay tuned for updates.

