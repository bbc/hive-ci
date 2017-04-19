---
layout: documentation
title: Best Practices
permalink: documentation/best-practices.html
menu_level: 2
---

{: .lead}
This page contains some notes of experience we have found in setting up and using the Hive. Your experiences may vary and we welcome feedback to improve this advice.

## Contents
* [iOS devices](#ios-devices)

## iOS devices

* Turn off notifications.
  * Notifications can cause UI tests to fail so as far as possible these notifications or the services that cause them should be disabled. From the settings menu go to 'Notifications' and then for each app turn off notifications.
* Turn on developer mode and UI automation.
* Provision the device.
* Turn off the screen lock on the device completely.
  * If the screen lock is set then the device will boot up locked. Therefore it is necessary to unset the screen lock.
