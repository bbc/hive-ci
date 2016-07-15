---
layout: documentation
title: Overview
permalink: documentation/overview.html
---

# Overview

{: .lead}
Hive CI is a CI Platform for on-device testing. It encompasses a number of
applications, tools and libraries that we use at the BBC for testing our
applications and websites on devices.

## Setting up a Hive

There are three main components that you will need to deploy to set up a Hive:

*   ***Hive Scheduler***

    This is the hive web application for scheduling tests and viewing results

*   ***Hive Runner***

    This is the component that detects devices and runs the tests. You can install
runners on multiple machines and they will all run tests from the scheduler and
report results back in.

*   ***Hive Mind***

    This is the device database. Though not essential for the running of Hive
CI it enables better monitoring and control of devices.

The complete setup containing these three components looks like this:

![Hive CI overview](/hive-ci/images/full-hive-ci.png){: class="img-responsive" }

Both Hive Scheduler and Hive Mind are web services that provide a web interface
and an API for communicating with devices and hives. The Hive Runners, normally
referred to simply as 'Hives' are servers to which the devices are connected.
These may be located at multiple locations provided they are able to have a
network connection to Hive Scheduler and Hive Mind.

### Hive Scheduler

The scheduler is the main hive application. It contains the definitions of your
tests and allows you to schedule those tests to run on devices that are connected
to your runners.

It is also where the results of your test runs are reported back.

### Hive Runner

The Hive Runner, or Hive, is the component that executes the tests and submits
results. It is also responsible for monitoring and looking after devices.

The core of the Hive is the `hive-runner` gem and it is configured with one or
more runner plugins depending on the device types that are to be connected.
Currently there are plugins for:

* Android mobile devices, from the `hive-runner-android` gem
* iOS mobile devices, from the `hive-runner-ios` gem
* TV and set top boxes, from the `hive-runner-tv` gem
* Shell scripts, built into the core Hive Runner

![Hive Runner top level](/hive-ci/images/hive-runner.png){: class="img-responsive" }

Each plugin comprises a single controller that manages all devices of the
particular type and workers for each device that are created as the devices
are connected and terminated as they are disconnected.

![Hive Runner top level](/hive-ci/images/hive-runner-and-plugin-details.png){: class="img-responsive" }

When you install the runner, you will be guided to install additional gems to
support the devices you want to test.

### HiveMind

Hive Mind is a database application for monitoring devices and device state.

### Testmine

Testmine is an optional results engine for HiveCI. It is a seperate application
that stores detailed reports of your test runs, and lets you investigate trends
in test performance. We will be releasing this component soon.

