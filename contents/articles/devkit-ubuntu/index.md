---
title: GameClosure's DevKit and Ubuntu 12.04
author: Stewart Platt
date: 2013-03-23
template: article.jade
---

Game Closure have recently released an [Android and iOS development kit](https://github.com/gameclosure/devkit) for producing games using HTML 5 and Javascript.

Presently, they only support Mac OSX, but I played around and got it reasonably happy running on an Ubuntu 12.04 VM I had laying around.

## Pre-requisites

    $ # You will need git if you don't already have it
    $ sudo apt-get install git

    $ # Install java (Not tested yet)
    $ sudo apt-get openjdk-7-jre

    $ # This repository holds a very recent node.js package.
    $ sudo add-apt-repository ppa:chris-lea/node.js
    $ sudo apt-get update
    $ sudo apt-get install nodejs

### Sound + Chromium Browser

Install Chromium Browser, the preferred browser for usiing DevKit.

    $ # Install the Chromium browser (Note, this behaves odd for me in Unity)
    $ sudo apt-get install chromium-browser

By default, you won't be able to play mp3 sounds (like those shipped with the example whack-a-mole game) through Chromium, this installs ffmpeg support.

    $ sudo apt-get install chromium-codecs-ffmpeg-extra

## Get the latest DevKit

Following the [official DevKit tutorial](http://docs.gameclosure.com/guide/install.html), we'll clone the latest DevKit.

    $ git clone https://github.com/gameclosure/devkit 

----

**UPDATE: The following is no longer necessary! Please see [this post](/post/view/14).**

**Now comes the important stuff.**
Before running `./install.sh`, open it up  in your favourite editor and remove the initial lines which check the UID and exit if you are root.

e.g., just comment the entire conditional block out:

    # Make sure we are not running as root
    #if [[ $EUID -eq 0 ]]; then
    #   echo "This script should not be run as root" 1>&2
    #   exit 1
    #fi

----

Now execute!

    $ sudo ./install.sh

After this is complete, you will be left with a few files owned by root in your home directory. In particular, ~/.tmp and ~/.npm  

    $ sudo chown -R $USER:$USER ~/.tmp ~/.nmp

Unfortunately, one of the first things you'll want to do is run `basil update` but this actually checks out the latest version of the code and runs `./install.sh` again. You will need to repeat the above steps to get it to work again (removing the exit on UID == 0, running install.sh as root, clearing up afterwards).

## Success!

![Whack-A-Mole!](http://i.imgur.com/V0GN3Fl.png)

## Where now?

The next challenge is to get the example projects deploying to my, now ageing, HTC Desire HD. Check back soon for an update.
