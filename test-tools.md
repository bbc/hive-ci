---
layout: default
title: Test Tools
---

# Test Tools

{: .lead}
Tools and libraries released under the BBC Hive open source project


<br />

## BBC Hive Open Source 

*BBC Hive* is a *BBC Digital* initiative to contribute open source tools and
libraries to the open source community. This page provides an overview of
the projects that fall under this open source banner. They're all hosted on
github and [MIT licensed](/hive-ci/license.html).

* * * 

### DeviceAPI

*DeviceAPI* is a collection of gems that make working with physical devices
easy and consistent. *DeviceAPI* provides common utilities such as device
detection and identification, and useful helpers for installing applications
and identifying problems with devices.

We use *DeviceAPI* heavily to leverage devices in *Hive CI*. It's also used by
our test developers in their test code. It's our interface to the devices we
test against and is where we put all common code for interacting with our
physical devices. It also provides an abstraction so we can deal with devices
interchangeably.

* * * 

#### DeviceAPI-Android

*DeviceAPI-Android* is the *DeviceAPI* implementation for Android devices. It
uses the [Android SDK](https://developer.android.com/sdk) command line tools
for interacting with connected devices:

    device = DeviceAPI::Android.devices.first
    
    device.model                        #=> 'samsung gt-i9100'
    device.orientation                  #=> :portrait
    device.install('path/to/build.apk') #=> true
    

In addition to detecting and identifying physical devices, *DeviceAPI-Android*
handles functions like:

* managing keystores and signing apps
* installing and deleting applications
* managing orientation and settings
* capturing screenshots
* performing cleanup operations
* executing monkey testing

<br />
<a class="btn btn-default" href="https://github.com/bbc/device_api-android">Code</a>
<a class="btn btn-default" href="https://rubygems.org/gems/device_api-android">Gems</a>
<a class="btn btn-default" href="http://www.rubydoc.info/github/bbc/device_api-android/master">Docs</a>

* * * 

#### DeviceAPI-IOS

*DeviceAPI-IOS* is the iOS implementation of *DeviceAPI*. It uses
*[libimobiledevice](http://www.libimobiledevice.org/)* for interacting with
connected devices.

This gem offers most of the same features as the Android version. The API 
is identical where possible:

    device = DeviceAPI::IOS.devices.first
    
    device.model      #=> 'iPad mini 3'

<br />
<button class="btn btn-default">[Code](https://github.com/bbc/device_api-ios)</button>
<button class="btn btn-default">[Gem](https://rubygems.org/gems/device_api-ios)</button>
<button class="btn btn-default">[Docs](http://www.rubydoc.info/github/bbc/device_api-ios/master)</button>

* * * 

#### DeviceAPI Base gem 

Shared code for the *DeviceAPI* gems lives in this base gem. If you want to
write a *DeviceAPI* gem for a new type of device (say a smart fridge or mass
spectrometer) you should take a look at the base gem documentation.

<br />
<button class="btn btn-default">[Code](https://github.com/bbc/device_api)</button>
<button class="btn btn-default">[Gem](https://rubygems.org/gems/device_api)</button>
<button class="btn btn-default">[Docs](http://www.rubydoc.info/github/bbc/device_api/master)</button>


* * * 

### ISA -- Image Session Analyzer

ISA is a ruby gem for comparing screenshots over a testing session. We use it in
combination with the [device_api](#device_api) gem to capture screenshots during
video playback tests and confirm that video is actually being played.

ISA uses ImageMagick to do the actual image analysis, but presents a simple
interface lets you concentrate on writing your tests.

It also generates animated gifs of the testing session, so we can visibly check
any problems the tests find.


<br />
<button class="btn btn-default">[Github](https://github.com/bbc/isa)</button>
<button class="btn btn-default">[Rubygems](https://rubygems.org/gems/isa)</button>
<button class="btn btn-default">[Documentation](http://www.rubydoc.info/github/bbc/isa/master)</button>



* * *

### Simple stats store

Simple Stats Store is a ruby gem that provides a simple and lightweight method
of sharing an SQLite database without contention.

It is appropriate to be used when:

* there are multiple threads or processes submitting data to a single SQLite
database
* data concurrency is less important than avoiding waiting on database locks

Rather than hold a database lock, any processes wanting to write to the
database drop a uniquely named file to a directory containing the data. These
files are picked up by a single thread that is the sole accessor of the
database.


<br />
<button class="btn btn-default">[Github](https://github.com/bbc/simple_stats_store)</button>
<button class="btn btn-default">[Rubygems](https://rubygems.org/gems/simple_stats_store)</button>
<button class="btn btn-default">[Documentation](http://www.rubydoc.info/gems/simple_stats_store)</button>

* * *

