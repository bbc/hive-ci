---
layout: documentation
title: Getting Started
permalink: documentation/getting-started.html
---

# Getting Started

{: .lead}
Hive CI -- Key concepts

<br />

## Introducing Hive CI

## Hive components

### Scheduler

### Runner

The Runner is the component that executes the tests and submits results. It is
also responsible for monitoring and looking after devices. The core of the
runner is the hive-runner gem and this needs to be supplemented by another gem,
such as hive-runner-android, to provide device specific functionality. The base
hive-runner gem provides a shell runner that enables scripts to be run on the
server.

#### Quick start

Install the hive-runner and set up your hive:

    gem install hive-runner
    hive_setup my_hive

Follow the instructions and, after configuring as appropriate for your hive
type, start the Hive with:

    cd my_hive
    hived start

See the status of the Hive with:

    hived status

Stop the Hive with:

    hived stop

### DeviceDB

### Testmine



