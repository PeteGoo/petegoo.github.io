---
layout: post
title: "Small Pull Requests and Batch Size"
categories:
- blog
tags:
- batch
- petegoo
- flow
- programming
- efficiency
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

<sub>_The below is from an internal memo I wrote on reducing batch size in pull requests_</sub>

One of the things I find most interesting about my role these days is that I get to talk with such a wide variety of people across our engineering group and I get to hear various challenges that they face. As a result I get to observe common patterns and themes that emerge which often stem from similar issues. I’d like to talk about one of those common issues - Batch Size.

### My experience

When I joined my current company many years ago now there was something about the way that we worked that took me by surprise. It was fast, but more than that I felt more focused, I was more engaged than I had ever been and I was learning rapidly. 

We deployed multiple times per day. We were reviewing each other's code multiple times per day. Our poor QA (singular) was getting smashed but managing to keep up with this mayhem. How was this possible? I’d never seen this pace before. 

The reason it worked was that we had made each change or “Pull Request” small and incremental. We had a saying that was basically “do the smallest, dumbest thing you can to learn the next thing”.

### The science bit

The advantages of breaking tasks down into smaller chunks is something we have all experienced in lots of aspects of our lives. 

In [manufacturing](https://en.wikipedia.org/wiki/Lean_manufacturing) and economics circles this is sometimes referred to as Lot or Batch Size and it’s an important factor in the throughput and efficiency of any system. Don Reinertsen does a [really good job of explaining the theory](https://www.youtube.com/watch?v=zVASqSj_kvc) but ultimately reducing the size of the changes we make leads to reduced cycle times, consistent flow, faster feedback, reduced risk, fewer overheads, greater efficiency, higher motivation and reduced costs. 

Our batch is a Pull Request. It is the car in our assembly line.<br/>
<sub>_(If you have a long lived feature branch then your batch is the feature branch))_</sub>

### How does this benefit me (you)?

The weird thing about my experience was that it also caused me to change my behaviour and the relationship I had with my code. I used to write a bunch of code on my machine, add to it, add more, refactor, add more, test it, clean it up, write tests (whoops), double check it, triple check it, then eventually let someone else see it when I knew it was safe for me to do so. I was optimising for never being wrong, not learning to improve. I built up so much stress during this process that it was a rollercoaster of fear and insecurity. [Not good](https://rework.withgoogle.com/blog/five-keys-to-a-successful-google-team/).

In my current company however, if I had more than a day's work on my machine I started to get nervous, like I was walking around with a wallet full of too much cash.

There are other benefits though. Code reviews are quick and easy. More tests are automated. QAs are able to focus on what matters. Product and UX can provide timely feedback. Course corrections happen earlier before wasted effort. Incident impacts are smaller and downtime is shorter because when things go wrong in production it’s easy to see what changed. We do fewer revolutions, big refactorings, rewrites. Long lived feature branches and merge conflict resolutions are a thing of the past. We focus on continually providing value, learning from how our customers use our software and responding to their changing needs.

This isn’t to say that we shouldn’t design software or plan how we will implement it. This is still a skill we need to exercise, however no design or plan is perfect so why would we wait to find that out?
