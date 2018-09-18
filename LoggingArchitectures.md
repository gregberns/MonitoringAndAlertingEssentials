# Logging Architectures

**The following is still in progress**

Logging is not simply calling a `log` method. To be a complete solution, most systems will contain the pieces described below. 

It is possible to log directly to a database and call that a system, but I've never seen it be successful.

## System Architecture

There are several important parts to a logging solution

* API and Client
* Injestion
* Persistence
* Queries
  * Reports
  * Ad hoc querying
  * Queries for Alerting
* Alerting

### API and Client

* Transportation - how does it get from client to aggregator? Needs to be general, flexible 
* Payload structure - light weight, standardized structure

Technologies well suited to these requirements:

* Http - reliable, technology agnostic
* Json - technology agnostic, easily serializable

### Injestion

* Needs to be able to injest large quantites of messages. 
* High availability
* Asynchronus processing could be quite helpful when processing messages, if the 'injestor' and 'processor' are separated, downtime impact of the processor can prevent loss of messages

Technologies similar to Kafka would be helpful here.

### Persistence

Log events need to be stored so they can be retrieved at a later date.

Considerations:
* Time - How long do events need to be persisted?

## Querying

Once logs have been persisted, the data needs to be queryable, to provide useful information. 

There are several different categories of queries you'll most likely encounter:

* Queries to generate reports
* AdHoc queries
* Realtime queries for alerting

### Queries for Reports

Reports will be helpful to understand if a system is operating as expected or if there is a potential issue.

### AdHoc Queries

Running AdHoc queries will be important when:

* Trying to understand non-reproducible bugs in production
* Understanding why a system isn't working: Where is this issue coming from? Is another system causing the problem? etc.

### Queries for Alerting

Queries for alerting are valuable so production issues can be identified as quickly as possible. The objective is to know there is an issue occuring before users report it.

* identifying issues with a deployment immediately after a release.
* understand when third party services are failing

Think of these queries as targeting how many errors are occuring in Production at any given time. 

These queries will often be used in conjunction with alerting, which lets you know when thresholds are exceeded.

These queries often return numeric data so conditional calculations can be computed.s

## Alerting

Alerting is a critical piece of a robust software system. Alerting allows the system to notify developers and operations when issues begin to occur so they can be triaged and resolved.

Alerting usually involves queries running at a regular intervals and when a condition is met based on the query, a notification will be sent. Notifications can be sent to places like email, Slack, On-Call systems (VictorOps), etc.

An example of an alert could be:
* Query - How many 500 errors occured in the last 5 minutes
* Interval - run every 5 minutes
* Condition - Is the number of errors over 10 errors
* Notification - Send email to operations team

