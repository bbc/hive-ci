---
layout: documentation
title: Advanced runner setup
permalink: documentation/installing-runners.html
---

# Advanced runner setup

{: .lead}
Hive Runners are daemons that perform the execution of your tests. They are also
resonsible for monitoring devices and keeping them ready for execution. This
page guides you through general insallation instructions.

## Requirements

We recommend running your hive with ruby 2.0 and above, but it will also work
with ruby 1.9.3.

Once you've got ruby installed, you will need to install bundler:

    gem install bundler

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

## Configuration

The `hive_setup` script creates a configuration file `config/settings.yml`
containing the `daemon_name`, the process name of the running hive daemon,
and the following sections:

* Controllers
* Ports
* Logging
* Timings
* Network
* Diagnostics

### Controllers

Contained within the controllers section are subsections for each of device
module. All controller types include the following options:

{: .table .table-condensed .table-striped}
| Option          | Description                                |
|-----------------|--------------------------------------------|
| port_range_size | Number of ports to allocate to each worker |
| name_stub       | Stub for process name of each worker       |

Additionally, the shell controller takes the following options

{: .table .table-condensed .table-striped}
| Option  | Description                                             |
|---------|---------------------------------------------------------|
| workers | Number of shell workers                                 |
| queues  | List of queues to which each shell worker is subscribed |

### Ports

The range of ports available for workers.

### Logging

{: .table .table-condensed .table-striped}
| Option        | Description                                  |
|---------------|----------------------------------------------|
| directory     | Location of log files                        |
| pids          | Location of PID files                        |
| main_filename | Name of the main hived log file              |
| main_level    | Logging level for the main hived log file    |
| worker_level  | Logging level for each worker log file       |
| home          | Location of test workspaces                  |
| homes_to_keep | Number of completed test workspaces too keep |

### Timings

{: .table .table-condensed .table-striped}
| Option                   | Description                                                           |
|--------------------------|-----------------------------------------------------------------------|
| worker_loop_interval     | Time for each worker to wait between polls to the scheduler           |
| controller_loop_interval | Time for the Hive daemon to wait between polls to the device database |

### Network

{: .table .table-condensed .table-striped}
| Option    | Description                                    |
|-----------|------------------------------------------------|
| scheduler | URL of the Hive Scheduler                      |
| devicedb  | URL of the Device Database                     |
| cert      | Location of the SSL certificate file           |
| cafile    | Location of the SSL certificate authority file |
    
### Diagnostics

This section is empty by default but may include details of plugins for each
device type.

## Running a shell hive

