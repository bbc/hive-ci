---
layout: documentation
title: Getting Started
permalink: documentation/getting-started.html
---

# Getting Started

{: .lead}
Hive CI is a CI Platform for on-device testing. It encompasses a number of
applications, tools and libraries that we use at the BBC for testing our
applications and websites on devices.

## Setting up a Hive

There are two main components that you will need to deploy to set up a Hive:

*   ***Hive Scheduler***

    This is the hive web application for scheduling tests and viewing results

*   ***Hive Runner***

    This is the component that detects devices and runs the tests. You can install
runners on multiple machines and they will all run tests from the scheduler and
report results back in.

A typical setup might look like this:
   
                      ________________
                     |                |
                     | Hive Scheduler |
                     |________________|
                    /                  \
                   /                    \
           ______________               ________________
          |              |             |                |
          |  iOS Runner  |             | Android Runner |
          |______________|             |________________|

We host our scheduler in the cloud, and have multiple ubuntu boxes with android
devices connected by usb hubs, and multiple Mac Minis running osx with attached
iOS devices.

### Scheduler

The scheduler is the main hive application. It contains the definitions of your
tests and allows you to scheduler those tests to run on devices that are connected
to your runners.

It is also where the results of your test runs are reported back.

### Runner

The Runner is the component that executes the tests and submits results. It is
also responsible for monitoring and looking after devices.

The core of the runner is the hive-runner gem. This provides a simple shell
runner for running tests that don't require devices.

When you install the runner, you will be guided to install additional gems to
support the devices you want to test.

### HiveMind

Hive Mind is a database application for monitoring devices and device state. This
project isn't yet ready for open source release, but you can still set up your
hive without it.

### Testmine

Testmine is an optional results engine for HiveCI. It is a seperate application
that stores detailed reports of your test runs, and lets you investigate trends
in test performance. We will be releasing this component soon.

