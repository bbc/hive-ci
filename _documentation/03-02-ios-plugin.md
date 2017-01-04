---
layout: documentation
title: iOS Plugin
permalink: documentation/ios_plugin.html
menu_level: 2
---

{: .lead}
The iOS Hive Runner plugin is used for managing tests on iPhones and iPads. It can be more complicate to set up than the Android Runner as developer certificates and provisioning profiles are required.

# Contents
* [Prerequisites](#prerequisites)
* [Installation](#installation)

# Prerequisites

[XCode](https://developer.apple.com/xcode/) and [libimobiledevice](http://www.libimobiledevice.org/) need to be installed as these are used for communicating with the attached devices.

A developer certificate and provisioning profile are required to install and run apps on the device.

# Installation

The Ruby gem for the iOS Hive plugin is `hive-runner-ios` and it can be added to an existing hive by adding

```ruby
gem 'hive-runner-ios'
```

to `Gemfile` and executing `bundle install` while the Hive is stopped. Before restarting the Hive the following lines should be added to the `config/settings.yml` file inside the `controller` section:

```yaml
    ios:
      name_stub: IOS_WORKER
      port_range_size: 10
```

Alternatively, the iOS plugin can be included when setting up a new Hive. Select the option to 'Add module' and enter `ios` as the module name.
