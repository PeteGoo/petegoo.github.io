---
layout: post
title: TeamCity Slack Build Notifier
categories:
- blog
tags:
- teamcity
- slack
- build
- notifications
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

We have become immense fans of [Slack](http://slack.com) in our office, as have lot of people I know in the software development industry. If you haven't heard of Slack, well it's basically a chat system for businesses, much like HipChat or Campfire. The difference being that Slack seems to bring a healthy dose of cool to everything they do, they are iterating incredibly fast right now and seem to be hitting all the right notes.

Ok, so once you have Slack up and running, you turn to integrations with other systems that are going to maximise on the true [ChatOps](https://www.youtube.com/watch?v=NST3u-GjjFw) experience. There are already integrations with JIRA, New Relic, GitHub, Twitter and just about every other thing you can think of. 

I started to look for a way to have TeamCity notify build results directly into Slack channels (rooms) and found that there were a few options I could have gone with. I could of course use my own chat bot project [mmbot](https://github.com/mmbot) to do the notifications for me but I would either need to poll TeamCity continuously or use a webhooks plugin. There is a very good [webhooks plugin available already](https://netwolfuk.wordpress.com/category/teamcity/tcplugins/tcwebhooks/) for TeamCity, the only thing is it doesn't support commit messages / users and it would bring in a chain of communication that is not strictly necessary. Nope, I wanted a plugin for TeamCity that would report directly to Slack.

There is a [Slack plugin already for TeamCity](https://github.com/Tapadoo/TCSlackNotifierPlugin) but I wasn't too keen on the way the notifications looked or the reliance on XML configuration to set it up for each project. 

So I decided to take the plunge, learn some Java and setup a new plugin to report build status directly from TeamCity into a Slack room. Initially I started by taking a lot of inspiration from the [tcWebHooks plugin](https://netwolfuk.wordpress.com/category/teamcity/tcplugins/tcwebhooks/) I mentioned above. I really liked the configuration experience for this plugin and wanted that experience for my users.

I ended up using [IntelliJ IDEA from JetBrains](http://www.jetbrains.com/idea/), this was by far the easiest IDE to setup although for a n00b it was still really painful in java-land. I'm not sure how much of this was pre-conception vs freaky hard configuration in the JDK etc. The build system is Maven and everything else is largely simple stuff.

![Pass](/images/2014/07/2014-07-13 21_58_13-build-status_pass.png)
![Fail](/images/2014/07/2014-07-13 21_58_13-build-status_fail.png)
![Configuration](/images/2014/07/2014-07-13 21_58_13-build-slack-config.png)

As usual the [code is on GitHub at PeteGoo/tcSlackBuildNotifier](https://github.com/PeteGoo/tcSlackBuildNotifier).
