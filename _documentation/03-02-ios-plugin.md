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
* [Test environment](#test-environment)
* [Troubleshooting](#troubleshooting)
  * [Devices are not detected](#devices-are-not-detected)

## Prerequisites

[XCode](https://developer.apple.com/xcode/), [libimobiledevice](http://www.libimobiledevice.org/) and [ideviceinstaller](https://github.com/libimobiledevice/ideviceinstaller) need to be installed as these are used for communicating with the attached devices. Note that the currently released version of `libimobiledevice` (1.2.0) no longer works with recent versions of XCode and so it is necessary to installed from Github using (for example) `brew install --HEAD libimobiledevice`.

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
      signing_identity: 'Your iPhone developer signing identity'
```

Alternatively, the iOS plugin can be included when setting up a new Hive. Select the option to 'Add module' and enter `ios` as the module name. You will need to add the `signing_identity` line to `config/settings.yml`, shown above, before starting the Hive.

## Test Environment

The following environment variables for use by test scripts executed by the
iOS runner.

{: .table .table-striped}

| Variable | Description |
|---|---|
| `$BUNDLE_ID` | The bundle id of the application |
| `$APP_PATH` | The path to the application bundle file (IPA) |
| `$APP_BUNDLE_PATH` | Alias for `$APP_PATH` |
| `$DEVICE_TARGET` | Serial number of the device |
| `$DEVICE_NAME` | Name of the device |
| `$PLATFORM_NAME` | Set to `iOS` |
| `$PLATFORM_VERSION` | iOS version |
| `$DEVICE_ENDPOINT` | `http://<device ip>:37265` (Used by Calabash tests) |
| `$CHARLES_PROXY_PORT` | Port on the Hive available for use by the test |
| `$APPIUM_PORT` | Port on the Hive available for use by the test |
| `$BOOTSTRAP_PORT` | Port on the Hive available for use by the test |
| `$CHROMEDRIVER_PORT` | Port on the Hive available for use by the test |

The port variables are named for convenience of use with various testing
frameworks but may be used by the test when any local ports are required.

## Troubleshooting

### Devices are not detected

For an iOS device to be detected by the Hive it needs to be listed by
`idevice_id` and `ideviceinfo` must be able to list its information:

```bash
$ idevice_id -l
$ ideviceinfo -u <udid of your device>
```

#### Problem: `idevice_id` command not found

Install `libimobiledevice`. For example, use `brew` on MacOS.

```bash
$ brew install --HEAD libimobiledevice
```

Note, the `--HEAD` option is required to get the latest version as the current release, 1.2.0, no longer works. See https://github.com/libimobiledevice/libimobiledevice/issues/380 for more details.

#### Problem: `ideviceinfo` does not return device information or returns "Could not connect to lockdown"

* The device must be trusted when the USB cable is attached
* 'Enable UI Automation' must be set. On the device open 'Settings' and open 'Developer'.

#### Problem: Cannot find 'Developer' settings on device

For iOS 10 and later the 'Developer' settings need to be enabled with XCode. With XCode running attach the device and the option will appear. If it does not, open the 'Devices' dialog from the Windows menu and select the device. On older versions of XCode there will be an option to enable the developer mode.

This only needs to be done once for each device.
