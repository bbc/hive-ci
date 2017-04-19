---
layout: documentation
title: Device API
permalink: documentation/device_api.html
menu_level: 2
---

{: .lead}
Device API is a collection of gems that make working with physical devices easy and consistent. It provides common utilities such as device detection and identification, and useful helpers for installing applications and identifying problems with devices. It is used by the Hive Runner but can be used in other projects.

## Contents
* [Basic usage](#basic-usage)
* [Connecting to devices](#connecting-to-devices)
* [Other device commands](#other-device-commands)

## Basic Usage

Currently Device API support Android and iOS mobile devices. To install, add

```ruby
gem 'device_api-android'

# or

gem 'device_api-ios'
```

to the `Gemfile` of your project. Do detect all connected devices use

```ruby
require 'device_api-android'
devices = DeviceAPI::Android.devices

# or

require 'device_api-ios'
devices = DeviceAPI::IOS.devices
```

To get a list of the model and OS version of all devices:

```ruby
devices.each do |d|
  puts "#{d.serial}:"
  puts "  Model #{d.model}"
  puts "  OS Version #{d.version}"
end
```

## Connecting to devices

A device object can be created from its **qualifier**:

```ruby
device = DeviceAPI::Android.device( qualifier )
device = DeviceAPI::IOS.device( qualifier )
```

For iOS the qualifier is the device UDID as found in iTunes or XCode.

For Android the qualifier is the serial number for devices connected over USB
or the the IP address together with port number (default 5555) for devices
connected over the network. This is shown by `adb devices`.

Alternatively, an array of all device objects can be created:

```ruby
devices = DeviceAPI::Android.devices
devices = DeviceAPI::IOS.devices
```

## Other Device Commands

```ruby
# Show serial number (Android) or UDID (iOS)
device.serial
device.serial_no        # Alternative for Android only

# Show status
device.status

# Show device qualifier
device.qualifier
# Android -> Serial number or IP address plus port 5555
# iOS     -> UDID

# Get device name
# TODO: Make consistent
device.device           # Android
device.name             # iOS

# Get device make and model
device.model
device.manufacturer     # Android only

# Get OS version
device.version

# Get IP address
device.ip_address       # Helper app needs to be installed for iOS

# Get MAC address
device.wifi_mac_address # Unavailable for Android 6.0 and later

# Get IMEI
device.imei

# Show installed packages
# TODO: Make the output consistent between Android and iOS
device.list_installed_packages

# Install/uninstall package
device.install( package_file )
device.uninstall( package_name )

# Take a screenshot
device.screenshot( filename: filename )

# Reboot
device.reboot
device.restart
```
