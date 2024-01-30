---
layout: post
title: "Incident Response Part 2: Roles"
categories:
- blog
tags:
- team
- incident-response
- petegoo
- on-call
- postmortems
- leadership
- roles
- incident-commander
- scribe
- operator
- escalation
status: publish
type: post
published: true
meta: {}
author:
  login: petegoo
  email: pete@petegoo.com
  display_name: PeteGoo
  first_name: Peter
  last_name: Goodman

---

Other parts of this series on Incident Response:

1. [So you need an on-call team](https://blog.petegoo.com/2023/12/06/so-you-need-an-on-call-team/)
2. [Severity Levels](https://blog.petegoo.com/2024/01/17/incident-response-severity-levels/)
3. Incident Response Roles (this article)

In this, the 3rd part of the series on Incident Response, we're going to cover arguably one of the most important aspects of incident response teams and that is distinguishing the roles that responders should assume.

![duke of york, belfast](/images/2024/duke-of-york.jpg)



# Incident Commander
If you talk to almost any incident response team you will find that they commonly have identified the need for one person to orchestrate the efforts of the team handling the incident. In high-pressure environments there isn't time for consensus driven decision making, random interruptions, or waiting for people to volunteer for tasks. 

The Incident Commander is the person who is responsible for:
- Establishing lines of communication
- Delegating tasks to other responders
- Making decisions on severity, response, approach, lines of enquiry, and the response team
- Situation Reports (SitReps will be covered in a separate post)
- Concluding an incident
- Evaluate the need for a blameless post-mortem and ensure it is done

You will see that I mention "Lines of Communication" and "Lines of Enquiry". To me this is a great model for guiding the actions of an incident response team. Often we forget to validate our assumptions, explore other options, and communicate with the right people. For more details on this model read my earlier post - [Lines of Communication and Lines of Enquiry in Incident Response](https://blog.petegoo.com/2023/02/22/incident-response-lines-of-communication-enquiry/). I won't cover those details here.


## Delegating tasks to other responders
One of the key responsibilities of the Incident Commander (IC) is to make sure that tasks are delegated and assigned to responders. It is important that the IC has enough space to perform the other responsibilities outlined here, therefore they should not overcommit themselves to many Incident Response activities.

ICs need to be assertive in delegating tasks. They should try to avoid asking for volunteers. Nobody has time for that awkward silence while we wait for someone to put their hand up, instead assign the task to the most appropriate person. Itʼs up the IC to know each responders capabilities, what they are working on, and how to prioritise the incident response tasks.

## Making decisions on severity, response, approach, lines of enquiry, and the response team

By this stage you should have identified severity levels that are important to your business if not read [part 2 in this series](https://blog.petegoo.com/2024/01/17/incident-response-severity-levels/). An Incident Commander should familiarise themselves with the Incident Severity Levels .

You can change the severity levels during an incident. Sometimes the Severity can dictate the appropriate level of incident response so make sure you evaluate the severity regularly throughout the duration of the incident.

## The Response Team
The Incident Commander chooses the members of the team.

- Do we need to bring new members into the team for more specialised expertise or to add
more hands?

- What specific roles are each of the team performing?

- Are members of the team fatigued or have other commitments and need to be replaced?

## Identifying the Incident Commander

Not everyone will want to take on the responsibility of being an Incident Commander. It can be a stressful, tough (but rewarding), role and some folks may not feel inclined or ready to take it on. Preferably there should be a list of pre-trained and/or experienced staff who have knowledge of the procedures and expectations of being an incident commander.

# Operator

The operator is the most obvious role in an incident response team. They are the person who is doing the work to resolve the incident. They are the person who is typing the commands, running the scripts, and making the changes. 

At the beginning of an incident this may actually be the only role until we know that we have a significant enough incident to warrant an Incident Commander and Scribe.

This is often where people are most comfortable. Though be careful, without any designated Incident Commander you can only really have two Operators before things get really hairy.

# Scribe
I always tell this story of how we identified the need for this role. It came from the behaviours of one of our QAs at a previous role. When we had an incident they would calmly slide over beside the folks involved and start taking notes in Sublime Text. At the time they used a plugin that noted the time beside each line. Later they would contribute during incident reviews by referring to the record of events.

Now we can just use Slack for this. On any incident, if there any actions to be taken make sure that someone performs the role of "scribe". This is especially important if the response team is distributed, on a video call. Call out notes for the scribe to add to the record. For example:

- Observations that have been made
- Actions we are taking
- Expectations that we have of those actions
- Assumptions we have made so far
- Impact analysis

Treat it like the court reporter in a trial. They are there to record the facts and observations, not to interpret them. The scribe should not be making any decisions or recommendations. 

The output that the scribe produces will serve as the vital component of a Blameless Post Mortem - a true record of the timeline that will help us to understand what happened and why.

# Optional Roles

The following roles are not necessarily something you need but they can be useful in some situations.

## Impact Analysis

Impact Analysis can be a very detailed task. In special cases I found it useful to spin out one or more people to gather this data for Customer Success to contact or to guide the response plan.

They likely will have to dig deep on observability tooling, logs, a data lake, production databases etc.

## Executive Communications

This is a little more common. It is an incredibly good idea to set the expectation with senior stakeholders that they should stay well away from the incident response team. Nobody needs the CEO/CTO rocking up into an incident response call and asking scary questions. 

Typically the Incident Command will handled these requests but if the incident is large enough, is moving fast enough, and the number of stakeholders is large enough it can be useful to have someone dedicated to this role.


## Customer Communications

This is a very important role. If you have a Customer Success team then they should be the ones to handle this. If not then you should have someone who is responsible for communicating with customers. 

# Future Posts

In future posts in this series we will cover:

- Situation Reports
- Incident Response Playbooks
- Reporting on Incidents
- (Blameless) Postmortems
- Paying people for on-call


