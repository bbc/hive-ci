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

You should first create a script to base your project on. When you're ready to
create a project:

1.  Click on the **Projects** link on the top nav

2.  Click **New**

3.  Fill in a **Name** for your project

4.  Fill in a **Respository** for where your code presides -- currently we support
git and svn locations*. 

5.  Fill in the **Execution Directory** -- this is the directory relative to the root
of your checkout where your code presides. You should set this to `.` if your
tests execute in the root of your checkout.

6.  Select the exectution **Script** from the dropdown.

7.  Select a **Population Mechanism**. This should typically be **Manual**
unless you're using the optional **TestRail** integration.

    The **Population Mechanism** defines what test you run, and what devices
    they run on.

8.  Add in the queues you want the tests to run against.


**Note** that queues are currently only discoverable from the hive runner
command line. Run the following on your hive to discover the queues available to
you:

    hived status
    
    start_hive: running [pid 14952]
    
    Controllers in use:
      * shell
    Total number of devices: 2
    
    +---------+--------+-----+-----------+--------+
    | Device  | Worker | Job | Status    | Queues |
    +---------+--------+-----+-----------+--------+
    +---------+--------+-----+-----------+--------+
    | Shell-1 | 14956  | --- | ---       | osx    |
    |         |        |     |           | shell  |
    +---------+--------+-----+-----------+--------+
    | Shell-2 | 14957  | --- | ---       | osx    |
    |         |        |     |           | shell  |
    +---------+--------+-----+-----------+--------+


Note there are two shell workers, each available on the osx and shell queues.
    
Finally click **Create Project** to run your tests.
    
* Note that we only support the git default branch although we're working on
supporting multiple branches and tags.

## Triggering a batch

Once your tests are set up you can trigger a batch in two ways:

* Using the UI

* From an external system using the api

### Triggering with the UI

1.  Click on the 'Batches' tab

2.  Fill in a **Name** for the batch

3.  Select the **Project** you previously set up

4.  Fill in a **Version**
    This is a string that represents the version of the thing being tested. This
    is useful for comparing results across different builds.
    
5. Click **Next** to click through the various optional sections of the form.

    * Fill in any execution variables you need  
    * Upload the build or builds if your tests need them
    * Add or remove any queues you want to run the tests on
    * Optionally choose to split the tests
      
      **Note**: This is only available if you have defined the tests you were 

6. Click **Finish** to trigger your tests

### Simple 'Hello world' example

1. Go into the **Scripts** tab

2. Create a new script

3. Select **Shell Script** as the target platform

4. Set the name to <code>Hello \<name\></code> and in the template box enter:
   <code>echo Hello $HIVE_WORD</code>

5. Add a new execution variable called `word` and set the field type to **String**

6. Save the script

7. Go into the **Projects** section and create a new project.

8. Set the name to **Hello world** and select your **Hello \<name\>** script

9. Select the **Manual** population mechanism and enter <code>world</code> in
   the Word field and <code>bash</code> in the Queues field. Leave all other
   fields as the defaults.
   
10. Save the project.

11. Go into the **Batches** section and create a new batch. Set the name to
<code>First test batch</code>.

12. Select <code>Hello world</code> as the project and set the version to
<code>1.0</code>.

13. Leave all other fields as the defaults.

14. Save the batch.

In the **Batches** section you will now see the batch you have just created
and by clicking on the name you can see a single job belonging to this batch for
the queue 'bash'. If you have a hive set up to run shell tests on a queue
named 'bash' then this test will be executed and by clicking on the job number
you can view the output.


