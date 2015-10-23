---
layout: documentation
title: About
permalink: documentation/hive-ci.html
---

{: .pull-right}
![Loads of Devices](/hive-ci/images/pile-of-devices.png)

# Introducing Hive CI

{: .lead}
At the BBC we build a large number of TV and mobile applications. In order to
ensure the widest posible audience reach we have to test these applications
across hundreds of different device models.

This is enormously expensive and
slows down our abilty to push new features out to our audiences. In
order reduce the manual testing effort, we invested heavily in automation
and became very good at writing automated tests to help alleviate the testing
effort. This only helped so much; although we had good automation suites,
actually running them was an expensive manual job -- locating devices, setting
up test environments, executing the scripts, and collating and interpreting the
results.

*Hive CI* was born out of a desire to entirely eliminate the manual effort of
running our automated tests. Conventional CI platforms weren't really a good
fit for what we were trying to do. We wanted a CI system that could understand
and manage devices, run our tests without manual intervention, and collate and
interpret results without having to resort to spreadsheets.

