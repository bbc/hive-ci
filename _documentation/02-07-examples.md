---
layout: documentation
title: Examples
permalink: documentation/examples.html
menu_level: 2
---

{: .lead}
This page contains some examples to show how tests may be set up and executed.

## Contents
* [iOS](#ios)
  * [XCode UI Tests](#xcode-ui-tests)

## iOS

### XCode UI Tests

Since XCode 8 it has been possible to create a build for testing that can then be zipped up into a package and used in a Hive batch. The method for creating the build is shown below but first the project in Hive Scheduler needs to be set up.

On the Hive Scheduler, create a new test script with the 'iOS IPA' targed with the template:

```bash
# Copy the uploaded build and rename as a zip file
cp $APP_PATH your_project.zip

# Unpack
unzip your_project.zip

cd Products

# Find the xctestrun file
TEST_FILE=`ls *.xctestrun`
if [ x$TEST_FILE == 'x' ]
then
  echo No xctestrun file found >> $HIVE_SCRIPT_ERRORS
  exit 1
else
  # Execute the tests
  xcodebuild -xctestrun "$TEST_FILE" \
             -destination "platform=iOS,id=$DEVICE_TARGET" \
             test-without-building > raw_output.txt
  XC_EXIT=$?

  # Generate a JUnit and xcpretty results files
  cat raw_output.txt | xcpretty --report junit > xcpretty.out

  # Convert the JUnit result file into RES format for uploading to the Hive
  res --junit build/reports/junit.xml
end

# Copy all results files to the results directory for uploading
cp raw_output.txt xcpretty.out *.res $HIVE_RESULTS

# Report an error if XCodeBuild failed to run
if [ $XC_EXIT -ne 0 ]
then
  echo XCodeBuild exited with error >> $HIVE_SCRIPT_ERROR
fi
```

Create a project using this script. It is not necessary to specify a source repository or branch and the execution directory should be set to `.`. Remember to set the 'Population Mechanism' as appropriate as well as any default queues.

Now that the project has been set up in Hive Scheduler, create a 'build for testing' from your XCode project:

```bash
xcodebuild -workspace your_project.xcworkspace \
           -scheme 'your test schema' \
           -destination 'generic/platform=iOS' \
           clean build-for-testing
```

The destination `generic/platform=iOS` indicates that the tests can be executed on any suitably provisioned device that satisfies the requirements of the project.

The `build-for-testing` and `test-without-building` actions in this command and the script created above together make up the `test` action that can be used with xcodebuild for compiling and executing on a connected device. You can try the `test-without-building` command as shown in the script above to verify this.

The above command will create a build inside the `DerivedData` directory together with an xctestrun file. These should be packaged up with `zip`.

```bash
cd DerivedData/your_project/Build
ls Products
# =>    your_project.xctestrun   Debug-iphoneos/

zip -r your_project.zip Products
# The Hive Scheduler currently requires iOS build files to have an ipa extension
mv your_project.zip your_project.ipa
```

The `your_project.ipa` file is the build to be uploaded to the Hive Scheduler when creating the batch.


You can now execute the project using the `your_project.ipa` file generated above as the build.

Note that all the commands for creating and packaging the build can be built into a CI pipeline and the Hive Batch can then be triggered with:

```bash
# PROJECT_ID = Project Id from Hive Scheduler
curl http://your_hive_scheduler_url/api/batches \
     --form version=1.0 \
     --form project_id=$PROJECT_ID \
     --form batch[name]='My iOS project' \
     --form build=your_project.ipa \
     --form execution_variables[queues]='queue_1,queue_2,queue_3'
```
