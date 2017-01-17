---
layout: documentation
title: Glossary
permalink: documentation/glossary.html
---

{: .dl-horizontal .dl-spaced}
Batch
: A *batch* is an instance of a project for a specific version or build of the
  product. A batch comprises one or more jobs, which execute the same project
  with the same configuration and product build for different devices.

Controller
: The *controller* is part of the Hive Runner and its purpose is to manage all
  devices of a particular type so, for example, a Hive for Android devices will
  have a single Android controller. The controller for a device type is
  responsible for creating and managing workers of that type. The
  Hive Runner [gem](https://en.wikipedia.org/wiki/RubyGems) includes the
  controller for shell runners and each plugin gem includes its own controller.

Device API
: *Device API* is a Ruby library used by the runner to abstract device
  management functions.

Execution Script
: An *execution script* or *script* is a set of commands used as the foundation
  of a test in the hive scheduler. An execution script may be used by one or
  more projects.

Hive
: A *Hive* is a server running the Hive Runner software.

Hive CI
: *Hive CI* is a collection of one or more hives together with a
  Hive Scheduler server and optionally a
  Hive Mind server.

Hive Messages
: *Hive Messages* is a Ruby library providing the communication mechanism
  between the runner and the scheduler.

Hive Mind
: *Hive Mind* is the device management web application for Hive CI

Hive Runner
: The *Hive Runner* or *Runner* is the software for managing devices and
  executing tests. The runner includes one or more controllers as appropriate
  for the devices. The core Hive Runner, including the shell controller, is
  contained in the `hive-runner` [gem](https://en.wikipedia.org/wiki/RubyGems)
  and plugins for device types follow the naming convention
  `hive-runner-device_type`. For example, `hive-runner-android` and
  `hive-runner-ios`.

Hive Scheduler
: The *Hive Scheduler* or *Scheduler* is a web application for creating and
  managing test jobs, and assigning them to devices running on hives.
  The scheduler normally runs on a server separate from the
  hive runner and can provide tests for devices on multiple
  hives and of various types.

Mind Meld
: *Mind Meld* is a Ruby library providing an API for Hive Mind.

Job
: A *job* is an execution of a batch for a single device chosen according to
  the queue attached to the job. A batch is made up of multiple jobs configured
  for different queues.

Project
: A *project* in the Hive Scheduler denotes a test set of a particular product.
  A project comprises an execution script together with configuration details.

Queue
: A *queue* is a label attached to one or more devices to indicate them as
  candidates for executing a job. For example, a queue may contain all devices
  of a given operating system version while another queue may contain only
  devices of a very specific make, model and operating system.

Res
: *Res* is a results format used for reporting test results from the
  runner back to the scheduler.

Runner
: See Hive Runner

Scheduler
: See Hive Scheduler

Script
: See Execution Script

Worker
: The *worker* is part of the Hive Runner and its purpose is to
  control a single device of a particular type. A worker for a device is
  created by the controller for the device's type and it is
  responsible for requesting and executing tests from hive scheduler, reporting
  results back to the scheduler, reporting device information to Hive Mind and
  keeping the device in a healthy state. The core Hive Runner includes a *shell
  worker* that allows tests to be executed on the Hive itself. The shell worker
  can be used as an example for the creation of plugins for other device types.
