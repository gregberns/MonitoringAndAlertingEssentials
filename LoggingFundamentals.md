# Logging Fundamentals

## Purpose of Logging

(This may be basic, but it is critical to understand)

* WHEN
* WHERE
* WHY
* Context

Understand why failures are occurring in a system

## What is Logging 

There are two parts: a 'log' and an 'entry' in the log

* The log:
  * a set of events or activities that have occurred over time

* The log entry:
  * Is when something occurred
  * Data about the event
  * Often includes a ‘severity level'


## Example of Log

An Apache Web Server Log:

```
// host - user [utcdate] “METHOD /url/resource.file protocol” response filesize

127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326
```

It answers: What was accessed, when, who, and provides context

## Purpose of Logging

(This helps make a distinction between logging and metrics)

Gather detailed information about:

* WHEN an event occurred
* WHAT happened when the event occurred (context)
* WHERE it happened


## Not the Purpose

Logging is not designed to provide these things:

* What the system as a whole is doing
* How much the system is doing
* High level view of system activity

These are the objectives of a metrics system.

**Tactic:** It **is** possible to use logging to accomplish these goals. If attempting to stabalize a sytem without logging or metrics, starting with logging can often help answer the questions metrics are designed to answer.

**Best Practice:** Keep logging separated from metrics. If mixed, and you want to raise the log level in production (only log warnings and errors) you will loose your metrics, which is unacceptable.

## Log Levels

* Fatal - When process has crashed (Full client, NodeJs)

* Error - Significant error has occurred. Keep an eye on these.

* Warning - Shouldn't occur, but not sure how to handle (Try/Catch)

* Information - Comprises most logs, can help understand failures

* Trace - 'Debugging' in Prod

## Log Entry Structure

Example of the contents of a log entry:

```
entry = {
  severity: Fatal | Error | Warn | Log | Trace,
  timestamp: UtcDate,
  // system the log originates from
  source: String,
  correlation_id: UUID,
  message: String,
  payload: Any,
}
```

## Usage

* During development we mainly use: Information, Warnings and Errors
* Once operational, maybe only log Warnings
* Debugging Prod, use Trace - understand why the errors are occurring
* In high volume system, only capture Errors. 

## Strategies

There are different strategies that can be used to handle entries on the client side:

* Log to Disk, with utility to move logs to more permanent location
  * Pros: Reduces network traffic, logs don’t get lost during ‘log service’ outage
  * Cons: Increase disk IO and need another job to persist
* Log directly to a service 
  * Pros: Immediate log feedback (great for debugging)
  * Cons: Increased network traffic

## Production Issues to Consider

* Availability - What happens when log service is unavailable?
* Performance Impact - heavy logging impacts disk and/or network
* Be cautious of hand rolled HTTP solutions: if remote site is unavailable, the connection timeout can cause a PROD system to grind to a halt. (I’ve seen it, twice, on the same system.)
* Industry convention says: "Do not build your own logging library", there are many very good frameworks. They've found all the edge cases and failure paths (See StackOverflow)
* Logging system goes down - For example, pushing all logs to a database could overwhelm a database and so overall performance of the system (or other systems using that logging database could be degraded).

## Technologies:

* Log4Net - derived from very popular Java library
* System.Diagnostics.Trace - .Net framework
* GreyLog - logs to disk, separate process sends to a log aggregator service
* Log Database - sends log directly to a database
* Splunk - send logs directly to a log service
* Application Insights - has some ‘logging’ capabilities - not recommended for logging, its the wrong tool.

### Log source metadata

**More research needs to be done in this area to understand industry best practices.**

It may be helpful to include more information about the source of the logs, such as:

* Server/Node name - this allows issues with specific nodes to be identified
* Code version

## Types of Logging

There are various types of logging.

* Application Logging 
* Web Server (IIS)
* OS

Different systems may have different needs and slightly different solutions.

