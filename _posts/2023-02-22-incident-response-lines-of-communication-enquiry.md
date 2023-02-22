---
layout: post
title: "Lines of Communication and Lines of Enquiry in Incident Response"
categories:
- blog
tags:
- incident response
- petegoo
- incidents
- communication
- incident commander
status: draft
type: post
published: false
meta: {}
author:
  login: petegoo
  email: pete@petegoo.com
  display_name: PeteGoo
  first_name: Peter
  last_name: Goodman

---

Over the last 9 years I've been involved in a lot of incident response activity and subsequent post-mortems or post-incident reviews (a good thing). I'm always incredibly interested in the behaviours of incident responders and just how difficult it is to remember all the right things to do when you are the one sitting in the hot seat.

At times I have seen first hand that frozen state where you forget how everything works and have no idea where to look for clues. At other times I've seen that all-too-common scenario where the incident response team is busy looking around, diagnosing, investigating but they have forgotten to tell anyone that there is an incident going on so that customers and stakeholders can be informed. 

A few years ago I came to the realisation that there are two distinct lists that you should keep for your incident response team.
- Lines of Enquiry
- Lines of Communication

For me I put these in a wiki page that is easily at hand. Our incident response bot links to these pages so folks can easily find them when they think "Lines of what again?".

### Lines of Enquiry

These are going to be different for almost every organization but there are some common themes that you should try to hit. 

Firstly, all things happen for a reason and that reason is almost always a change of some sort. Use your experience and trawl through your incident reports. Try to think of the most common changes that cause issues. These are typically deployments, infrastructure changes, patching, flag flips, etc.

For us, this looks like:


> 1. What did we change?
> 
> Very often a system starts to misbehave after a change of some sort
> - Was there a recent deploy?
>   - Did the issue begin happening just after or during a new deployment?
>   - Is there a chance that the deployed change is related?
>   - Is the deployment safe to revert?
>   - Deploy the previous release asap and continue to investigate while you wait for the result
> 
> - Are we currently patching machines?
>   - Was there a change in machine images that could have caused the issue?
>   - Was there a recent update of the operating system or a component?
> 
> - Has a related workload been deployed recently?
> - Have we just deployed some terraform changes?


Next you want to think about any changes in traffic patterns 


> 2. Has our traffic changed?
> 
> - Is there more traffic load?
> - Are we seeing unusual load on certain endpoints?
> - Is it a \<insert seasonal outliers e.g. end of month, end of year\>?
> - Are we processing a large number of queued messages / workload A / workload B / workload C?


It's a good idea to give them links to dashboards etc here.

Next up, it's always good to check if it's someone else's stuff that's broken


> 3. Has one of our partners had a fault?
> - Is \<insert cloud vendor\> reporting issues?
>   - \<link to their status page\>
>   - Single AZ failure? Regional?
> - Twilio / SendGrid / Vendor A / Partner B ?
> - Global CDNs? CloudFlare? Fastly? Akamai?
> - DNS providers?
> - Mobile Carriers? AT&T? Verizon?
> - Large networking providers? Commcast? BGP again?


Then there's a list of things that can change without prompting by us


> 4. What could have been changed on us?
> - Could the SQL query optimizer have created a bad plan in the database that is impacting our query performance? Purge the worst plans.
> - Could a scheduled maintenance job have kicked in?
> - Could a container have recycled?
> - Autoscaling kicked in/out?


Now you want to think about less recent changes that may only be impacting now because of some confluence of events.


> 5. Could this be the first time we have done this since a change?
> - It may be the first time a certain type of scheduled activity has run since a change.
> - It could be that we have just scaled out our cluster since a change to the machine images, configuration, code.


Lastly it's a good idea to think about adjacencies. You never know when you might that extra context is going to challenge your assumptions.


> 6. What else could be affected
> 
> Look for clues from services with the same dependencies etc to see if they also have issues but are not producing alerts in the same way.

# Lines of Communication

My personal failure mode in the past has been getting lost in the details of an incident and forgetting to inform other people. They have been some of the hardest post-incident conversations. It's understandable but it's also incredibly frustrating when you find out way too late that something is going on, or customers are calling up complaining and you have nothing to tell them, or someone says "hey tell me about this ongoing incident for \<that thing you're responsible for\>" and you have no idea what they are talking about.
 
So here are some starting points for lines of communication.

> 1. Have you got an Incident Commander?
> - You’re in Incident Response. You need one. Escalate to one if you don’t
> 
> 2. Have you told Customer Support / Success that customers could be affected?
> 
> 3. Do you need to let your Manager / Director / VP know?
> - \<Insert your own escalation policy here e.g. If it’s Severity 1-3 then you should...\>
> 
> 4. Should we post a StatusPage?
> - \<insert your orgs philosophy on when this is appropriate and who makes the decision\>
> 
> 5. Have we opened a ticket with \<cloud provider\> / \<Vendor A\> / \<Partner B\>?
> - Let them know they have a problem
> 

