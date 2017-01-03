---
layout: documentation
title: TV Runner
permalink: documentation/tv_runner.html
menu_level: 2
---

# TV Runner

{: .lead}

The TV Runner provides capabilities for executing tests on Smart, or Connected,
TVs.

For Smart TVs, the Hive does not normally have direct connection with the
device. As the applications for TVs are Javascript web applications this
problem is overcome by loading Javascript libraries along with the application
to communicate with the Hive and with Hive Mind, and between tests a "holding
application" that communicates with Hive Mind is loaded.

The communication between the device and Hive Mind or the Hive is done by
making a regular request (a poll) where the server is able to respond with
instructions for the device.

When Hive Mind is polled it may respond with a
url for the device to load, either for the application to be tested or the
holding application at the end of a test.

During a test the Hive runs a Talkshow server for the device to poll and the
responses are the test commands.

An overview of the communication flow between the device, Hive Mind, the Hive
and Hive Scheduler is given below.

![TV Runner flow diagram](/hive-ci/images/tv-runner-flow.png)

