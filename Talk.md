# Shedding Light on Black Box Services

---

# Overview

Will discuss   Logging, Metrics, Monitoring, and Alerting

* Why you should care
* Help understand the fundamentals
* Discuss options of each concept
* Pros and cons of the options
* Technologies that support these concepts

---

# Talk Objectives

I WANT you to WANT these things:

* Identify - Proactively identify outages and issues
* Prevent - Limit/eliminate impact of issues
* Improve - Increase the resilience and robustness of systems

In other words, your system should tell you:

* when your system has a problem
* where an issue exists
* why it's occuring

---

# These Ideas 

Are not mine.... 

These are the best two sources I've found:

* **Prometheus documentation** - a metrics, monitoring, and alerting open source tool set
    * great docs to understand metrics basics
* **Site Reliability Engineering** - an open source book by Googlers
    * Site Reliability Engineers (SRE's) at Google are responsible for:
        * Keeping services up and running.
        * Make them more robust
    * (Uber also has a similar team)

???

Plenty of literature - not all is helpful

---

# Why would you want to do this? 

Where's the value in metrics?

---

# Story

'Customer' walks up and says: 

"The X site isn’t responding!!!"

---

My first question is: 

Why didn’t you know there was an issue in the first place?

In any case...

---

So, you spend a couple minutes in utter disbelief making excuses: 

**“It can’t be us!”**

**"Its working fine for me!"**

![Works fine for me](https://cdn.meme.am/cache/instances/folder332/72583332.jpg)

---

Try to find the issue by digging through logs...

![Dog Digging](https://miloblog.s3.amazonaws.com/2016/Jun/Digging-1465401349389.jpg)

---

Finally, make a recommendation on the 'cause'

![Its not my fault](http://rebootauthentic.com/wp-content/uploads/2013/10/responsibility.png)

---

Lets prevent this. 

Lets use good engineering practices.

---

# What can we improve

* Let's proactively notify our customers of issues
* Be notified of issues before customers report them
* Identify problem areas quickly
* Dig into causes quickly
* Strategically make improvements after failure

???

What can we do  to improve our systems?

---

# Methodologies 

Start with well known concepts or methodologies. Then identify technologies.

### Problem

The available technologies blur the lines. So we need to understand boundaries of concepts.

???

Talk NOT about Technology, rather have understanding of concepts


---

# Concepts

Problem: A system was down, but we weren't aware of it.

Lets work our way backwards:

* Need to receive a notification
    * Alerting - Helps us know an issue has occurred (Email, Phone call, etc) 

* Alert trigger: need to detect *when* something goes wrong
    * Monitoring - a **threshold** has been breached

* Need 'numbers' to calculate threshold
    * Metrics - aggregates the *what*, *when*, and *where*

* Once area is identified as a problem area, how do we determine a cause?
    * Logs - Show you details of *what* happened, hopefully a *why* can be inferred

---

## STOP

Don't be intimidated....

> KISS - Keep it stupid simple

Don't try and do everything at once.

Take it one step at a time and make small iterative improvements.

???

How are we going to build this into our systems?

---

## Concepts

When implementing, start at the top, work down...

* Logging - add basic logging
* Metrics - simple tracking metrics (Four Golden Signals, REDs)
* Monitoring - create calculations, thresholds
* Alerting - get notifications

---

# Let's talk about ....

---

# Logging

---

# Purpose of Logging

(This may be basic, but it is critical to understand)

WHEN

WHERE

WHY

Context

Understand why failures are occurring in a system

---

# What is Logging 

There are two parts: a 'log' and an 'entry' in the log

* The log:
    * a set of events or activities that have occurred over time

* The log entry:
    * Is when something occurred
    * Data about the event
    * Often includes a ‘severity level'

---

# Example

An Apache Web Server Log:

```
// host - user [utcdate] “METHOD /url/resource.file protocol” response filesize

127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326
```

Answers: What was accessed, when, who, and provides context

---

# Purpose

(This will help us make a distinction between logging and metrics)

Gather detailed information about:

* WHEN an event occurred
* WHAT happened when the event occurred (context)
* WHERE it happened

---

# Not the Purpose:

* What the system as a whole is doing
* How much the system is doing
* High level view of system activity

???

Should not expect logging to provide these things

---

# Log Levels:

* Fatal
* Error
* Warning
* Information
* Trace

???

* Fatal - When process has crashed (Full client, NodeJs)

* Error - Significant error has occurred. Keep an eye on these.

* Warning - Shouldn't occur, but not sure how to handle (Try/Catch)

* Information - Comprises most logs, can help understand failures

* Trace - 'Debugging' in Prod

---

# Usage

* During development we mainly use: Information, Warnings and Errors
* Once operational, maybe only log Warnings
* Debugging Prod, use Trace - understand why the errors are occurring
* In high volume system, only capture Errors. 

---

# Strategies

* Log to Disk, with utility to move logs to more permanent location
    * Pros: Reduces network traffic, logs don’t get lost during ‘log service’ outage
    * Cons: Increase disk IO and need another job to persist
* Log directly to a service 
    * Pros: Immediate log feedback (great for debugging)
    * Cons: Increased network traffic

---

# Production Issues to Consider

* Availability - What happens when log service is unavailable?
* Performance Impact - heavy logging impacts disk and/or network
* Be cautious of hand rolled HTTP solutions: if remote site is unavailable, the connection timeout can cause a PROD system to grind to a halt. (I’ve seen it, twice, on the same system.)
* Industry convention says: "Do not build your own logging library", there are many very good frameworks. They've found all the edge cases and failure paths (See StackOverflow)
* Logging system goes down - For example, pushing all logs to a database could overwhelm a database and so overall performance of the system (or other systems using that logging database could be degraded).

---

# Technologies:

* Log4Net - derived from very popular Java library
* System.Diagnostics.Trace - .Net framework
* GreyLog - logs to disk, separate process sends to a log aggregator service
* Log Database - sends log directly to a database
* Splunk - send logs directly to a log service
* Application Insights - has some ‘logging’ capabilities - not recommended for logging, its the wrong tool.

---

# Splunk Log Message Example

```
{
  "id": "0",
  "severity": "Information",
  "data": [
    {
      "Source": "Telematics.Jobs.BuzzerJobProxy",
      "Timestamp": "2017-07-19T21:52:32.2617467Z",
      "CorrelationId": "e0a4cff0-46ea-41a4-8a4c-24a2032a3614",
      "Message": "Recieved Event",
      "Payload": {
        "Command": "MaxSpeedSet",
        "DeviceKey": "971916dd-1bd5-4369-a92b-aea71b20116a",
        "Vin": "3FA6P0H94ER360202"
      }
    }
  ]
}
```

---

# Summary

What does logging accomplish: 

> Something happened: what was it, where, when, and some **details**.

---

# Metrics

> "Monitor Every Thing!"
> 
>    -Etsy

???

Nice idea... but

---

# Remember

> KISS - Keep it stupid simple  

Don't try and do everything at once.

???

I'll provide some guidelines on where to start

---

# Purpose

Metrics allow you to:

* Help identify problem areas
* Give high level overview of 'what the system is doing’ (but not why)

---


# Types of Metrics

* Counter
    * Only can go up: 1,2,4,10,25
    * Example: times a URL has been hit, or 500s returned
* Gauge
    * Fluctuates like a speedometer
    * Example: Requests per second
* Histogram
    * Counts observations and puts them in buckets
    * Request durations (50 @ < 250ms, 10 @ < 500ms, etc)
    * Averages are not sufficient

---

# Example of a Counter Metric

```
api_http_requests_total{method="POST", url="/messages”} count=50

        metric name     set of properties(dimensions)    value
```

(Contrast this with logging: this is small and compact data)

---

# Attributes of Metrics vs Logs

Metrics are:

* Small - in data size, compared to logs
* Viewed in Aggregate - single log can be valuable, single metric is worthless
* Structure - simpler, more concise, more structured than a log

---

# Objectives of Metrics

* Pros
    * Light weight - very little metadata included
    * Shows 'higher level view' of system
* Cons
    * Not good for debugging - No detailed cause of issues
    * No 'why'

Note:

* You CAN get metrics from logs, but remember: Separation of Concerns.

* Lots of logs prevent scaling up. But metrics just mean a larger ‘int’ value, which is very scalable.

---

# The Four Golden Signals (SRE Book)

If you can only measure four metrics of your user-facing system, focus on these four:

* Latency - The time it takes to service a request. A slow error is even worse than a fast error! Therefore, it’s important to track error latency, as opposed to just filtering out errors.
* Traffic - A measure of how much demand is being placed on your system.
* Errors - The rate of requests that fail
* Saturation - How "full" your service is. Saturation helps predict impending issues.

---

# REDs

What to measure in your system?

| | |
|-|-|
| **R**equest Rate             | How busy is my service?             |
| **E**rror Rate               | Are there any errors in my service? |
| **D**uration of requests     | What is the latency in my service?  |


[1] https://www.slideshare.net/weaveworks/monitoring-weave-cloud-with-prometheus

---

# Metrics Technologies

* Prometheus - Great documentation on what to think about when talking about metrics
* Splunk - Create metrics/reports off logs, but missing critical pieces (response time)
* Application Insights - Great at easily getting basic metrics from web servers (requests per minute, response time, etc)

---

# Next

Monitoring and Alerting

A bit faster...

---

# Monitoring Generally

* Consumes metrics
* Provides query capabilities
* Notifies of threshold breaches

???

What Monitoring generally does...

---

# Monitoring Engines

What real world Monitoring engines provide

* Ingestion - consume
* Storage - persistence
* Querying - retrieve aggregated data sets
* Visualization - easily consume data with tables and charts
* Setting thresholds - notify of 'breaches'

---

# Ingestion: Push vs Pull

* Push - Client pushes(sends) metrics to monitoring service
* Pull - Monitoring service pulls(requests) metrics from client

*See Prometheus documentation for pros/cons

???

Push
* Each metric generated is pushed (usually over UDP)
* Don't know if server is 'down' or 
* Less scalable
* Client knows about server

Pull
* Pulls every x seconds
* Knows when server is unavailable
* More scalable
* Server knows about all clients

---

# Storage and Query Engine

* Metrics have to be stored somewhere
* Need querying capabilities
* Time series database

---

# Query and Visualization

* AdHoc queries
* Reports
* Historical

---

# Rules / Thresholds / Conditions

Run query. Has a condition been met? If so, send alert?

Requires constant querying of logs.

???

All the same: (Rules / Thresholds / Conditions)

---

# Monitor Symptoms Not Causes

Monitor the response time of a service, not the reason it might be slow. 

Don't monitor SQL query response time, rather monitor request response time.

---

# Monitoring Technologies

* Splunk: Queries + Alerts
    * Good when data is in logs
    * Can alert when Errors increase significantly
    * Can’t alert on latency
* AppInsights
    * Can set some thresholds
    * Create alerts on particular data points
    * Querying and rules are lacking - not enough control

---

# Alerting

Alerting is:

* Notification of Ops, Dev, or On-Call staff, when system has issues
* Alert generated when conditions are met
* Not every threshold breach generates an Alert

---

# Difference between Monitoring and Alerting

Alerting is to UI as Monitoring is to Backend

Separate the 'generation of events' from 'sending of alerts'

Alerting system should control the flow of alerts to reciever

---

# Alerting Essentials

* Limit Noise
* Alerts are Actionable
* Responder recieves only 1-2 alerts per shift

---

# Limit Noise

Think: The Boy Who Cried Wolf

Alerts shouldn't be too noisy... otherwise they’ll be ignored.

???

In a backend service, don't put a 1 second response time threshold, if user wont notice

Tech looks at it ... "Its fine, its not an issue"

---

# Alerts Must Be Actionable

If you cant do anything about it, dont wake someone up.

???

Telematics Alerted at 15 sec threshold. Was valid issue, but nothing could be done.

---

# Limit Alerts Sent During Shift

If there are too many alerts, responder will get:

* burnt out
* not find root cause/fix underling issue

???

Shouldn't just put on bandaid... create real fix.

---

# Alerting Technologies
* OnCall tools (PagerDuty, VictorOps) - Notify ‘on-call’ staff to issues
* Splunk Queries + Alerts - Will send notifications to email, Slack
* AppInsights Alerts - Only sends email alerts

---

# Review

We want to:

* Identify - Proactively identify outages and issues
* Prevent - Limit/eliminate impact of issues
* Improve - Increase the resilience and robustness of systems

???

At the beginning of the talk...

You now have some concpets to start with

---

# Where Do We Start

* Start Simple: Logging / Alerting on Errors

* Use failure to make small incremental change

* Make change immediately after failure

???

Don't do everything at once

---

## Telematics Architecture

Where is Monitoring, Logging and Alerting?

* Event System Logging
* Core System Metrics and Logging

## Logging, Monitoring Alerting in Splunk

* Logs
* Alerts
* Dashboards

## Metrics, Monitoring and Alerts in Application Insights

* Real-time Monitoring
* Analytics

---

# Done