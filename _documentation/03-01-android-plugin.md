---
layout: documentation
title: Android Plugin
permalink: documentation/android_plugin.html
menu_level: 2
---

{: .lead}
The Android Hive Runner plugin is used for managing tests on Android mobile and tablet devices.

# Contents
* [Prerequisites](#prerequisites)
* [Installation](#installation)
* [Keeping devices healthy](#keeping-devices-healthy)

# Prerequisites

Although it is theoretically possible to run an Android Hive on any operating system that supports Android Studio, for best results it is recommended to use Linux. Specifically, Ubuntu is known to work well.

The Android Hive communicates with devices using `adb` and so [Android SDK](https://developer.android.com/studio/index.html#downloads) need to be installed. Specifically, it must be possible to execute the following commands; `adb` and `aapt`. A recent version of Android SDK should be installed in preference to versions supplied with the Linux distribution as these can have problems working with multiple devices concurrently.

# Installation

The Ruby gem for the Android Hive plugin is `hive-runner-android` and it can be added to an existing hive by adding

```ruby
gem 'hive-runner-android'
```

to `Gemfile` and executing `bundle install` while the Hive is stopped. Before restarting the Hive the following lines should be added to the `config/settings.yml` file inside the `controller` section:

```yaml
    android:
      name_stub: ANDROID_WORKER
      port_range_size: 10
```

Alternatively, the Android plugin can be included when setting up a new Hive. Select the option to 'Add module' and enter `android` as the module name.

# Keeping devices healthy

The Android plugin includes some diagnostic checks for memory usage, battery temperature and uptime. In order to use these diagnostics you need to enable them in `config/settings.yml`. The following settings will reboot all devices every 2 hours (7200 seconds) and stop tests running if the battery temperature increases above 32C or the voltage increases above 4600mV:

```yaml
  diagnostics:
    android:
      uptime:
        reboot_timeout: 7200
      battery:
        temperature: 320,
        voltage: 4600
```
