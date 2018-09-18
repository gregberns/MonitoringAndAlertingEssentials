# Monitoring And Alerting Essentials

## Purpose

The following is a guide to understand improving system reliability through logging, monitoring, and alerting, and a process of continuous improvement.

If we want to consider ourselves 'engineers' our systems need to work reliably. When they do not work, we need to know **when** they are failing and **why** they are failing.

## Content

There are several parts to this documentation.

* [Process of Making Unreliable Systems Reliable](./ProcessOfMakingUnreliableSystemsReliable.md) - A process to make incremental improments to existing systems

* [Slides for 'Shedding Light on Black Box Services' talk](https://gregberns.github.io/MonitoringAndAlertingEssentials) - Broad overview of why we like reliable systems and the tools we can use to get them

* [Logging Fundamentals](./LoggingFundamentals.md) - Cover all the basics of logging

* [Logging Architecture](./LoggingArchitectures.md) - Things to think about when choosing a logging framework

* [Site Reliability Engineering Book](./SiteReliabilityEngineeringBook.md) - Summary of the more valuable parts of the book

* [When to Conduct a Postmortem](./WhenToConductAPostmortem.md) - A critical part of the continuous process

## Scope

This documentation is primarily limited to application logging. OS, web service, and other types of logging will not be covered. 

## References

The majority of the ideas in this repo have been taken from the places where I learned them:

* [Google Site Reliability Engineering book](https://landing.google.com/sre/book.html)
* [Prometheus documentation](https://prometheus.io/docs/introduction/overview/)
    * There is a great [ChangeLog](https://changelog.com/podcast/168) podcast where the creator of Prometheus discusses why the tool was built
