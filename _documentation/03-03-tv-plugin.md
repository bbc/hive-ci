---
layout: documentation
title: TV Plugin
permalink: documentation/tv_plugin.html
menu_level: 2
---

{: .lead}
The TV Hive Runner plugin is used for managing HTML application tests on TVs and set top boxes.

## Contents
* [Prerequisites](#prerequisites)
* [Installation](#installation)
* [TV test lifecycle](#tv-test-lifecycle)

## Prerequisites

Communication between TVs and the Hive Runner depends on [Hive Mind](https://github.com/bbc/hive_mind) and so this needs to be set up with the `hive_mind_tv` engine.

Between tests the devices need to be running a holding application such as [Hive Capture](https://github.com/bbc/hive-capture). This holding application should be installed on a web server and will allow the TV to poll Hive Mind. The holding application url and the application name that it reports to Hive Mind will be used in the configuration of the Hive, below.

For more details about the communication between the Hive and TVs see the [TV test lifecycle](#tv-test-lifecycle) section below.


## Installation

The Ruby gem for the TV Hive plugin is `hive-runner-tv` and it can be added to an existing hive by adding

```ruby
gem 'hive-runner-tv'
```

to `Gemfile` and executing `bundle install` while the Hive is stopped. Before restarting the Hive the following lines should be added to the `config/settings.yml` file inside the `controller` section:

```yaml
    tv:
      name_stub: TV_WORKER
      port_range_size: 10
```

and in the `network` section add:

```yaml
    hive_mind: http://<hive mind url>
    tv:
      titantv_url: http://<holding application url>
      titantv_name: <holding application name>
```

Alternatively, the TV plugin can be included when setting up a new Hive. Select the option to 'Add module' and enter `tv` as the module name. The `network` section above will need to be configured after installation as above.

## TV test lifecycle

The TV Runner provides capabilities for executing tests on Smart, or Connected, TVs.

For Smart TVs, the Hive does not normally have direct connection with the device. As the applications for TVs are Javascript web applications this problem is overcome by loading Javascript libraries along with the application to communicate with the Hive and with Hive Mind, and between tests a "holding application" that communicates with Hive Mind is loaded.

The communication between the device and Hive Mind or the Hive is done by making a regular request (a poll) where the server is able to respond with instructions for the device.

When Hive Mind is polled it may respond with a url for the device to load, either for the application to be tested or the holding application at the end of a test.

During a test the Hive runs a Talkshow server for the device to poll and the responses are the test commands.

An overview of the communication flow between the device, Hive Mind, the Hive and Hive Scheduler is given below.

![TV Runner flow diagram](/hive-ci/images/tv-runner-flow.png)
