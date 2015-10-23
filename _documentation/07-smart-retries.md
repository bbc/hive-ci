---
layout: documentation
title: Smart Retries
permalink: documentation/smart-retries.html
---

# Smart retries

{: .lead}
Getting HiveCI to understand your tests and reschedule just your failures.

Device-based testing is by its nature prone to random failures and flakiness. We
got fed up very fast of having to re-run an entire test suite in order to see
everything fail. With smart retries you can hook into the Hive's concept of a
test and ensure that only the tests that fail are retried.

## RES

The RES library is central to how Hive CI understands tests. RES is a standard
output format for test result resports. It captures more than just basic pass/fail
information. It captures the context of how your tests were run, and lets you
include urns to drive what your retry arguments look like.

The hive runners will look in the results directory at the end of your test run
to see if a .res file has been deposited. If your res file includes retry urns
or test names then these will be passed to the retry of your suite in the form
of environment variables.