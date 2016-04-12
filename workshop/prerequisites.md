---
title: Workshop prerequisites
layout: default
---

# Workshop Prerequisites

{: .lead}
This page contains information about software that needs to be installed in
advance of the Hive Workshop or can be pre-installed to avoid time (and network
problems). Suggested instructions are given for installing a Hive on Ubuntu but
alternative methods may be used if desired (for example, `rbenv` in place of
`rvm`).

On Mac it might be necessary to install 
<a href="/hive-ci/workshop/mac-preinstall.html">a few other things.</a>

## Ruby

The Hive requires Ruby version 2.0 or later. You can test the version of Ruby
you have installed with:

{% highlight bash %}
ruby -v
{% endhighlight %}

If Ruby is not installed or the version is too old then it can be installed
with [RVM](https://rvm.io/rvm/install):

{% highlight bash %}
# Install the latest version of Ruby with RVM
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable --ruby
source /home/jhaig/.rvm/scripts/rvm
gem install bundler
{% endhighlight %}

Some Ruby components will be installed during the workshop. This can be done in
advance

## Downloading Hive Scheuler, Hive Mind and Test Mine

Hive Scheduler, Hive Mind and Test Mine are Rails applications and can be
downloaded from Github with one of the following three methods.

### With Git

{% highlight bash %}
mkdir hive
cd hive

git clone https://github.com/bbc/hive-scheduler
git clone https://github.com/bbc/hive_mind
git clone https://github.com/bbc/testmine
{% endhighlight ^%}

### As a Tarball package

{% highlight bash %}
mkdir hive
cd hive

# Download
curl -L -o hive-scheduler.tar.gz https://github.com/bbc/hive-scheduler/tarball/master
curl -L -o hive_mind.tar.gz https://github.com/bbc/hive_mind/tarball/master
curl -L -o testmine.tar.gz https://github.com/bbc/testmine/tarball/master

# Unpack
tar -xzvf hive-scheduler.tar.gz
tar -xzvf hive_mind.tar.gz
tar -xzvf testmine.tar.gz

# Change to sensible directory name
mv bbc-hive-scheduler* hive-scheduler
mv bbc-hive_mind* hive_mind
mv bbc-testmine* testmine
{% endhighlight ^%}

### As a Zip package

{% highlight bash %}
mkdir hive
cd hive

curl -L -o hive-scheduler.tar.gz https://github.com/bbc/hive-scheduler/zipball/master
curl -L -o hive_mind.tar.gz https://github.com/bbc/hive_mind/zipball/master
curl -L -o testmine.tar.gz https://github.com/bbc/testmine/zipball/master

# Unpack
unzip hive-scheduler.zip
unzip hive_mind.zip
unzip testmine.zip

# Change to sensible directory name
mv bbc-hive-scheduler* hive-scheduler
mv bbc-hive_mind* hive_mind
mv bbc-testmine* testmine
{% endhighlight ^%}

## Preparing Hive Scheduler, Hive Mind and Test Mine

The Ruby gems required for the three Rails applications can be installed with:

{% highlight bash %}
cd hive-scheduler
bundle install
cd ..

cd hive_mind
bundle install
cd ..

cd testmine
bundle install
cd ..
{% endhighlight %}


## Hive Runner

The Hive Runner is installed as a gem:

{% highlight bash %}
gem install hive-runner
{% endhighlight %}

This is installed as part of the workshop but it can take some time so it is
useful to install in advance.

## Java

On Ubuntu, ADB requires Sun Java which can be installed as:

{% highlight bash %}
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
sudo apt-get install oracle-java7-set-default
{% endhighlight %}

## ADB

To run Android tests you will need to have ADB installed. Note that the version
that comes with Ubuntu will cause problems so use this instead:

* Download the latet version of the SDK from [http://developer.android.com/sdk/index.html](http://developer.android.com/sdk/index.html)
* Unzip and move to `/opt/adt/sdk`
* Run `/opt/adt/sdk/tools/android update sdk --no-ui` to download the platform
  tools. This will install all platforms and so may take a while.
* Add the following environment variables into your `~/.bash_profile` (or
  equivalent):

{% highlight bash %}
export PATH=$PATH:/opt/adt/sdk/tools
export PATH=$PATH:/opt/adt/sdk/platform-tools
export ANDROID_HOME=/opt/adt/sdk
export ANDROID_SDK_HOME=/opt/adt/sdk
{% endhighlight %}

## Javascript

Rails requires a JavaScript runtime. For example,

{% highlight bash %}
sudo apt-get install nodejs
{% endhighlight %}

## Set up for Calabash tests

Install Calabash:

{% highlight bash %}
gem install calabash-android
{% endhighlight %}
