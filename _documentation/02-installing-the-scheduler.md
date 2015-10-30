---
layout: documentation
title: Installing the Scheduler
permalink: documentation/installing-hive-scheduler.html
---

# Installing the Scheduler

{: .lead}
The scheduler is the main web application that drives Hive CI. It is where you
define your tests, and schedule them to run on devices. The results of your
test runs get reported back into the scheduler after they have run.

## Installation instructions

### Pre-requisites

Hive Scheduler is a rails application. It requires at least ruby 2.2 and
requires a mysql database. You will need to set up a mysql database, and install
the mysql client libraries. For example on Ubuntu:

    sudo apt-get install libmysqlclient-dev

We recommend using git to download the code and manage updates. Pull down the
code from github and checkout the latest stable tag:

    git clone	git@github.com:bbc/hive-scheduler.git
    git co stable

Install the gem dependecies for the application:

    cd hive-scheduler
    bundle install

### Configuration

Hive-Scheduler is configured using the Chamber gem. You can add configuration directly
into the config/settings.yml file, or by setting the following environment variables (in
the following lines to `~/.bashrc` (or equivalent):

    export RAILS_ENV=production
    export DATABASE_ADAPTER=mysql2
    export DATABASE_ENCODING=utf8
    export DATABASE_HOST=your_database_host
    export DATABASE_PORT=3306
    export DATABASE_POOL=5
    export DATABASE_USERNAME=database_username
    export DATABASE_PASSWORD=database_password
    export DATABASE_NAME=database_name
    export ATTACHMENT_STORAGE=filesystem
    export ATTACHMENT_STORAGE_PATH_BASE=public

### Set up database

    ./bin/rake db:migrate
    ./bin/rake db:seed

### Start the server

We recommend using bundler to manage the gem dependecies for the installation:

    bundle install
    bundle exec rails s

Your application should start on port 3000. Note you can't run any tests until
you've set up a runner for the devices you want to connect.



