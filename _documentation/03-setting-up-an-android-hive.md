---
layout: documentation
title: Setting up an Android Hive
permalink: documentation/android-hive.html
---

{: .pull-right}
![Android Hive](/hive-ci/images/android-hive.png)

# Setting up and Android Hive

{: .lead}
The massive fragmentation of the Android market makes it a primary target of
Hive CI. This page illustrates how to set up your android test environment and
install Hive Runner for Android.

## Hardware

We run our android runners on the latest ubuntu and recommend you do the same.

Android runner will work on OSX, but we have found the stability of adb to be
unsuitable for our CI needs.

## Pre-requisites

Install the [Android SDK](#). You should ensure that the follwoing commands are
available on the command line:

* `adb`
* `aapt`

You will also need a recent version of ruby (>=2.0) although we are trying to
maintain compatibility back to 1.9.3 for teams stuck on older versions.

## Installing the runner

To install the hive-runner and set up your hive:

    gem install hive-runner
    hive_setup my_hive

The hive_setup script will guide you through the install. You will need to
choose 'Add Module' and entering 'android' and then 'BBC' for the account name.

If you've already setup a Hive and want to add the Android runner afterwards,
you can do so by editing the Gemfile within your runner folder and adding in:

    gem 'hive-runner-android'

## Android Config

    controller:
      android:
        name_stub: ANDROID_WORKER
        port_range_size: 10

## Keeping a healthy hive

Hive Runner Android includes some diagnostic checks for memory usage, battery temperature and uptime.

If you wish to use these diagnostics you need to enable them through the config, below is an example of how to enable the uptime diagnostic and reboot once a day:

    diagnostics:
      android: {}
      uptime: {
        reboot_timeout: 86400,
      }
