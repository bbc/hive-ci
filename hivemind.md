---
layout: default
title: Hive Mind
---

<h1 class="pull-right"><span class="label label-grey">BETA</span></h1>

# Hive Mind

{: .lead}
**A device monitoring and inventory application for Hive-CI**

<img src="/hive-ci/images/hivemind-brands.png" class="col-md-6 pull-right img-responsive" alt="Hive CI interface">  

<br />
<br />

Hive Mind allows you to administer your hives and devices:

* Specify which devices run which type of tests
* Look up device information
* Spot problems with devices

It also serves as an inventory system for your office devices.

<br />
<br />
<br />
<br />


## Getting started

HiveMind is a beta system at the moment. We are just switching to using it internally from
our old device management database. You're welcome to play with it but be aware we're working
on it right at this moment!

You will need to install a recent version of ruby.

To get started, check out the rails application:

    git clone git@github.com:bbc/hive_mind.git
    cd hive_mind
    
You will need to do some basic configuration to get going. Open up the Gemfile, and add the following:

    gem 'hive_mind_mobile', git: 'https://github.com/bbc/hive_mind_mobile'
    
This will enable the mobile engine. Now run:

    bundle install --without integration
    
This will install the dependencies. Finally run:

    bundle exec rake db:migrate

This will set up the database. You're running in development mode, which means you're using a sqlite
database. If you want to configure a different database. You can modify config/database.yml. We use
a mysql database so that is the default for production hivemind.

Finally, start your application by running:

    bundle exec rails s
    
This will start hive_mind on port 3001 on your machine. Visit http://localhost:3001.

There's not much to see at the moment. You need to start getting devices reporting in!

<br />

## Connecting up to your hive runners

Make sure your hive runners are up to date. You can add your local HiveMind to the runner config
to get your hives reporting into the system. Edit the config/settings.yml file in each of your
runner instances, adding in the hive_mind url to your network config:

    network:
      hive_mind: http://localhost:3001

Restart your hives:

    hived stop
    hived start

Your hives should start reporting in within 30 seconds. Visit http://localhost:3001/brands/1 to
see your BBC branded hives checking in.

<br />
<br />

## Connecting up to your scheduler

Add the following config to your bash profile:

    export HIVE_MIND_URL=http://<your-ip-address>:3001

Restart your scheduler. When your runners report test results to the hive, they will now also
report in the device id from HiveMind, giving you more details about your devices.