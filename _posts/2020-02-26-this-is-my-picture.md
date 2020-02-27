---
layout: post
title: "This is my picture: Why you should be drawing your systems and code"
categories:
- blog
tags:
- aws
- petegoo
- drawing
- picture
- architecture
- communication
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

How inclusive is the culture of architecture and design within your teams? Is the way you communicate these designs and intent accessible by everyone? Can everyone in your organisation easily contribute ideas and share their experience?

For the last few years I've been trying to actively spend more time drawing code, architectures and concepts rather than just talking about them. I feel like this was once a lot more common in the places I have worked but seems to have become something of a rarity.

![marker-winning](/images/2020/this-is-my-picture.gif)


## The problem with words

When I first moved to New Zealand I found myself working for a company building legal software. On day one an architect took me into a room and started explaining the architecture of the product in the terminology of [Domain Driven Design (DDD)](https://martinfowler.com/tags/domain%20driven%20design.html), a concept I had never come across before that is packed full of specific terminology and concepts. So much so in fact that [the bible of DDD by Eric Evans](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=sr_1_1?keywords=domain+driven+design&qid=1582796070&sr=8-1) is 560 pages of pure gold but takes quite a few reads before it really sinks in. _Ironically one of the main goals of DDD is to reduce confusion between individuals and teams by 
devising a shared ubiquitous language_ 

So here I was getting a massive download of architecture and historical context. Like a typical [imposter dev](https://en.wikipedia.org/wiki/Impostor_syndrome), I wasn't going to admit that a lot of these new words and acronyms were completely foreign to me. 

So how then was I able to understand any of what was being said at the time?

The simple answer is that he drew on a whiteboard with a marker as he talked. The result was that this impervious wall of nomenclature washed over me while the picture filled in the blanks that were left behind.

## So why are pictures important

**Drawing pictures in front of people is a *room leveller*.**

We have so much assumed context when we present to or talk with our peers that we often forget that we may have completely different backgrounds and experiences. For example:

 - We may not all have the same level of confidence in groups and so we may not ask questions
 - We may not all have the same first spoken language
 - We may not all come from the same social circle / tech / culture or country
 - We may not all have gone to college / university or read the same comprehensive book on exotic and seldom used design patterns

Simple diagrams can transcend these differences. The box that represents a thing. The line that represents a relationship of some sort, traffic, data or control flow. A big cloud of amorphous internet. A stick figure user. These shapes describe abstract concepts more universally than any specific words could.

## Why is drawing pictures important?

There is more intent and meaning communicated in the act of drawing than just the end product.

I've often had the experience where I've tried to recreate a successful whiteboarding session to someone else using only the finished picture, expecting them to instantly gain the same understanding I did when it was drawn. Except now, they just look at me blankly. Why? I may even look back at the drawing and think, this is nonsense. These are the insane scribblings of an unhinged individual. 

Watching a drawing unfold in front of you as the intent is explained builds understanding as the picture evolves. While the conversation continues the breadcrumbs of how that understanding was formed are still there in full view, reinforcing the conceptual model we have built as it is committed to memory. At the same time it allows the presenter to add a layer of language and terminology on top of that new understanding. 

That newly accumulated understanding, language and terminology will always be rooted in the memory of the drawing. A drawing that is relevant and helpful. Without these visual memories I tend to associate what I learned with the shoes of the presenter, the smell of the room or the colour of the walls. 

These visual anchors that are still on the board are even more useful when participation is encouraged. The terminology and scope can be expanded as those present contribute to the refinement process. This interactive part is where the diagram really comes into its own. Use it as others expand on your ideas and provide alternatives. When questions are asked, point back to the relevant elements as you answer. You may even find yourself drawing more elements or just pointing as others talk and refine your explanation.

![this-is-my-picture](/images/2020/MVIMG_20191004_161559.jpg)

## How to do it

So there are some rules to follow when whiteboarding / drawing for a group

- No UML!!! 
- Limit the predefined shapes. If you need to draw a pipe for a queue, explain what it is and why a pipe works.
- A database might be a cylinder but reinforce what it is as you draw it
- Stick to boxes and lines as much as possible
- Sure, add arrows for direction
- Use colour sparingly. Two, three colours. After that people need a legend.
- Try to avoid sequence diagrams. They don’t work well for asynchrony or fan out / fan in.
- Don't mix different levels of abstraction in the same diagram. Use an inset or callout box for detail.
- No fricking UML!!! Nobody cares that you took the time to learn it once.

## Running an open inclusive session

It’s a really good idea to encourage this style of presentation and communication within an organisation. Don’t just settle for the same individuals talking at the others in your teams. Diagramming and whiteboarding can make your workplace more inclusive and democratise architecture and design. 

In our office Friday at 3pm is "This is my picture" time.

 - It lasts an hour
 - We have beer and pizza at 4:30 so people are winding down anyways, might as well maximise on the reluctance people have to start deep thinking.
 - We have a Slack channel where we remind folks that it is on
 - We make a list on the board of carry over items from last week
 - We ask for ideas, these can be any of:
   - Something you want to build
   - Something you have built
   - Something you are building
   - Something that exists
   - Something you can draw
   - Something you want to see someone else draw
   - Something you know
   - Something you want to know
 - List the ideas on the board
 - Now ask for a show of hands on each item in turn
 - Mark the votes beside each item
 - Start with the highest voted item and ask for volunteers to draw it
 - If nobody is willing to draw it or it doesn't get enough votes then it can carry over
 - Almost always it spills over in discussion to drinks and pizza
 - Live slack the list of ideas so that stragglers can opt in
 - Take photos of the presenters with the end product. Like a kid holding up their picture to the class. Post them to the Slack channel for posterity.
 - Don't be afraid to repeat ideas weeks or months later. Try a different presenter. There will be new people present and/or different perspectives in the room.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I run an open whiteboarding session on Fridays in our office. For Christmas my team made me some magnetic icons. All the usual ones are there AWS Buckets, Route 53, EC2, MLP, potatoes... <a href="https://t.co/KSt3n65zrP">pic.twitter.com/KSt3n65zrP</a></p>&mdash; Peter Goodman (@petegoo) <a href="https://twitter.com/petegoo/status/1207413304877932544?ref_src=twsrc%5Etfw">December 18, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## FAQ

**Q**. What about having multiple people working together on a board?  
**A**. This is ok for a discussion between two people but is confusing for a group. Try to avoid unless the two presenters have a good collaborative presentation style. You're telling a story after all.

**Q**. What about remote teams / individuals?  
**A**. Well sometimes we try to do it on hangouts and/or record it. This is _ok_. YMMV

**Q**. What about digital / online tools.   
**A**. Keen to hear about suggestions but these can be prohibitively expensive and clunky.

**Q**. Is this just for developers?  
**A**. Absolutely not. I've been trying to get more QAs, UX, Designers, Product people involved.


