# Monitoring Talk 

* Discuss what logging, metrics, monitoring and alerting is.
* Discuss options for each topic
* Discuss Pros and cons within these options
* Discuss technologies that support these things

### Objectives

An attempt to persuade you that you WANT to know what is happening in your system, and how to get that information out of the system, or make the system tell you there is an issue.

### Why would you want to do this? 

Story: 
* 'Customer' walks up and says: The X site isn’t responding. (First question is: why didn’t you know it wasn’t responding?)
* You then spend a couple minutes in utter disbelief making excuses: “It can’t be us!”
* You spend half an hour trying to dig into the logs to figure out what might be going on.
* You make recomendation on who is to blame

Lets prevent this. 

Lets be good engineers and proactively notify our customers, so they don’t don’t have to notify us. 

### Visibility into the system:

* Logs - Show you details of WHAT happened
* Metrics - Show you that something IS happening
* Monitoring - Lets you SEE something is happening
* Alerting - Lets you know something IS happening

### Why:

Its terrible to get interrupted when someone says 'Its not working'. 'Whats not working!' you respond.

I’m no expert in logging, metrics, monitoring and alerting, but limiting the impact of system failures is huge and it makes our systems more robust and resilient.

### Ideas 

* Prometheus documentation, a metrics, monitoring and alerting open source tool set
* Site Reliability Engineering - an open source book wrote by Site Reliability Engineers (SREs) at Google, responsible for keeping Google services up and running 

### Principle

> KISS - Keep it stupid simple

Don't try and do everything at once. Take it one step at a time and make small iterative improvements.

### Theorietical System

Create ‘theoretical system’ that has multiple nodes that need to be monitored. How do we do logging, monitoring and alerting on that system?

```
            API ->
UI1, UI2 -> API -> API -> DB
UI3      -> API ->     -> API
```

One day, UI1 and UI2 report serious latency issues. How do we figure out whats going on?
Add logging in the core and at the seams

## Logging

### Purpose of Logging:

Understand why failures are occurring in a system

### What is Logging 

(This may be basic, but it is critical to understand, so you can see the difference soon)

There are two parts: an ‘entry’ and the log itself
* A log of events or activities that have occurred within the system
* A Log entry should contain a DateTime of an occurrence, a ‘level' where it occurred, and some data about the event

For example, an Apache Web Server Log:

```
// host - user [utcdate] “METHOD /url/resource.file protocol” response filesize
127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326
```

### What does this get us:

* What was accessed and when and by who
* How many times was a url accessed (or not)

> Logging gives us the what, where, when and maybe who

### Log Levels:

* Trace
* Information
* Warning
* Error
* Fatal

* During development we mainly put in Information, Warnings and Errors logs to identify when things happen. 
* During testing these logs can be used heavily to understand what is causing issues. 
* Once the system is operational and stable we can turn the logs up to Warning to start eliminating all the informational warnings. 
* If theres a bug, in prod, we can add some trace statements and run that code in a ‘prod-like’ environment to understand why the errors are occurring.
* If you really need to get performance, then you can only capture Errors. Be wary of getting rid of too many levels, when issues do occur they can be difficult to identify

So where could we put logging in to understand our system better:

### Strategies

* Log to Disk, then potentially have utility move the logs to more permanent location
    * Pros: Reduces network traffic, logs don’t get lost during ‘log service’ outage
    * Cons: Increase disk IO
* Log directly to a service, 
    * Pros: Immediate log feedback (great for debugging)
    * Cons: Increased network traffic

### Problems 

* What happens when log service is unavailable?
* Be cautious of hand rolled HTTP solutions: if remote site is unavailable, the connection timeout will cause a PROD system to grind to a halt. (I’ve seen it, twice, on the same system.)
* DO NOT BUILD YOUR OWN LOGGING SYSTEM, there are many very good frameworks. They've found all the edge cases and failure paths.

### Purpose

* Gather detailed information about WHAT our system is doing

### Not the Purpose:

