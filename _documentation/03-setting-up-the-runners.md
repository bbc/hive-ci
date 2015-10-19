---
layout: documentation
title: Installing the Runners
permalink: documentation/installing-runners.html
---

# Setting up the Runners

{: .lead}
Installing the ruby daemons for job execution

<br />

## Requirements

(ruby versions etc)

## Installing the runner

To install the hive-runner and set up your hive:

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

## Running a shell hive

