---
layout: post
title: "Incident Response Part 1: So you need an on-call team"
categories:
- blog
tags:
- team
- incident-response
- petegoo
- on-call
- postmortems
- leadership
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

As your product gains traction and expectations from customers increase, you may find that at some point things start failing. You see one or more of the following signs start to accumulate:

1. You find out about issues from your customers, not your team or your own monitoring systems.
2. These issues are increasingly appearing outside of your working hours and sit unresolved until the next day/week.
3. Your teams are fully committed to new features and are struggling to find time to fix issues.
4. You find it hard to prioritise issues because you don't have a clear understanding of the impact.
5. You have no clear ownership of production issues so they get passed around between teams.

You may further find that these failures are starting to cost you in terms of the trust you have with your customers, the tension between people and roles internally, and possibly even the health of your engineering team. 

You need an on-call team.

# What is an on-call team?
An on-call team is a group of people who are responsible for responding to production issues. They are the first line of defence when things go wrong. They are the people who are woken up in the middle of the night when your systems fail. They are the people who are responsible for restoring service to your customers.

# Do I need to hire another team of people for on-call?
No. 

In a lot of traditional organizations, the on-call team is a separate team of people who are responsible for responding to production issues. This is a terrible idea. It creates a divide between the people who build the systems and the people who run the systems. It creates a culture of "throwing things over the wall" and "not my problem". It creates a culture of "us vs them". It creates a culture of "I don't care about the quality of my work because I'm not the one who has to fix it when it breaks".

Coda Hale outlines this beautifully in his talk Metrics, Metrics Everywhere: 

> "Our code generates business value **when it runs**, not when we write it". 

In other words we should really care about what our code is doing when it runs because that is when it is doing it's job. If you don't then you're creating art, not business value.

There's another aspect at play here and that is that the people who are responsible for creating the issue are the ones who are best placed to fix it. They are the ones who have the context and the knowledge to understand the problem. They are the ones who are best placed to learn from the issue and to prevent it from happening again. If you want an efficient engineering organization then you need to shorten the time from impact to learning and then outcome (more reliable software). You can only do this if the people who feel the pain are the ones who can alleviate that same pain. Separate teams creates weird power dynamics and misaligned incentives.

# So should everyone be on-call?
It depends. If you have a very clear service architecture and a big budget you can have a rotation in each team though this can get prohibitive. I think that, if you have a separate SRE or Platform Engineering team then it makes sense for most of those folks to be on-call as a lot of the incidents that occur will need some insight into the underlying platform/infrastructure. Your service/product/program teams can be a little more fluid depending on how homogenous your services are, and how well the teams communicate changes and risks.

If you have a small org then just do what you can. See below for how to organize rotations and ideal rotation size.

# How do I convince my engineers to go on-call?

There are a number of ways of doing this and it depends the resources (budget) you have available to you, the size of your engineering team, the level of trust you have with your engineers, and the amount of empathy they have for each other and your customers.

1. Start with the most dedicated, driven people
   
   Chances are you probably have some people already on your team who are driven, care deeply about your customers, and are willing to go the extra mile to make sure things are working. These are the people you want to start with. They are the ones who will set the tone for the rest of the team.

2. Pay people for their time

   If you have the budget you should pay people for their time because it's the right thing to do. Here in New Zealand this is fairly easy to do. In the US this can be a little harder but we'll cover this in more details in a future post.

3. Give them time in lieu for time spend responding out of hours.
   
   Regardless of whether you pay people for their on-call time or not, when someone is called out of hours to respond to an issue you should give them back that time by allowing them to reclaim it from their working hours. If you are paying them for being on-call chances are it's not their salaried rate any ways. It also will go a long way to helping them justify the disruption to their personal lives, those of their partners and kids.

   Further, in my experience, this is something you have to reinforce. People will try to be heroes and power through. Gently remind them that they need to take time to recover.

4. If you can, always respond in pairs

   We learned this at a previous company and it served us really well. [Psychological safety at work is incredibly important](https://www.cnbc.com/2019/02/28/what-google-learned-in-its-quest-to-build-the-perfect-team.html). Psychological safety at 3am when things are on fire is even more important. Two pairs of eyes are infinitely more reliable, safe, and effective than one. Having a copilot to make sure you're typing the right command, clicking the right button, shutting down the right server, or whatever, is invaluable.

5. Make sure you have a clear escalation path

   As for #4, make sure people know that they are not alone and they can always escalate. That typically means that you will be contactable yourself. You need to make it ok and make it something that you would rather they did in a time of uncertainty than not.

6. Recognise and praise the on-call team regularly

   When there is a significant incident make sure to publicly praise the on-call team, and thank them for their contribution, no matter the outcome. Other people see this and it helps to build empathy for those that are on rotation. 

7. Have a company phone plan for the on-call team

   They may well have to hotspot whereever they are so make sure they have a company phone plan that covers this. It's a small thing but it's a nice thing to do.
   

One of the surprising outcomes that I notice about on-call teams is that the people on-call have a much better mental model for the way that the software works. They are more active in architectural and design discussions as a result and they tend to be more effective generally. This means they get promoted faster and this gets noticed. 

We often would find that we had a queue of people who had expressed interest in joining the on-call team. This was true even before we paid people an hourly rate for being on-call. When I asked people why they wanted to join the rotation they would tell me the same thing - it was seen as a great learning opportunity and way to grow their career.

To be clear, you don't promote people because they are on-call, you promote them when they become more effective at their jobs.

Another observation I made though was that this good will erodes very quickly if the on-call team is getting woken up constantly, are unable to effect the outcomes they need, and are generally getting beaten up night after night. You still need a good incident response, continuous improvement, learning, and blameless culture in order to make this work.

# What about the people who don't want to go on-call?
We all have lives and different priorities. Listen to their context and apply some empathy. They might have young kids, be caring for a dependent, dealing with a health issue, shared living spaces, who knows.

# Organizing rotations
This is highly dependent on your specific product and team topology but here are a few guiding principles:

1. Healthy rotations in my experience are 1 week at a time, every 4-7 weeks. This gives enough time for recovery while not being too far apart that you forget how things work or lose context on what has changed.
2. Rotations should preferably be in pairs. This is for psychological safety and to make sure that you have a copilot to help you out.
3. Swaps will happen but you need to set some ground rules like no more than two weeks on-call for any individual.
4. Handover rotations during the week, during working hours. Tuesday midday for example is good.
5. Christmas and New Years needs special treatment. We would shorten this to 1 or 2 day rotations and make sure that people were not on-call on Christmas day or New Years day. 
6. Set expectations around taking a laptop everywhere you go and not drinking alcohol or partaking in other mind altering substances while on-call. 

At a previous company we would have an SRE team and a product specific team member paged for each incident.

e.g.
Product 1 alert -> SRE + Product 1 team member alerted
Product 2 alert -> SRE + Product 2 team member alerted

Very rarely did we have overlapping incidents unless there was a cloud provider failure in which case we merged the incidents anyways.

# Future Posts

In future posts in this series we will cover:

- Incident Response Playbooks
- Incident Response Roles
- Incident Severity Levels
- Reporting on Incidents
- (Blameless) Postmortems
- Paying people for on-call


