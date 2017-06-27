---
layout: documentation
title: Technical Information
permalink: documentation/technical_information.html
menu_level: 2
---

{: .lead}
This page contains some technical information about various components of Hive
CI. This will not normally be required information for day-to-day use but will
be of use for development. Note that this page is work in progress so if there
is information you require that is missing please ask in our
[Gitter channel.](https://gitter.im/bbc/hive-ci)

## Contents
* [Hive Scheduler](#hive-scheduler)
* [Hive Messages](#hive-message)

## Hive Scheduler

Hive Scheduler is a Ruby on Rails application using Rails version 4.1.

### Scripts

An execution script includes the following fields:

* `name`: The display name for the script.
* `template`: A template for the script to execute.
* `target_id`: Foreign key to the target type model.
* `install_build`: Flag to indicate that a build should be installed prior to the test.

### Targets

A target refers to a particular device type and configures certain parameters
that are included in test batches and jobs. By default there are five targets:

Android APK
: A build (APK file) is required and the test will run on the Android Hive
  Runner

iOS IPA
: A build (IPA bundle) is required and the test will run on the iOS Hive
  Runner

Mobile Browser
: A url is required and the test will run on either an Android or iOS Hive
  Runner

TAL TV App
: An application url and application url parameters string are required and
  the test will run on the TV Hive Runner

Shell
: No additional details are required and the test will run on the Shell Hive
  Runner

The required parameters are stored in the Field model and the targets ase are
defined in `db/seeds.rb`.

### Fields

Extra fields can be added to a target by adding a new element in the Field
model:

```ruby
Target.find(name: 'Android APK').fields << Field.create(
  name: 'new_field',
  field_type: :string
)
```

This new field will then appear in the batch creation field automatically
provided that there is a partial view in `app/views/fields` corresponding
to the field_type.

## Hive Messages

Hive Messages is a Ruby gem providing the communication mechanism between
the scheduler and the hives. The gem can be found at
[Rubygems](https://rubygems.org/gems/hive-messages) and the source at
[Github.](https://github.com/bbc/hive-messages)

Hive Messages uses [Roar](https://rubygems.org/gems/roar) for transferring
details of the job and the messages (`Hive::Message::Job` and
`Hive::Message::Artifact`) are wrappers for representers (see the Roar
documentation).

### Job Message

A job message is created using:

```ruby
Hive::Messages::Job.new( *job attributes* )
```

It contains the following attributes:

{: .table .table-striped}
| Attribute | Format | Source in Hive Scheduler |
|---|---|---|
| `command` | String | `Script.template` |
| `job_id` | Integer | `Job.id` |
| `repository` | String | `Project.repository` |
| `execution_directory` | String | `Project.repository` |
| `target` | Hash | `Job.target` |
| `execution_variables` | ExecutionVariables | Various |
| `reservation_details` | Hash | 
| `device_id` | Integer | `Job.device_id` |
| `running_count` | Integer | `Job.running_count` |
| `failed_count` | Integer | `Job.faild_count` |
| `errored_count` | Integer | `Job.errored_count` |
| `passed_count` | Integer | `Job.passed_count` |
| `state` | String | `Job.state` |
| `result` | String | `Job.test_results` |
| `exit_value` | Integer | `Job.exit_value` |
| `message` | String | `Job.message` |
| `result_details` | String | |
| `test_results` | Hash | `Job.test_results` |
| `log_files` | Hash | `Job.log_files` |

In Hive Scheduler the job message is created in the `Hive::Messages::Job`
class in `app/commands/job_commands/job_message_mapper.rb` and for attributes
that are not directly from the `Job` class are generated in this class.

The `execution_variables` are defined in the `Script` model and they can be
set either in the `Project` or the `Batch`. Additionally, `Batch.version`,
`Job.id` and `JobGroup.queue_name` are included.

For adding a new parameter for a job it can either be added to the
`execution_variables` or as a new field. In the latter case the following
changes are required:

* In Hive Scheduler, add the field to the model as required
  * If the field belongs to the `Job` model then no further change to Hive
    Scheduler is required
  * If the field does not belong to the `Job` model then the
    `job_message_attributes` method
    `app/commands/job_commands/job_message_mapper.jb` needs to be updated
* In Hive Messages
  * Create a new spec test by adding the new field as an attribute in
    `TestJob` in `spec/lib/hive/representers/job_representers_spec.rb` and
    then add an appropriate test in the format:

    ```ruby
    its(:new_field) { should eq job_attributes[:new_field] }
    ```
  * Add the new field as a new property in
    `lib/hive/representers/job_representer.rb`
  * Add the field as an attribute in `lib/hive/messages/job.rb`
