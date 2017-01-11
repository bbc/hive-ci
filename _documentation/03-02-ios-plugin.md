---
layout: documentation
title: iOS Plugin
permalink: documentation/ios_plugin.html
menu_level: 2
---

{: .lead}
The iOS Hive Runner plugin is used for managing tests on iPhones and iPads. It can be more complicate to set up than the Android Runner as developer certificates and provisioning profiles are required.

## Contents
* [Prerequisites](#prerequisites)
* [Installation](#installation)

## Prerequisites

[XCode](https://developer.apple.com/xcode/) and [libimobiledevice](http://www.libimobiledevice.org/) need to be installed as these are used for communicating with the attached devices.

A developer certificate and provisioning profile are required to install and run apps on the device.

To allow devices to be used in the Hive:

* The device must have 'Enable UI Automation', under Settings -> Developer, is enabled.
* The device must be trusted.

## Installation

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

## Troubleshooting

### Devices are not detected

For an iOS device to be detected by the Hive it needs to be listed by
`idevice_id` and `ideviceinfo` must be able to list its information:

```bash
$ idevice_id -l
$ ideviceinfo -u <udid of your device>
```

#### Problem: `idevice_id` command not found

Install `libidevicemobile`

```bash
$ brew install libidevicemobild`
```

#### Problem: `ideviceinfo` does not return device information or returns "Could not connect to lockdown"

* The device must be trusted when the USB cable is attached
* 'Enable UI Automation' must be set. On the device open 'Settings' and open 'Developer'.
