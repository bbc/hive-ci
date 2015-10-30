---
layout: documentation
title: Setting up an iOS Hive
permalink: documentation/ios-hive.html
---


# Setting up an iOS Hive

{: .lead}
Setting up an iOS hive can be more involved than setting up an Android Hive and
requires a developer certificate and provisioning profile before you can install
applications and test them on devices.

{: .pull-right}
![Android Hive](/hive-ci/images/ios-hive.png)


## Pre-requisites

[XCode](https://developer.apple.com/xcode/) and [libimobiledevice](http://www.libimobiledevice.org/) need to be installed as these are used for communicating with the attached devices.

A developer certificate and provisioning profile is required to install and run apps on the deviceThe fruity_builder gem is also required if you wish to build and install your XCode projects.

## Installing the iOS runner

If you're using the Hive Setup script, you can easily add the iOS runner by choosing to 'Add Module' and entering 'ios' and then 'BBC' for the account name.

If you've already setup a Hive and want to add the iOS runner afterwards, you can do so by editing the Gemfile within your runner folder and adding in:
`gem 'hive-runner-ios'`

# iOS Config

  controller:
    ios:
      provisioning_profile: ab12c3d4-567e-8901-f2ab-34567890c1dg
      development_team: ABCD1EF2345
      signing_identity: iPhone Developer
      name_stub: IOS_WORKER
      port_range_size: 10

