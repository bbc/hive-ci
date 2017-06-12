---
layout: documentation
title: Quick Start Guide
permalink: documentation/quick-start.html
menu_level: 2
---

{: .lead}
This quick start guide demonstrates how to get a single Hive Runner and Hive Scheduler up and running on a single server to execute a simple shell script test. This is followed by two further setup guides; setting up Hive Mind and a Hive Runner to run Android tests.

## Contents
* [Part 1: Hive Runner and Hive Scheduler]({{ post.url }}#part-1-hive-runner-and-hive-scheduler)
  * [Prerequisites](#prerequisites)
  * [Hive Scheduler](#hive-scheduler)
  * [Hive Runner](#hive-runner)
  * [Example test](#example-test)
* [Part 2: Android Hive Runner](#part-2-android-hive-runner)
  * [Modifying an existing Hive Runner](#modifying-an-existing-hive-runner)
  * [Setting up a new Hive Runner with the Android plugin](#setting-up-a-new-hive-runner-with-the-android-plugin)
  * [Example test](#example-test-1)
* [Part 3: iOS Hive Runner](#part-3-ios-hive-runner)
  * [Modifying an existing Hive Runner](#modifying-an-existing-hive-runner-1)
  * [Setting up a new Hive Runner with the iOS plugin](#setting-up-a-new-hive-runner-with-the-ios-plugin)
* [Part 4: Hive Mind](#part-4-hive-mind)
  * [Connect Hive Runner to Hive Mind](#connect-hive-runner-to-hive-mind)
  * [Connect Hive Scheduler to Hive Mind](#connect-hive-scheduler-to-hive-mind)
* [Suggested Hardware](#suggested-hardware)

## Part 1: Hive Runner and Hive Scheduler

### Prerequisites

The following software needs to be installed for this guide:

* Ruby and the `bundler` gem
* Git
* MySQL server and the client development library (for Hive Scheduler)
* A Javascript runtime (for Hive Scheduler). Eg, NodeJS

#### Ruby

It is possible to use the version of Ruby installed with the operating system provided it is version 2.1 or later. However, when installing gems it may be necessary to have escalated privileges. For this reason, the recommended method is to use Ruby installed using a version manager such as RVM. Full details can be found at https://rvm.io/. For Ubuntu curl will need to be installed first so the full instructions are:

``` bash
$ sudo apt-get install curl
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
$ \curl -sSL https://get.rvm.io | bash -s stable --ruby
```

Then log out and back or:

``` bash
$ source $HOME/.rvm/scripts/rvm
$ rvm install 2.3
$ rvm use 2.3
```
Note that we have been informed of issues in installations of hive-runner in ruby >= 2.4. So, use ruby < 2.4, preferrably 2.3.0

Additionally, install the bundler gem:

``` bash
$ gem install bundler
```

#### Git, MySQL and the Javascript runtime

On Ubuntu, the remaining requirements can be installed with:

``` bash
# For all components
sudo apt-get install git
# For Hive Scheduler and Hive Mind
sudo apt-get install mysql-server libmysqlclient-dev nodejs
```

The Rails and MySQL database configuration should be set in environment variables for Rails to use. Add the following lines to `~/.bashrc`

```
export RAILS_ENV=production
export DATABASE_HOST=localhost
export DATABASE_USERNAME=<username>
export DATABASE_PASSWORD=<password>
export DATABASE_DATABASE=<database name>
```

Either reload this file with `source ~/.bashrc` or log out and back in.

### Hive Scheduler

Download the Hive Scheduler software with:

``` bash
$ git clone https://github.com/bbc/hive-scheduler
$ cd hive-scheduler
$ bundle install --without development test
```

Set up the database and assets. If the database username is not `root` the first command will ask for the MySQL root password.

``` bash
$ rake db:create
$ rake db:migrate
$ rake db:seed
$ rake assets:precompile
```

Start the Hive Scheduler with:

``` bash
$ rails s
```

After Rails has started up go to `http://localhost:3000` in a browser to see the running scheduler.

### Hive Runner

Install the Hive Runner gem:

``` bash
$ gem install hive-runner
```

and then run the `hive_setup`:

``` bash
$ hive_setup my_hive
```

Leave all the default options and select continue by typing 'x'. After the installation has completed enter the `my_hive` directory and start the Hive Runner:

``` bash
$ cd my_hive
$ hived start
```

Check that the Hive Runner is running:

``` bash
$ hived status

start_hive: running [pid 37175]

  Controllers in use:
    * shell

  Software versions:
    * hive-runner: 2.1.17
    * hive-messages: 1.0.6

  Total number of devices: 5

+---------+--------+-----+--------+--------+
| Device  | Worker | Job | Status | Queues |
+---------+--------+-----+--------+--------+
+---------+--------+-----+--------+--------+
| Shell-1 | 37178  | --- | ---    | bash   |
+---------+--------+-----+--------+--------+
| Shell-2 | 37181  | --- | ---    | bash   |
+---------+--------+-----+--------+--------+
| Shell-3 | 37185  | --- | ---    | bash   |
+---------+--------+-----+--------+--------+
| Shell-4 | 37189  | --- | ---    | bash   |
+---------+--------+-----+--------+--------+
| Shell-5 | 37193  | --- | ---    | bash   |
+---------+--------+-----+--------+--------+
```

If you now go back to `http://localhost:3000` to view the Hive Scheduler, as set up above, and follow the "Monitoring -> Workers" link you can see the five shell workers. Note that the 'Hive' and 'Device' columns will not contain information as this depends on the Hive Mind device database.

### Example test

The example test will find the current date and compare with a day of the week provided as a parameter for the test. This way you can see the behaviour for tests that pass and fail.

Additionally, some system information will be gathered and reported in the log files.

From the Hive Scheduler, http:/localhost:3000, follow the link in the top bar for 'Scripts'. Then press the 'New' button. From the 'Target platform' options select 'Shell Script' and fill in the name 'Day test'. In the template field enter:

```bash
## System information
uname -a
df -h

## Generate error output
ls /tmp/directory_does_not_exist

## Do something to take some time
date
sleep 30
date
sleep 30
date
sleep 30
date

## Create a custom output file
echo Hello World > $HIVE_RESULTS/hello.txt

if [ `date +%a` == $HIVE_DAY ]
then
  exit 0
else
  exit 1
fi
```

In the 'Execution Variables' section select 'New Execution Variable' and enter the field type as 'String' and the name as 'day'. This corresponds to the `$HIVE_DAY` environment variable used in the template.

Press the 'Create Script' button. This will return to the Scripts table and show the newly created script.

Select 'Projects' from the top bar and press the 'New' button. Enter 'Is it this day?' as the name and leave 'Repository', 'Branch' and 'Execution directory' as their default values. In the 'Script' field select the script created above, 'Day test'. Notice that this adds the 'Environment Variables' section to the bottom of the form. Under 'Population Mechanism' select 'Manual', which will add more fields to the 'Environment Variables' section.

Under 'Environment Variables', enter 'bash' in the 'Queues' field and leave all the other options blank.

Press the 'Create Project' button. This show the details of the newly created project.

Select 'Batches' from the top bar and press the 'Create batch' button. Select the project created above, 'Is it this day?' and enter the name, 'Is it Tuesday?', and version, 1.0.

Press 'Next' to enter the execution variables and set 'Day' to 'Tue'. Leave the 'Retries' field blank.

Press 'Next' to view the queue information. There will already be a single value listed, 'bash'.

Press 'Next' to view the jobs information. Leave the options blank. Press 'Finish' to create the batch.

The test should now be running and you can see this firstly by visiting the main 'Batches' page in the Scheduler web interface and secondly on the Hive Runner by executing

```bash
$ hived status

start_hive: running [pid 37175]

  Controllers in use:
    * shell

  Software versions:
    * hive-runner: 2.1.17
    * hive-messages: 1.0.6

  Total number of devices: 5

+---------+--------+-----+---------+--------+
| Device  | Worker | Job | Status  | Queues |
+---------+--------+-----+---------+--------+
+---------+--------+-----+---------+--------+
| Shell-1 | 37178  | --- | ---     | bash   |
+---------+--------+-----+---------+--------+
| Shell-2 | 37181  | --- | ---     | bash   |
+---------+--------+-----+---------+--------+
| Shell-3 | 37185  | 1   | running | bash   |
+---------+--------+-----+---------+--------+
| Shell-4 | 37189  | --- | ---     | bash   |
+---------+--------+-----+---------+--------+
| Shell-5 | 37193  | --- | ---     | bash   |
+---------+--------+-----+---------+--------+
```

Once the test has completed `hived status` will show the test to be `completed` and in the Hive Scheduler batches page it will show the status as 'passed' if today is Tuesday or 'failed' otherwise. By clicking on the batch name 'Is it Tuesday?' you will view the batch view showing a single job. Note that the columns on the right of the table, which would normally give a summary of the test step results, are blank as the test does not submit a results file.

Click on the job id, `#1` in the left-most column of the jobs table to view details about the job. This will display the execution script for the test together with the standard output and error as well as any other output files.

## Part 2: Android Hive Runner

In this section the Hive Runner will be set up to run Android tests using the `hive-runner-android` gem. It will be assumed that the Hive Scheduler will be running on the local machine, as detailed above. If you already have a Hive Runner set up it can be modified to run Android tests. Alternatively, it can be configured during the setup.

Before starting it will be necessary to install the Android Debug Bridge (adb), ZIP, Android Asset Packaging Tool (aapt) and the Java run time environment and development kit. On Ubuntu:

```bash
sudo apt-get install zip openjdk-9-jre-headless openjdk-9-jdk-headless lib32stdc++6 lib32z1
```

Note that `adb` and `aapt` are also available via `apt-get` but there are problems when using multiple devices.

The Android SDK also needs to be installed:

```bash
$ wget http://dl.google.com/android/android-sdk_r24.4.1-linux.tgz
$ tar -xzvf android-sdk_r24.4.1-linux.tgz
$ ./android-sdk-linux/tools/android update sdk --no-ui
```

and then add the following line to `~/.bashrc'

```bash
export ANDROID_HOME=$HOME/android-sdk-linux
export ANDROID_SDK_HOME=$ANDROID_HOME
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
```

The `aapt` binary in `build-tools` also needs to be accessible:

```bash
cd android-sdk-linux/tools
ln -s ../build-tools/23.0.1/aapt .
```

Note that `23.0.1` in the above command may need to be changed depending on your version of Android SDK.

### Modifying an existing Hive Runner

While in the Hive Runner directory, stop the runner with

```bash
$ hived stop
```

and then edit `Gemfile` to add the line

```ruby
gem 'hive-runner-android'
```

and load the new gem

```bash
$ bundle install
```

Edit the file `config/settings.yml` so that it includes:

```yaml
  controllers:
    shell:
      # Number of shell workers to allocate
      workers: 5
      # Queue for each shell worker
      queues:
        - bash
      # Number of ports to allocate to each shell worker
      port_range_size: 50
      name_stub: SHELL_WORKER
    android:
      port_range_size: 10
      name_stub: ANDROID_WORKER
```

Note that YAML file format depends on indentation so it is essential that `android` is indented by the same number of spaces as `shell` and the fields underneath, `port_range_size` and `name_stub` should both be indented by the same number of spaces, more than `android`. Optionally, you may remove the `shell` section if you do not want to execute shell runner tests.

Restart the Hive Runner and ensure that it is running:

```bash
$ hived start
$ hived status
```

The output of `hived status` will show that the Android controller is in use and if an Android device is connected it will be automatically listed. Note the list of queues that are automatically generated based on the make, model and android version information reported via adb.

```
+--------------------+--------+-----+-----------+--------------------+
| Device             | Worker | Job | Status    | Queues             |
+--------------------+--------+-----+-----------+--------------------+
+--------------------+--------+-----+-----------+--------------------+
| Android-SERIAL1234 | 44655  | --- | ---       | xt1068             |
|                    |        |     |           | motorola           |
|                    |        |     |           | android            |
|                    |        |     |           | android-6.0        |
|                    |        |     |           | android-6.0-xt1068 |
+--------------------+--------+-----+-----------+--------------------+
```

### Setting up a new Hive Runner with the Android plugin

Install the Hive Runner gem (if it is not already installed):

```bash
$ gem install hive-runner
```

and then run the `hive_setup`:

```bash
$ hive_setup my_hive
```

Select the option to 'Add module' and type in `android` as the module name. This will add the `hive-runner-android` gem.
Continue the installation by typing 'x'. After the installation has completed enter the `my_hive` directory and start the Hive Runner:

```bash
$ cd my_hive
$ hived start
$ hived status
```

The output of `hived status` will show that the Android controller is in use and if an Android device is connected it will be automatically listed. Note the list of queues that are automatically generated based on the make, model and android version information reported via adb.

```
+--------------------+--------+-----+-----------+--------------------+
| Device             | Worker | Job | Status    | Queues             |
+--------------------+--------+-----+-----------+--------------------+
+--------------------+--------+-----+-----------+--------------------+
| Android-Serial1234 | 44655  | --- | ---       | xt1068             |
|                    |        |     |           | motorola           |
|                    |        |     |           | android            |
|                    |        |     |           | android-6.0        |
|                    |        |     |           | android-6.0-xt1068 |
+--------------------+--------+-----+-----------+--------------------+
```

### Example test

For this example the Calabash Cross Platform Example from https://github.com/calabash/x-platform-example will be used. This test uses the `zip`

Create a new script as in the example above selecting 'Android APK' for the target platform and with the template:

```bash
echo 'gem "res"' >> Gemfile
bundle install

calabash-android resign $APK_PATH

bundle exec calabash-android run $APK_PATH -p android \
          -f Res::Formatters::RubyCucumber2 -o \"$HIVE_RESULTS/out.res\" \
          -f pretty
```

Note:
* Environment variables are available to help running the tests. For example, `$APK_PATH` is the full path of the apk that was uploaded for the test and `$HIVE_RESULTS` is the directory in which any files to be uploaded to the Hive Scheduler should be put.
* The `res` gem, added to the `Gemfile` in the first line, provides the `Res::Formatter::RubyCucumber2` formatter for cucumber. This creates a Res output file for reporting results back to the Hive Scheduler when `-f Res::Formatter::RubyCucumber2 -o \"$HIVE_RESULTS/out.res\"` is added to the cucumber or calabash command.

Give the script a name such as 'Calabash Android'. No additional execution variables are required.

Create a new project using this script and set the fields:

* Name: Calabash Cross Platform Example
* Repository: `https://github.com/calabash/x-platform-example.git`
* Population mechanism: Manual
* Queues: `android`
 * Note that this will run on any android device. This can be made more specific using one of the automatically generated queues from the output of `hived status` above.
* All other fields should be left as the default.

Create a new batch with this project. Download the pre-built apk from https://github.com/calabash/x-platform-example/tree/master/prebuilt and submit it in the 'Build information' tab. All other fields can be left as the default.

## Part 3: iOS Hive Runner

In this section the Hive Runner will be set up to run iOS tests using the `hive-runner-ios` gem. It will be assumed that the Hive Scheduler will be running on the local machine, as detailed above. If you already have a Hive Runner set up it can be modified to run iOS tests. Alternatively, it can be configured during the setup.

Before starting it will be necessary to install XCode, ilibmobiledevice and
ideviceinstaller.

```bash
xcode-select --install
# Then select 'Install' from the pop-up and accept the license

# Install brew if necessary. See https://brew.sh/

# --HEAD is required until version 1.2.1 is officially released
brew install --HEAD libimobiledevice

brew install ideviceinstaller
```

The version of Ruby that comes installed with MacOS Sierra is 2.0.0 and so a newer version needs to be installed. For information about installing the latest version see https://rvm.io/rvm/install

### Modifying an existing Hive Runner

While in the Hive Runner directory, stop the runner with

```bash
$ hived stop
```

and then edit `Gemfile` to add the line

```ruby
gem 'hive-runner-ios'
```

and load the new gem

```bash
$ bundle install
```

Edit the file `config/settings.yml` so that it includes:

```yaml
  controllers:
    shell:
      # Number of shell workers to allocate
      workers: 5
      # Queue for each shell worker
      queues:
        - bash
      # Number of ports to allocate to each shell worker
      port_range_size: 50
      name_stub: SHELL_WORKER
    ios:
      port_range_size: 10
      name_stub: IOS_WORKER
```

Note that YAML file format depends on indentation so it is essential that `ios` is indented by the same number of spaces as `shell` and the fields underneath, `port_range_size` and `name_stub` should both be indented by the same number of spaces, more than `ios`. Optionally, you may remove the `shell` section if you do not want to execute shell runner tests.

Restart the Hive Runner and ensure that it is running:

```bash
$ hived start
$ hived status
```

The output of `hived status` will show that the iOS controller is in use and if an iOS device is connected it will be automatically listed. Note the list of queues that are automatically generated based on the make, model and ios version reported by `ideviceinfo`. 

```
+----------------+--------+-----+-----------+------------------------+
| Device         | Worker | Job | Status    | Queues                 |
+----------------+--------+-----+-----------+------------------------+
+----------------+--------+-----+-----------+------------------------+
| Ios-SERIAL1234 | 11391  | --- | ---       | iphone_6_plus          |
|                |        |     |           | ios                    |
|                |        |     |           | ios-10.2               |
|                |        |     |           | ios-10.2-iphone_6_plus |
|                |        |     |           | iphone-10.2            |
+----------------+--------+-----+-----------+------------------------+
```

### Setting up a new Hive Runner with the iOS plugin

Install the Hive Runner gem (if it is not already installed):

```bash
$ gem install hive-runner
```

and then run the `hive_setup`:

```bash
$ hive_setup my_hive
```

Select the option to 'Add module' and type in `ios` as the module name. This will add the `hive-runner-ios` gem.
Continue the installation by typing 'x'. After the installation has completed enter the `my_hive` directory and start the Hive Runner:

```bash
$ cd my_hive
$ hived start
$ hived status
```

The output of `hived status` will show that the iOS controller is in use and if an iOS device is connected it will be automatically listed. Note the list of queues that are automatically generated based on the make, model and ios version information reported by ideviceinfo.

```
+----------------+--------+-----+-----------+------------------------+
| Device         | Worker | Job | Status    | Queues                 |
+----------------+--------+-----+-----------+------------------------+
+----------------+--------+-----+-----------+------------------------+
| Ios-SERIAL1234 | 11391  | --- | ---       | iphone_6_plus          |
|                |        |     |           | ios                    |
|                |        |     |           | ios-10.2               |
|                |        |     |           | ios-10.2-iphone_6_plus |
|                |        |     |           | iphone-10.2            |
+----------------+--------+-----+-----------+------------------------+
```

## Part 4: Hive Mind

Hive Mind is used to manage devices connected to the hive. Whilst it is not essential to use Hive Mind, it is useful for displaying the following information:
* Which devices are connected to which Hive Runners
* When devices are offline or experiencing other problems
* Hive Runner logs for devices
* Diagnostics details such as uptime and battery temperature

Additionally, Hive Mind can be used to create custom queues.

Check out the Hive Mind repository:

```bash
$ git clone https://github.com/bbc/hive_mind
$ cd hive_mind
$ bundle install --without development test integration
```

Normally Hive Mind and Hive Scheduler would be run on different servers. For the moment they will both use the same details as set in `~/.bashrc`, above, but the Hive Mind database name will be given explicitly with each command.

```bash
$ DATABASE_NAME=hivemind rake db:create
$ DATABASE_NAME=hivemind rake db:migrate
$ DATABASE_NAME=hivemind rails s
```

If you now visit http://localhost:3001 you can view the home page of Hive Mind with no devices registered. Leave Hive Mind running while proceeding on to configure Hive Mind and Hive Scheduler.

### Connect Hive Runner to Hive Mind

Stop the Hive Runner if it is running:

```bash
$ cd my_hive
$ hived stop
```

Find the following section in `config/settings.yml`:

```yaml
  network:
    scheduler: http://localhost:3000
    #hive_mind: http://localhost:3001
```

and uncomment the `hive_mind` line. Restart the Hive Runner:

```bash
$ hived start
```

Visiting http://localhost:3001 will now show two device types, Hive and Mobile. Clicking on either of these will show a list of manufacturers, BBC in the case of Hive, and then clicking on the manufacturer will show all models and devices. Clicking on a device will display the full details.

Note that the default names are given to devices; the hostname for a Hive and the device model for a mobile. These names can be changed from the device details page.

### Connect Hive Scheduler to Hive Mind

The connection to Hive Mind from Hive Scheduler is defined in `config/settings.yml` in Hive Scheduler. Ensure that the default or production sections have

```yaml
  hive_mind:
    url: http://<ip address>:3001
```

and restart Hive Scheduler. Note that the setting requires an IP address or hostname even if Hive Scheduler and Hive Mind are running on the same server. This is because it is used for showing links from the Hive Scheduler batch and job pages to the device details in Hive Mind.

Rerun the same tests created above and you will see the device name listed on the batch page once it has started running. 

## Suggested Hardware

It is recommended that Ubuntu PCs should be used for Android Hive Runners and Macs should be used for iOS Hive Runners. The Hive Scheduler and Hive Mind would normally be on separate servers and may be hosted in the cloud.

For the Android Hive we use [Lenovo IdeaCentres Q190](http://shop.lenovo.com/gb/en/desktops/lenovo/q-series/q190/) upgraded to 8G memory. Whilst these are no longer being made by Lenovo it can be used as a guide.

For USB hubs, [D-Link DUB-H7](http://www.dlink.com/uk/en/home-solutions/connect/usb/dub-h7-7-port-usb-2-0-hub) and [Kensington Charging Cabinets](https://www.kensington.com/en/gb/4716/charge-sync-tablet-cabinets) both work well.
