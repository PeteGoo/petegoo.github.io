---
layout: post
title: "Better Octopus Registration"
categories:
- blog
tags:
- petegoo
- cd
- octopus
- deploy
- deployment
- octopus-deploy
- lambda
- aws
- security
- ec2
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
We use [Octopus Deploy](https://octopus.com) for a lot of our deployments. It has great primitives that help us create simple, reliable, repeatable deployment processes. 

If you are using it to deploy code onto machines you typically use the supplied agent, installed on your machine. Octopus calls this agent a Tentacle, naturally. The Tentacle registers itself with the API on the Octopus server. Your machine will define which roles it would like to perform and these roles can be used to define which deployment steps will run for that machine.

## The Problem

There is an issue with this registration approach however. In order to be able to register, the Tentacle needs an API key. You can scope that API key to an Environment like Test, Prod etc but not, as far as I can tell, to a role. Therefore **the tentacle can ask to be any role it likes**. Even if you could restrict the API key to a role, managing the keys and their scopes would be a nightmare.

So now we know that a machine we intend to be `non-sensitive-role`, if it were compromised by an attacker can now register (or re-register) itself as `very-sensitive-role`, essentially creating a form of [lateral movement](https://en.wikipedia.org/wiki/Network_Lateral_Movement). For example, if the `very-sensitive-role` delivered some code with a database connection string and password from Octopus variables but  `non-sensitive-role` was never designed to get those secrets then we have a problem.

![old octopus registration](/images/2018/octopus_reg_old.png)

## The Solution

So how do we get around this. Well, we wanted to eliminate the reliance on the machine telling us what role it should be. Our machines are in AWS and we can use EC2 Tags to add metadata to our machines. So we added an `OctopusRole` tag when AWS creates the machine (EC2 instance) with the name of the role(s) intended to be used by that machine. You can also add `OctopusMachinePolicy` if you want.

Then when we want to register the machine on startup, we remove the need for access to the API by instead publishing an SNS message that simply has the EC2 Instance Id and any other useful information like the Tentacle thumbprint.

This SNS message triggers a Lambda which uses the EC2 APIs to query the metadata for the instance. The lambda then registers with Octopus on behalf of the instance. Octopus will subsequently reach out to the machine to establish the connection to the listening Tentacle agent.

![new octopus registration](/images/2018/octopus_reg_new.png)

## What did we learn?

Basically validate your client inputs. They can lie like terrible lying things. 

Lambda is great piece of glue you can use to solve these types of problems. Now you can even write them in Powershell should you so desire. Personally I write most of mine in Python but you can choose your poison without needing to change this pattern.

