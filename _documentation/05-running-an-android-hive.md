---
layout: documentation
title: Running an Android Hive
permalink: documentation/android-hive.html
---

{: .pull-right}
![Android Hive](/hive-ci/images/android-hive.png)

# Running an android hive

{: .lead}
The massive fragmentation of the Android market makes it a primary target of
Hive CI. This page illustrates how to set up your android test environment and
install Hive Runner for Android.

## Pre-requisites

The Android SDK should be installed and the `adb` and `aapt` commands should be available to the command line.

## Installing the Android runner

If you're using the Hive Setup script, you can easily add the Android runner by choosing to 'Add Module' and entering 'android' and then 'BBC' for the account name.

If you've already setup a Hive and want to add the Android runner afterwards, you can do so by editing the Gemfile within your runner folder and adding in:
`gem 'hive-runner-android'`

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


