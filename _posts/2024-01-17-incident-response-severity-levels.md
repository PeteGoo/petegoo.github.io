---
layout: post
title: "Incident Response Part 2: Severity Levels"
categories:
- blog
tags:
- team
- incident-response
- petegoo
- on-call
- postmortems
- leadership
- severity-levels
- impact
- priority
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
2. Severity Levels (this article)
3. [Incident Response Roles](https://blog.petegoo.com/2024/01/27/incident-response-roles/)
4. [Situation Reports](https://blog.petegoo.com/2024/03/13/incident-response-sitreps/)

In [Part 1](https://blog.petegoo.com/2023/12/06/so-you-need-an-on-call-team/) of this series we covered why you might need an on-call team. In this post we will cover how to define severity levels for incidents. This is crucial in order to understand the impact of the incident on your customers and your business. This in turn will help your on-call team, your leadership team, and the rest of the business understand how to respond and how to communicate the incident.


![delboy mobile](/images/2024/dangerous-for-swimming.jpg)

# What are Severity Levels?
Severity Levels are a way of describing how important an incident is to your business. This may mean that the impact to customers is high, or the impact to revenue is high, reputational damage is likely, or any other factor that is important in your context.

Typically these are numbers from 1 to 5 or more, with 1 being the most severe and 5 being the least severe. I think it is best to avoid words like "Critical", "High", "Medium", "Low", and "Informational" as these are all relative and can be interpreted differently depending on your background, experience and language/dialect.

Also, don't make them zero-based. People will end up using the term P0 or Sev0 in extreme situations or in jest but there's no need to formalise it, it will just confuse the non-tech people.

# Why is it important to define Severity Levels?
Incident response can be an extremely fast moving and stressful situation. Often when we are in the throes of an incident it can be difficult to contextualise the level of response required in relation to the impact of the incident itself and even the potential impact of our actions. If you do it often enough you can easily fall into the trap of burning out your team or, in the other extreme, being overly complacent through familiarity.

Severity Levels help us to understand a very key aspect of incident response - who we should communicate the incident with and how. I talked about this at length in my post [Lines of Communication and Lines of Enquiry in Incident Response](https://blog.petegoo.com/2023/02/22/incident-response-lines-of-communication-enquiry/). I won't repeat too much of that here but I encourage you to read that post if you haven't already. In short, one of the most detrimental failures you can make in incident response is to fail to let the right people know that an incident is happening. 

Severity levels can also impact how we respond to an incident. For example - it may be ok to leave an incident for a few hours or overnight if no customers are affected and we have a mitigation in place, or if the feature is seldom used. On the other hand we may decide for some incidents that we need to disable key features, communicate with customers, even sacrifice availability in very rare cases.

# How do I define Severity Levels?

Severity levels are very likely unique to your context. The best way to define them is to sit down with representation from across the business - engineering, product, customer support, sales, legal etc and agree on what makes sense. You could use collaborative tools like Miro/Mural or a plain old whiteboard, then add cards for typical outages you have had in the past or can foresee in the future. Assign them to severity levels and then discuss and iterate until you have a set of levels that make sense to everyone.

What you are looking to end up with is a table much like the following:

<table>
  <thead>
    <tr style="border-bottom:1pt solid black;">
      <th>Severity</th>
      <th>Sev 6</th>
      <th>Sev 5</th>
      <th>Sev 4</th>
      <th>Sev 3</th>
      <th>Sev 2</th>
      <th>Sev 1</th>
    </tr>
  </thead>
  <tbody>
    <tr style="border-bottom:1pt solid darkgrey;">
      <td style="border-right:1pt solid darkgrey;"><strong>Description</strong></td>
      <td style="border-right:1pt solid darkgrey;">Internal Impact Only <br /><br /> No customers impacted</td>
      <td style="border-right:1pt solid darkgrey;">Problems reported with non-core functions</td>
      <td style="border-right:1pt solid darkgrey;">Customer confusion to small subset of customers <br /><br /> Background jobs failing <br /><br /> Could become P2/P3</td>
      <td style="border-right:1pt solid darkgrey;">Issue affecting a small group of customers <br /><br /> Redundancy loss with no impact <br /><br />Security near-miss</td>
      <td style="border-right:1pt solid darkgrey;">Affects large number of customers or a Top 10 customer <br /><br />Functionality severely impaired</td>
      <td>A serious event affecting most customers. <br /><br /> Generally unavailable <br /><br /> Impairs ability to perform key tasks <br /><br /> Security event e.g breach/disclosure</td>
    </tr>
    <tr style="border-bottom:1pt solid darkgrey;">
      <td style="border-right:1pt solid darkgrey;"><strong>Typical Examples</strong></td>
      <td style="border-right:1pt solid darkgrey;">-</td>
      <td style="border-right:1pt solid darkgrey;">-</td>
      <td style="border-right:1pt solid darkgrey;">-</td>
      <td style="border-right:1pt solid darkgrey;">-</td>
      <td style="border-right:1pt solid darkgrey;">-</td>
      <td>-</td>
    </tr>
    <tr style="border-bottom:1pt solid darkgrey;">
      <td style="border-right:1pt solid darkgrey;"><strong>Response</strong></td>
      <td style="border-right:1pt solid darkgrey;">-</td>
      <td style="border-right:1pt solid darkgrey;">Inform Customer Success</td>
      <td style="border-right:1pt solid darkgrey;">Inform Customer Success <br /><br /> Inform Engineering Leadership (VPE)</td>
      <td style="border-right:1pt solid darkgrey;">Inform Engineering Leadership (VPE+CTO) <br /><br /> Implement in-product notifications of issue</td>
      <td style="border-right:1pt solid darkgrey;">Executive Leadership Team<br /><br />Raise Status Page</td>
      <td>Notify Executive Leadership Team <br /><br />Notify Board<br /><br /></td>
    </tr>
  </tbody>
</table>


# How do I evaluate the Severity Level of an incident?
Keep a link to the above table in your incident response documentation. When an incident occurs, evaluate the impact of the incident against the table. If you are unsure, err on the side of caution and escalate to the next severity level.

Add examples that are relevant to you and your business. These should be regularly revised.

# How do I balance the need for impact analysis with the need to respond quickly?
It can be very difficult to evaluate the severity level of an incident in the heat of the moment. Often you are trying to prioritise stablilising the system over other seemingly non-critical tasks. This is one of the reasons why it is very useful to [have more than one responder to an incident](https://blog.petegoo.com/2023/12/06/so-you-need-an-on-call-team/). With multiple responders you can spin someone out to assess the impact of the incident and communicate with the rest of the business.

Failure to assess the severity level can result in substandard communication protocols, disgruntled customers, and even a lack of trust in the on-call team. Aspire to have very clear guidelines on what constitutes a severity level. Keep the document up to date with typical prior examples so that these can more easily be assessed in the moment.

# Future Posts

In future posts in this series we will cover:

- Situation Reports
- Incident Response Playbooks
- Reporting on Incidents
- (Blameless) Postmortems
- Paying people for on-call


