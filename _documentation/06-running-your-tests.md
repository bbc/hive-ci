---
layout: documentation
title: Running Your Tests
permalink: documentation/running-your-tests.html
---

# Running your tests

{: .lead}
The Hive Scheduler is where you define your tests and the projects that you want
to run.

## Adding a script

Scripts are execution templates for running your tests. You can share scripts
between different projects, and can optionally paramterise your scripts with
execution variables.

To add a new script:

1.  Click on the **Scripts** link on the top nav

2.  Click **New**

3.  Click on a platform:

    ![Target Platforms](/hive-ci/images/script-types.jpg)

4.  Type in a name for your script

5.  Fill in the script template

If you want to add parametisation, you can do so by adding execution variables.
These are exposed in your scripts as environment variables.

## Creating projects



## Triggering a batch



### Hello world

Go into the 'Scripts' section and create a new script. Select
'Shell Script' as the target platform.
Set the name to 'Hello \<name\>' and in the template box enter:

```bash
# This will use the execution variable 'word'
echo Hello $HIVE_WORD
```

Add a new execution variable called `word` and set the field type to 'String'. Save the script.

Go into the 'Projects' section and create a new project. Set the name to
'Hello world' and select the 'Hello \<name\>' script. Select the
'Manual' population mechanism and enter 'world' in the Word field and 'bash'
in the Queues field. Leave all other fields as the defaults. Save the project.

Go into the 'Batches' section and create a new batch. Set the name to 'First
test batch'. Select 'Hello world' as the project and set the version to '1.0'.
Leave all other fields as the defaults. Save the batch.

In the 'Batches' section you will now see the batch you have just created and
by clicking on the name you can see a single job belonging to this batch for
the queue 'bash'. If you have a hive set up to run shell tests on a queue
named 'bash' then this test will be executed and by clicking on the job number
you can view the output.

### Android test

The Android Calabash script is set up by default. In the 'Projects'
section create a new project and select 'Android Calabash' as the execution
type. Set the name to 'X Platform Example' and enter the following in the
repository field:

```
git@github.com:calabash/x-platform-example.git
```

Select 'Manual' as the population mechanism and enter 'android-5.1.1' in the
queue field. Click on the 'Add queue' link and enter 'android-4.4.4' in the
new queue field.

**Note;** the hive runner is configured to run tests listed
in queues specified by name. The values entered in these fields should be
changed as appropriate for the configuration of your hives.

Save the project.

In the 'Batches' section create a new batch and select the project 'X Platform
Example'. Set the name to 'Testing X platform example' and set the version.
Upload the apk, which can be found at

* https://github.com/calabash/x-platform-example/tree/master/prebuilt

Leave all other fields unchanged (the list of queues may be amended as required)
and save the batch. As with the 'Hello world' example, the jobs will run if
hives are set up with devices for the given queues and output can be viewed.

