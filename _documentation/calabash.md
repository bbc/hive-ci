---
layout: documentation
title: Calabash
permalink: documentation/calabash.html
---

# Running Calabash Tests

{: .lead}

Calabash is used to perform automated tests for native Android and iOS apps

1. While creating Hive script select appropriate Android or iOS as target platform 

### Global/Environment variables  
1. 	Access below varibles in Hive Script and test script when required

	* `ADB_DEVICE_ARG` - Unique device identifier of the connected physical device
	* `APK_PATH` - Uploaded android build in 'Create Batch' section is accessible here
	* `APP_BUNDLE_PATH` - Uploaded ios build in 'Create Batch' section is accessible here
	* `HIVE_RESULTS` - Hive result directory. Any dumped output here would be accessible in jobs log section

### Executing calabash tests for Android
	
	bundle exec calabash-android run $APK_PATH -f Res::Formatters::RubyCucumber -o \"$HIVE_RESULTS/out.res\" -f pretty > $HIVE_RESULTS/pretty.out 2>$HIVE_RESULTS/stderr.log

### Executing calabash tests for iOS
	
	bundle exec calabash-android run $APP_BUNDLE_PATH -f Res::Formatters::RubyCucumber -o \"$HIVE_RESULTS/out.res\" -f pretty > $HIVE_RESULTS/pretty.out 2>$HIVE_RESULTS/stderr.log
	
We can use cucumber as well for execution of calabash tests for Hive Script in following way

**Android**
	
	calabash-android build $APK_PATH
	export TEST_SERVER=`ls test_servers/`
	bundle exec cucumber ADB_DEVICE_ARG=$ADB_DEVICE_ARG APP_PATH=$APK_PATH TEST_APP_PATH=test_servers/$TEST_SERVER -f Res::Formatters::RubyCucumber  -o $HIVE_RESULTS/out.res -f pretty > $HIVE_RESULTS/pretty.out 2>$HIVE_RESULTS/stderr.log

**IOS**

	bundle exec cucumber APP_BUNDLE_PATH=/path/to/test.app -f Res::Formatters::RubyCucumber -o $HIVE_RESULTS/out.res -f pretty > $HIVE_RESULTS/pretty.out 2>$HIVE_RESULTS/stderr.log 

**Note**: Include [res](https://github.com/bbc/res) gem in your gemfile 
