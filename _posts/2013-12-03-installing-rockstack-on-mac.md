---
layout: post
permalink: /installing-rockstack-mac
title: "Installing Rockstack On Mac"
category: "OS X"
tags: devenv mac perl php-2 rock rockstack ruby-2
---
### Introduction

[Rockstack](http://www.rockstack.org/) is a developer environment (DevEnv) tool that makes installing and managing code bases extremely easy. At my new position, we use the tool frequently to reduce the headache and time to maintain code. A rock project could be any type of code with the addition of a `.rock.yml` file which contains specifics for running your code. It works with PHP, Node, Perl, Ruby and Python.

Some of the benefits include the ability to manage your `composer` dependencies or launch a test server.

## Installing Rock

Before you continue, make sure you have [`homebrew`](http://brew.sh/) installed.

Download the rockstack brew taps and Formula's:

    bash -c "$(curl -fsSL https://raw.github.com/rockstack/utils/master/install)"

Then install a runtime (or multiple) of your choice:

    brew install rock-runtime-php55

## Setting Your Environment

To prevent conflicts with binary names, if you install PHP, you will not be able to execute it from your existing `$PATH`. You will need to run:

    eval $(rock --runtime=<runtime> env)

Example:

    eval $(rock --runtime=php55 env)

Now when you run `php -i` you will get your PHP info output.

## Example `.rock.yml`

This `.rock.yml` file will set the runtime to `php55`, use `composer` to install and update packages and listen on a local HTTP socket of your choice to test locally. The directory structure for this example is as follows:

    /project
        |--- .rock.yml
        |--- bootstrap.php
        |--- web/
              |--- index.php

You can stick something basic into `index.php`.

**_.rock.yml_**

    runtime: 
        php55
    
    
    build: 
        exec composer install -v
    
    
    update: 
        exec composer selfupdate && composer update -v
    
    
    run: 
        exec php -S "${HTTP_HOST-0.0.0.0}:${HTTP_PORT-80}" -t "web"
    
    
    run_daemon: 
        exec php -S "${HTTP_HOST-0.0.0.0}:${HTTP_PORT-8000}" -t "web" &
    
    
    kill_daemon: 
        kill -9 $(ps aux | grep "php -S" | grep -v "grep" | awk -F" " '{print $2}')

**_bootstrap.php_**

    <?php
    include_once 'vendor/autoload.php';
    
    
    echo "My First Rock App";

**_web/index.php_**

    <?php
    include_once '../bootstrap.php';

## Conclusion

These basic steps will get rock running in your Mac with a basic config.

#### Gotcha's With Rock perl516

I had an issue installing `rock-runtime-perl516`. I would get the following:

    % brew install rock-runtime-perl516
    ==> Downloading http://www.cpan.org/src/5.0/perl-5.16.3.tar.bz2
    Already downloaded: /Library/Caches/Homebrew/rock-runtime-perl516-5.16.3.tar.bz2
    ==> ./Configure -des -Dprefix=/usr/local/Cellar/rock-runtime-perl516/5.16.3 -Dvendorprefix=/usr/local/Cellar/rock-runtime-perl516/5.16.3 -Dsiteprefix=/usr/local/
    ==> make
    ==> make install
    ==> curl -LO http://search.cpan.org/CPAN/authors/id/A/AP/APEIRON/local-lib-1.008009.tar.gz
    ==> tar -xzf local-lib-1.008009.tar.gz
    ==> perl Makefile.PL
    ==> make install
    ==> curl -Lo /usr/local/Cellar/rock-runtime-perl516/5.16.3/bin/cpanm https://raw.github.com/miyagawa/cpanminus/1.6008/cpanm
    ==> chmod 755 /usr/local/Cellar/rock-runtime-perl516/5.16.3/bin/cpanm
    ==> cpanm -l local http://backpan.cpan.org/authors/id/T/TO/TOKUHIROM/Test-Requires-0.06.tar.gz http://backpan.cpan.org/authors/id/D/DA/DAGOLDEN/Capture-Tiny-0.21
    tar: Error exit delayed from previous errors.
    ! Failed to unpack App-cpanminus-1.7001.tar.gz: no directory
    ! Failed to fetch distribution App-cpanminus-1.7001
    ! Bailing out the installation for carton-v0.9.10. Retry with --prompt or --force.
    12 distributions installed
    
    
    READ THIS: https://github.com/mxcl/homebrew/wiki/troubleshooting
    If reporting this issue please do so at (not mxcl/homebrew):
      https://github.com/rockstack/homebrew-rock/issues

If you look at the formula for brew, it says CPANM version of 1.6008 will be used, but there's and issue with extracting version 1.7001.

To correct this, you can do:

    % cpan App::cpanminus
    % cpanm -l local http://backpan.cpan.org/authors/id/T/TO/TOKUHIROM/Test-Requires-0.06.tar.gz \ 
        http://backpan.cpan.org/authors/id/D/DA/DAGOLDEN/Capture-Tiny-0.21.tar.gz \
        http://backpan.cpan.org/authors/id/M/MI/MIYAGAWA/App-cpanminus-1.6008.tar.gz

And then when you re-run `brew install rock-runtime-perl516`, it will execute successfully.

