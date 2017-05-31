# When to Conduct a Postmortem

The objectives of this document are to develop guidelines around why and when to go through the postmortem process.

## Goals of Conducting a Postmortem

The primary goals of conducting a postmortem are to:

* Ensure that the incident is documented
* That all contributing root cause(s) are well understood
* And especially, that effective preventive actions are put in place to reduce the likelihood and/or impact of recurrence

## Triggers for Postmortem

The postmortem process does present an inherent cost in terms of time and effort, so we are deliberate in choosing when to conduct one. Teams have some internal flexibility, but common postmortem triggers include:

* User-visible downtime or degradation beyond a certain threshold
* A resolution time above some threshold
* A monitoring failure (which usually implies manual incident discovery)
* On-call engineer intervention (release rollback, rerouting of traffic, etc.)
* Data loss of any kind

## Details

User-visible Downtime or Service Degradation

* What is our overall goal for up-time?
* What's the threshold for latency?
* What are the requirements around the IMS service dependency?

Resolution time

* More than 15(??) minutes to resolve an issue

Monitoring Failure

* If Help Desk has to tell us about an issue

On-call engineer intervention

* If Ops needs to scale a service
* If a deployment needs to be rolled back by Steve's group

Data loss of any kind

* Should events be included in this?
