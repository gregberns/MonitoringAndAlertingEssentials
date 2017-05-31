# Monitoring And Alerting Essentials

The following is an introductory guide to understand the basics of improving system reliability through active monitoring and alerting, and a process of continuous improvement.

Understanding when your systems are actively having issues is one of the first steps in improving their reliability. 

## Site Reliability Engineering Book

A majority of the topics below are based (read 'stolen') on the book [Site Reliability Engineering](https://landing.google.com/sre/book.html), written by Google's Site Reliability Engineers (SRE). SRE's are responsible for ensuring Google's services are reliable and when there are issues, they ensure there are measures put into place to prevent future issues.

The great thing about this book is that the book is broken down into bite sized chapters and each can be read independently of the others. After reading the [Introduction](https://landing.google.com/sre/book/chapters/introduction.html), feel free to skip around and read chapters of interest to you.

*Suggested Chapters*

* [Introduction](https://landing.google.com/sre/book/chapters/introduction.html)
* [Eliminating Toil](https://landing.google.com/sre/book/chapters/eliminating-toil.html) - Start small and eliminate pain one  place at a time. A little action goes a long way.
* [Monitoring Distributed Systems](https://landing.google.com/sre/book/chapters/monitoring-distributed-systems.html) - Monitoring and alerting enables a system to tell us when it’s broken. 
* [Postmortem Culture: Learning from Failure](https://landing.google.com/sre/book/chapters/postmortem-culture.html) - A blameless postmortem process helps identify areas for improvement


## Monitoring Distributed Systems

Systems with a distributed archetecture are inherently complex and without proper monitoring it is impossible to understand what they are doing and identify problem spots.

### The Four Golden Signals

The four golden signals of monitoring are latency, traffic, errors, and saturation. If you can only measure four metrics of your user-facing system, focus on these four.

* Latency - The time it takes to service a request. A slow error is even worse than a fast error! Therefore, it’s important to track error latency, as opposed to just filtering out errors.
* Traffic - A measure of how much demand is being placed on your system.
* Errors - The rate of requests that fail,
* Saturation - How "full" your service is. Saturation helps predict impending issues.

Example Service:
* Latency - If latency is usually under 1 second, greater than 10 second latency can signal issues like underlying database connection problems or other service availability issues. 
* Traffic - Traffic can be a great measure both when it spikes and drops. If traffic spikes, it may put unusual pressure on other systems. If traffic dips below an average, it may signal an issue in upstream systems.
* Errors - Maybe the most critical and first place to focus. Errors signal issues in downstream systems. Alerting on error spikes will help detect issues before users report them.
* Saturation - Ensure a service has enough 'overhead' so if load does increase, theres enough overhead to handle the increased traffic.

### Guidelines

Design your monitoring system with an eye toward simplicity. When chosing what to monitor keep the following guidelines in mind:

* The rules that catch most real incidents are often the most simple, predictable, and reliable rules.
* 'Turn down' alerts that fire too often, too many alters is much worse than not enough. 
* Consider removing alerts which aren't triggered or collected data that goes unused (e.g., less than once a quarter) 
* Remove signals that are collected, but not exposed in any prebaked dashboard or used by any alert

### Getting Started Suggestions

KISS - Keep it stupid simple

There are a million things you *could* monitor. Start with the most simple thing you can think of. Then iterate. Use Post Mortems to help drive monitoring/alterting improvements.


## Postmortem Culture: Learning from Failure

> "Always learn from your mistakes"
>  -- Your Mother

Having a blameless post mortem after system incidents

This process helps weather fewer outages and fosters a better user experience.

Example [When to Conduct A Postmortem](WhenToConductAPostmortem.md) document.

Example [Post Mortem document](https://landing.google.com/sre/book/chapters/postmortem.html)