* How much the system is doing
* High level view of system activity

### Downside to Logging

In a high volume system, if everything your system does is logged the logging can cause performance issues.

For example, pushing all logs to a database could overwhelm a database and so overall performance of the system (or other systems using that logging database could be degraded).

### Technologies:

* GreyLog - streams logs from disk to a log aggregator service
* Log Database - sends log directly to a database
* Splunk - send logs directly to a log service
* Application Insights - has some ‘logging’ capabilities - not recommended for logging though, not its real purpose. (MotionGps tried to use this, but its not really designed for logging)

### Summary:
What does logging accomplish: Something happened, I need to know what it was, where, and when and some details associated. Example: Error occurred because of X

## Metrics:

### Purpose of metrics:

Identify specific areas where issues are occurring or may occur soon
High level overview of 'what the system is doing’ (not why)

### What are metrics?
(Show Telematics Metrics)
(Contrast with logs, aggregated data vs non aggregated data)

### Types of metrics:

Counter - Only can go up  - Like the times a URL endpoint has been hit, Errors
Gauge - Can fluctuate like a speedometer - Requests per second, Average request time
Histogram - Counts observations and puts them in buckets - Request durations (50 @ < 250ms, 10 @ < 500ms, etc)

Example:
api_http_requests_total{method="POST", url="/messages”} count=50
(Contrast this with Logging information, its nice and compact data)

### Objectives

* Pros
    * Light weight - very little metadata included
* Cons
    * Not good for debugging - They give a general direction to look at, not a detailed cause of issues

Note:
You CAN get metrics from logs, but there are better ways. Remember: Separation of Concerns.
Lots of logs prevent scaling up. But metrics just mean a larger ‘int’ value, which is very scalable.


### The Four Golden Signals (SRE Book)

If you can only measure four metrics of your user-facing system, focus on these four:

* Latency - The time it takes to service a request. A slow error is even worse than a fast error! Therefore, it’s important to track error latency, as opposed to just filtering out errors.
* Traffic - A measure of how much demand is being placed on your system.
* Errors - The rate of requests that fail,
* Saturation - How "full" your service is. Saturation helps predict impending issues.

#### Use case #1:

When someone says a service is ‘slow’ you should define what that means:

> Is 10s slow? Is 5s slow? 

And if you ‘fix’ it, how do you know its fixed and doesn't degrade later?

### What to Monitor

Monitor ‘symptoms’ not causes:
* Monitor the response time of a service, not the reason it might be slow. 
    * You may want to track dependancies(AppInsights does) because SQL Databases can be difficult to monitor

### Problems

Do not build your own metrics engines. 
* Complex - aggregation and querying is hard
* Cannot average averages 
* There are already systems that do this

### Technologies:

* Prometheus - Great documentation on what to think about when talking about metrics, ex-Googler open source project (prometheus.io)
* Splunk - Create metrics/reports off logs, but missing critical pieces (response time)
* Application Insights - Great at easily getting basic metrics from web servers (requests per minute, response time, etc)


## Monitoring/Alerting

Monitoring is:

* The process of aggregating and querying metrics in a centralized location
* visualizing - make them human readable, easily consumable

Alerting is:
* Notifies dev or ops of system issues
* (Optional) Notify on-call staff first

Once you have metrics generated, you need to aggregate and visualize them, and alert when they go out of bounds

Make sure Alerts aren’t too noisy… otherwise you’ll tire of them and stop paying attention

Monitoring vs Alerting - Short term, get something working. Longer term, think Separation of Concerns.

Monitoring Technologies
* Splunk Queries + Alerts - Can alert when Errors increase significantly, but can’t alert on latency
* AppInsights Alerts - Can create alerts on particular data points, may not be fine grained enough

Alerting Technologies
* OnCall tools (PagerDuty, VictorOps)- Are used specifically to notify ‘on-call’ staff to issues
* Splunk Queries + Alerts - Will send notifications to email, Slack
* AppInsights Alerts - Only sends email alerts





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


