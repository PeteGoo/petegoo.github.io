---
layout: post
title: "Incident Response Part 4: SitReps (Situation Reports)"
categories:
- blog
tags:
- team
- incident-response
- petegoo
- on-call
- postmortems
- leadership
- sitreps
- situation-reports
- status-updates
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
3. [Incident Response Roles](https://blog.petegoo.com/2024/01/27/incident-response-roles/)
4. Incident Response SitReps (this article)

In the 4th part of this series on Incident Response I'd like to cover something I learned quite late in my 
Incident Response career and that is Situation Reports or SitReps.

![Blue Screen of Death  ](/images/2024/bsod.jpg)

A few years ago I happened to be attending an online session by [Brent Chapman](https://greatcircle.com/), who at the time was leading Incident Response at Slack. He was giving a talk if I remember correctly on his experience performing Incident Commander at Slack, Google, and the Burning Man festival. He mentioned the signifance of Situation Reports and it made me realise that I hadn't been consistent in the format of our Incident status updates and, to be honest, it was hurting us.

# The need for SitReps
Using Slack channels and video calls for incident response is great but a lot of us will have experienced the scenario where you join a Slack channel or video call for an ongoing incident and you want to ask "Can I get a summary of where we are at?". It's awkward because you know you have to interrupt whatever is being discussed to devote attention to getting you up to speed.

You will probably even ask a few common questions like "What is the impact?", "What is the severity?", "What have we tried?", "What have we ruled out?", "What are we trying next?".

This is where SitReps come in. They are a structured way to provide a summary of the incident. They should be delivered at regular intervals, should be concise and to the point. They should be delivered by the Incident Commander or a designated person and should address common questions that will be asked.

# A SitRep template

The below is a suggested template for SitReps. You can adjust it to suit your needs but the most important thing is that you keep the format consistent.

> ⚠️ **Situation Report** ⚠️ <br/>
> **Summary**: Short single line description of the incident<br/>
> **Severity**: [Sev2](https://blog.petegoo.com/2024/01/17/incident-response-severity-levels/)<br/>
> **Status**: Assessing/Stabilised/Mitigated/Resolved<br/>
> **IC**: [@PeteGoo](https://blog.petegoo.com/2024/01/27/incident-response-roles/)<br/>
> **Responders**: @JoeBloggs, @JaneDoe<br/> 
> **Actions**: <br/>
> Have ruled out network issues<br/>
> Joe is investigating recent releases to production to look for potential causes<br/>
> Jane is reaching out to the Customer Success team to gather impact data<br/>
> Pete is notifying stakeholders<br/>
> **Next SitRep**: 3:15pm

In this way we can give anybody that is scrolling through the channel a quick summary of the current status, actions being performed, who to contact, and when the next update will be.

Try to cover off those common questions that will be asked like what you have ruled out and what you are doing now. Do be careful however, I've seen teams use that "Actions" section to keep appending more and more context as the incident goes on. It needs to be concise and to the point, like less than 8 lines. If you need to provide more detail then consider a separate document or a Slack canvas.

A great trick is to also pin/bookmark the SitRep to the channel so that it is easily accessible to anyone who joins.

# Broadcasting SitReps
For a lot of folks who do not want the maximum amount of detail joining the Slack channel can be a step too far. Even worse, they may not know where the channel is or what it is called. In these cases it is a good idea to broadcast the SitRep to a wider audience. 

The best way I have found of doing this is to have an `#incidents` channel where SitReps and notifications of new Incidents are broadcast to anyone that cares. In turn they can enable all notifications for that channel to get high level updates any time a new incident appears or there is a significant update.

# Conclusion
Situation Reports have become a non-negotiable part of my Incident Response process. They provide a structured way to provide updates to the team and stakeholders and ensure that everyone is on the same page. They are a great way to keep the team focused and to ensure that the Incident Commander is not constantly interrupted with questions, especially when you include the next sitrep time to manage stakeholder expectations.

# Future Posts

In future posts in this series we will cover:

- Incident Response Playbooks
- Reporting on Incidents
- (Blameless) Postmortems
- Paying people for on-call


