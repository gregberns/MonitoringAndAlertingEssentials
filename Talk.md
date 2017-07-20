# Shedding Light on Black Box Services

---

# Overview

Will discuss   Logging, Metrics, Monitoring, and Alerting

* Why you should care
* Understand these concepts better
* Methodologies for each concept
* Pros and cons within each methodology
* Technologies that support these concepts

???

Methodologies - how to THINK / USE concepts

---

# Talk Objectives

I WANT you to WANT these things:

* Identify - Proactively identify outages and issues
* Prevent - Limit/eliminate impact of issues
* Improve - Increase the resilience and robustness of systems

In other words, your system should tell you:

* when your system has a problem
* where an issue exists
* why its occuring

---

# These Ideas 

Are not mine.... 

These are the best two sources I've found:

* **Prometheus documentation** - a metrics, monitoring, and alerting open source tool set
    * great docs to understand metrics basics
* **Site Reliability Engineering** - an open source book by Googlers
    * Site Reliability Engineers (SREs) at Google are responsible for:
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

Finally, make a recomendation on the 'cause'

![Its not my fault](http://rebootauthentic.com/wp-content/uploads/2013/10/responsibility.png)

---

Lets prevent this. 

Lets use good engineering practices.

---

# What can we improve

* Lets proactively notify our customers of issues
* Be notified of issues before customers report them
* Identify problem areas quickly
* Dig into causes quickly
* Strategically make improvements after failure

---

# Methodologies 

Start with well known concepts or methodologies. Then identify technologies.

### Problem

The available technologies blur the lines. So we need to understand boundaries of concepts.

---

# Concepts

Start with the problem: Theres an issue, but we didnt know about it.

Lets work our way backwards:

---

* Need to recieve a notification
    * Alerting - Know *that* something has occured (Email, Phone call, etc) 
     (Image of Woof from the Office)

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

---

## Concepts

When implementing, start at the top, work down...

* Logging - add basic logging
* Metrics - simple tracking metrics (Four Golden Signals, REDs)
* Monitoring - create calculations, thresholds
* Alerting - get notifications

---

# Lets talk about ....

---

# Logging

---

# Purpose of Logging

(This may be basic, but it is critical to understand)

WHEN

WHERE

WHY

Understand why failures are occurring in a system

---

# What is Logging 

There are two parts: a 'log' and an 'entry' in the log

* The log:
    * a set of events or activities that have occurred over time

* The log entry:
    * Is when something occured
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

* WHEN an event occured
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

* Error - Significant error has occured. Keep an eye on these.

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

# Attributes of Metrics vs Logs

Metrics are:

* Small - in data size, compared to logs
* Viewed in Aggregate - single log can be valuable, single metric is worthless
* Structure - simpler, more concise, more structured than a log

---

# Purpose

Sets of metrics allow you to:

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

# Use case #1:

When someone says a service is ‘slow’ you should define what that means:

> Is 10s slow? Is 5s slow? 

And if you ‘fix’ it, how do you know its fixed and doesn't degrade later?


---

# Metrics Technologies

* Prometheus - Great documentation on what to think about when talking about metrics, ex-Googler open source project (prometheus.io)
* Splunk - Create metrics/reports off logs, but missing critical pieces (response time)
* Application Insights - Great at easily getting basic metrics from web servers (requests per minute, response time, etc)

---

## Monitoring/Alerting

Monitoring is:

* The process of aggregating and querying metrics in a centralized location
* visualizing - make them human readable, easily consumable


Once you have metrics generated, you need to aggregate and visualize them, and alert when they go out of bounds


Monitoring vs Alerting - Short term, get something working. Longer term, think Separation of Concerns.


-charts



Monitoring Technologies
* Splunk Queries + Alerts - Can alert when Errors increase significantly, but can’t alert on latency
* AppInsights Alerts - Can create alerts on particular data points, may not be fine grained enough


# Alerting

Alerting is:
* Notifies dev or ops of system issues
* (Optional) Notify on-call staff first

Make sure Alerts aren’t too noisy… otherwise you’ll tire of them and stop paying attention




Alerting Technologies
* OnCall tools (PagerDuty, VictorOps)- Are used specifically to notify ‘on-call’ staff to issues
* Splunk Queries + Alerts - Will send notifications to email, Slack
* AppInsights Alerts - Only sends email alerts

### What to Monitor

Monitor ‘symptoms’ not causes:
* Monitor the response time of a service, not the reason it might be slow. 
    * You may want to track dependancies(AppInsights does) because SQL Databases can be difficult to monitor











## Show Telematics Archetecture

Where is monitoring
Where is logging
Where is Alerting


# Logging, Monitoring Alerting in Splunk


Splunk Logs
Splunk Monitoring
Splunk Alerts

# Metrics, Monitoring and Alerts in Application Insights

Application Insights
* Realtime Monitoring
* Analytics
* Monitoring and Alerting









## What do we start?

* Logging

Then what do we do next? What does each thing allow us to do?



Track systems you don’t own/don’t have control over. 

You should know when the systems aren’t working as expected, and can understand why they aren’t working. 
Important: You don’t need to automate ‘fixing’ issues. This can actually backfire. Read book to understand why. (Even Google doesn’t do automatic failovers)

                              Events      -> 
IMS, POS -> Vehicle Service -> Telematics -> DB
                             Motion       ->                   -> Lookup Service

Advantages to having Monitoring:
Nice to haves: Push to prod and track during deployments


