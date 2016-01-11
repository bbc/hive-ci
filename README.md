# Hive-CI

Hive CI is a CI Platform for on-device testing. It encompasses a number of
applications, tools and libraries that we use at the BBC for running tests on
physical devices on our in-house device cloud.

The Hive codebase is split across a lot of repositories on github. This repo
serves as a central landing-page for our github repositories.

If you want to set up a hive, you're best looking at [the documentation
site](http://bbc.github.io/hive-ci/documentation/hive-ci.html).

If you're interested in the contributing, the following repositories contain
the core code and details of how to start contributing.

## [hive-scheduler](https://github.com/bbc/hive-scheduler)

This is the main web application that drives the Hive. It's here that you
set up your tests and schedule them to run on devices.

## [hive-runner](https://github.com/bbc/hive-runner)

The runner is the execution component of the hive. The runner detects and
manages connected devices and takes jobs from the scheduler for execution.

The core runner includes a shell execution component, so you can run basic
tests without devices. You will have to install a runner plugin to get your
tests running on devices:

* [hive-runner-android](https://github.com/bbc/hive-runner-android)
* [hive-runner-ios](https://github.com/bbc/hive-runner-ios)

