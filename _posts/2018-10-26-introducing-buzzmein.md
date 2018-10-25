---
layout: post
title: "Introducing BuzzMeIn: Just in time access to your apps in AWS via Slack"
categories:
- blog
tags:
- aws
- petegoo
- buzzmein
- lambda
- remote-access
- vpn
status: publish
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
Hosting your internal apps in AWS is easy, knock up a container, an Application Load Balancer for 
ingress and add security group rules to allow your office ip addresses access. Maybe you add a VPN but 
now you have to provision more users, have a separate identity provider like AD, rotate credentials
etc.

Eventually you start to add your user's home IP addresses to the security group but now they're on 
the road and need access from a hotel or cafe. What now?

![/buzzmein](/images/2018/buzzmein_128x128.png)

BuzzMeIn creates short-lived temporary security group leases by adding your users IP address when
they request it via Slack. You can whitelist users or a channel, handy if you just wnat to invite people 
to a private channel from which they can issue the Slash command.

You can even add a Duo 2FA prompt for added piece of mind.

## Show Me

Your users use a slash command in Slack

![/buzzmein teamcity](/images/2018/buzzmein_slashcommand.png)
 

BuzzMein replies with a one-time use link to your app

![/buzzmein teamcity](/images/2018/buzzmein_response.png)

The user clicks on the link and gets an optional Duo 2FA prompt followed by the message...

```bash
Please wait. Redirecting you to TeamCity
```

In the background BuzzMeIn 
- Takes note of the user's IP address
- Creates a rule in your AWS security group to grant the user access
- Redirects the user to your napp

.....time passes.....

After a predetermined lease time, BuzzMeIn will:
- Remove the security group rule it created earlier
- Message the user letting them know that their time is up

Visit the [project on GitHub](https://github.com/petegoo/buzzmein/) for more info
