---
layout: documentation
title: Appium
permalink: documentation/appium.html
---

# Running Appium Tests

{: .lead}

Appium can be used to execute tests on both web and native apps on Android and iOS devices 

1. For responsive websites, existing selenium scripts can be easily ported to run on 
mobile browser supported by Appium.

2. Native app tests needs to be written in usual way. You need to upload build in 'Create Batch' section on Hive Scheduler.

To run both above just point tests to Appium server and access few global variables.

### Install Appium
1. Pre-requisite are npm and node (Install npm and node/nodejs using brew (on mac) or apt-get (on linux server))
	
	* `sudo apt-get install npm`
	* `sudo apt-get install node`

	Note: In some cases we require nodejs to start appium (particularly on mac)

2. Install appium 

	* `npm install appium`

### Global/Environment variables  
1. 	Access below varibles in Hive Script and test script as well as and when required

	* `APPIUM_PORT` - Appium port to listen on   
	* `CHROMEDRIVER_PORT` - Port upon which ChromeDriver will run
	* `BOOTSTRAP_PORT`  - (Android-only) port to use on device to talk to Appium
	* `ADB_DEVICE_ARG` - Unique device identifier of the connected physical device
	* `APK_PATH` - Uploaded build in 'Create Batch' section is accessible here
	* `HIVE_RESULTS` - Hive result directory. Any dumped output here would be accessible in jobs log section

### Starting an Appium server in Hive Script
	
	/path/to/appium/.bin/appium -U $ADB_DEVICE_ARG --port $APPIUM_PORT --bootstrap-port $BOOTSTRAP_PORT --chromedriver-port $CHROMEDRIVER_PORT > $HIVE_RESULTS/appium.log 2>&1 &

This should start appium server on port $APPIUM_PORT. In the test script use RemoteWebDriver address as http://0.0.0.0:$APPIUM_PORT.

**Note**: This is just a pointer and different appium server capabilities can be used either while starting server or can be passed from DesiredCapabilities