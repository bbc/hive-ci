---
layout: documentation
title: Appium
permalink: documentation/appium_test.html
menu_level: 2
---

# Running Appium Tests

{: .lead}

Appium can be used to execute tests on both web and native apps on Android and iOS devices 

1. For responsive websites, existing selenium scripts can be easily ported to run on 
mobile browser supported by Appium.

2. Native app tests needs to be written in usual way. You need to upload build in 'Create Batch' section on Hive Scheduler.

To run both above just point tests to Appium server and access few global variables.

### Install Appium and Dependencies
1. Pre-requisite are npm and node (Install npm and node/nodejs using brew (on mac) or apt-get (on linux server))
	
	* `sudo apt-get install npm`
	* `sudo apt-get install node`

	Note: In some cases we require nodejs to start appium (particularly on mac)

2. Install appium command

	* `npm install appium`

	Note: `npm install appium@version` will install specific appium version

3. Install [ios_webkit_debug_proxy](https://github.com/google/ios-webkit-debug-proxy) (for ios only)

### Global/Environment variables  
1. 	Access below variables in Hive Script and test script as and when required

	* `APPIUM_PORT` - Appium port to listen on   
	* `CHROMEDRIVER_PORT` - Port upon which ChromeDriver will run (Temporarily used in ios webkit proxy)
	* `BOOTSTRAP_PORT`  - (Android-only) port to use on device to talk to Appium
	* `ADB_DEVICE_ARG` - Unique device identifier of the connected physical android device
	* `APK_PATH` - Uploaded build in 'Create Batch' section is accessible here
	* `DEVICE_TARGET` - Unique device identifier of the connected physical ios device
	* `APP_BUNDLE_PATH` - Uploaded ios build in 'Create Batch' section is accessible here
	* `HIVE_RESULTS` - Hive result directory. Any dumped output here would be accessible in jobs log section

### Starting an Appium server 

***Android***

	/path/to/appium/.bin/appium -U $ADB_DEVICE_ARG --port $APPIUM_PORT --bootstrap-port $BOOTSTRAP_PORT --chromedriver-port $CHROMEDRIVER_PORT > $HIVE_RESULTS/appium.log 2>&1 &

***IOS (on Mac)***

	node /opt/appium/node_modules/.bin/appium -U $DEVICE_TARGET --port $APPIUM_PORT --bootstrap-port $BOOTSTRAP_PORT  --webkit-debug-proxy-port $CHROMEDRIVER_PORT --tmp /tmp/ios/$HIVE_JOB_ID > $HIVE_RESULTS/appium.log 2>&1 &

This should start appium server on port $APPIUM_PORT. In the test script use RemoteWebDriver address as http://0.0.0.0:$APPIUM_PORT.

### Starting webkit proxy (IOS only)
	
	ios_webkit_debug_proxy -c $DEVICE_TARGET:$CHROMEDRIVER_PORT -d > $HIVE_RESULTS/ios_webkit.log &	

**Note**: These are just pointers and different appium server capabilities can be used either while starting server or can be passed from DesiredCapabilities
