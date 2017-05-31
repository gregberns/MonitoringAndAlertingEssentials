# Monitoring And Alerting Essentials

** This document currently in progress **

The following is an introductory guide to understand the basics of improving system reliability through active monitoring and alerting, and a process of continuous improvement.

Understanding when your systems are actively having issues is one of the first steps in improving their reliability. Once you can detect a system is having issues, you can actively improve the reliability of that system and prevent further issues from occuring.

## Getting Started

KISS - Keep it stupid simple

There are a million things you *could* monitor. Start with the most simple thing you can think of. Then iterate. Use [Postmortems](SiteReliabilityEngineeringBook.md#postmortem-culture-learning-from-failure) to help drive monitoring/alterting improvements.

## How to Start Monitoring

Steps:

* Choose a system that provides the biggest headache
* Identify a critical bottleneck in the application that can easily be monitored
    * Error volume can be a great place to start. Usually error volume is quite low, so detecting when it rises quickly can be relatively easy
* Add logging and/or a monitoring to this bottleneck, forward these logs/metrics to monitoring service
* In the monitoring tool, build query to retrieve the desired

### Example System:

A system contains three parts:

* A set of clients that send requests to the primary service.
* A service which exposes an API that accepts requests from clients. The requests are stored in a database.
* A database that accepts requests from the service.

#### Critical Monitoring Points:

Read [The Four Golden Signals](SiteReliabilityEngineeringBook.md#the-four-golden-signals) to understand why the following recommendations are made.

##### Phase 1

The two most critical places to monitor are:

* Client-side Errors - If an error is returned or timeout occurs, this should be logged and passed onto the monitoring service.
* Server-side Errors - Any errors that are returned to clients should be tracked. This will catch improperly made requests, depenancy issues (i.e. database errors), and internal code exceptions (read: bugs).

What kind of insight can be gained?
* DB Errors - If the server's error count spikes, it signals a database issue because the database is the most vulnerable system dependency.
* Connection Issues - if server-side volume dips and client side errors spike, it signals an inability of clients to connect with the server
* Clients making invalid requests - Invalid requests can be passed onto the client developers to prevent future issues.



##### Phase 2

Do not rush into phase 2. Spending time fixing issues found from Phase 1, can pay big dividends. You may even find that phase 2 is unnecessary.

* Client - The client may make one or more calls to the supporting service. Logging/metrics should be added to each of the calls made to the service. Two things should be captured, but the first is most important:
    * Latency - Each call should track the time it takes for a request to be made. That latency should be tracked including the endpoint that the request was made to

* Server - Monitoring can be added in this prority
    * Latancy of calls to API - How log does it take for the service to process requests? Errors latency should be tracked with a separate from success latency.
    * Errors emited from the database - database errors can be caught by the API in the first iteration, but it will be helpful to know what DB calls are failing 
    * Latency of database calls - 
    
* Database - Databases usually come with their own monitoring tools. If it is possible to get metrics from your database do so. If it is not possible, you may be able to rely on the Server's errors for the time being.




