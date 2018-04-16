---
layout: post
title: "Concourse on AWS: Worker lifecycle management"
categories:
- blog
tags:
- aws
- petegoo
- concourse
- ci
- cd
- pipelines
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
Lately we've been running [Concourse CI](https://concourse-ci.org/) for a bunch of our builds. 
We really love Concourse for the pipeline features, ease of configuration, and docker primitives.
However, operating and feeding Concourse can be a voyage of discovery and sometimes sadness.

One of the issues with Concourse is that it doesn't really like it when workers disappear on it.
The workers will appear as `stalled` if you run `fly workers`. This means that any resources that
are performing `check` operations for new versions will be stuck and not trigger builds.
You then need to `prune-worker` if you want your builds to keep working. 

This post aims to give you the basics for getting lifecycle management a bit better so you can
simply roll the instances in your worker pool Auto-Scaling Group (ASG) when you want to get some 
fresh ones without incurring the annoyance of having to clear out those stalled workers.

[All code available here](https://github.com/PeteGoo/packer-win-aws)

## Lifecycle Hook

Hopefully you are running your Concourse workers in an Auto-Scaling Group. When your ASG removes
these instances nothing will tell Concourse that they are dead. To make this happen you need to
create an [Auto-Scaling Lifecycle Hook](https://docs.aws.amazon.com/autoscaling/ec2/userguide/lifecycle-hooks.html). 

[Create a lifecycle hook](https://docs.aws.amazon.com/autoscaling/ec2/userguide/lifecycle-hooks.html#adding-lifecycle-hooks) for termination called `worker-terminating`.

Add the following script in a CRON job run every minute or two.

```bash
#!/bin/bash

# Need this path to allow aws command to work
PATH=$PATH:/usr/local/bin

instance_id=$(curl -s http://169.254.169.254/latest/meta-data/instance-id/)

lifecycleState=$(aws autoscaling describe-auto-scaling-instances --instance-ids $instance_id --query 'AutoScalingInstances[0].LifecycleState' --output text --region us-west-2)

if [ "$lifecycleState" == "Terminating:Wait" ]; then
  asg=$(aws autoscaling describe-auto-scaling-instances --instance-ids $instance_id --query 'AutoScalingInstances[0].AutoScalingGroupName' --output text --region us-west-2)
  
  # We store the TSA Host parameter
  TSA_HOST="my.tsa.host"

  concourse retire-worker \
  	--name $(hostname) \
  	--tsa-host $TSA_HOST \
  	--tsa-public-key /path/to/tsa-public-key \
  	--tsa-worker-private-key /path/to/tsa-worker-private-key

  # Sleep for 10 minutes to let the builds finish. I know, not ideal but it works for now
  sleep 10m

  service concourse-worker stop

  aws autoscaling complete-lifecycle-action \
    --instance-id $instance_id \
    --auto-scaling-group-name $asg \
    --lifecycle-hook-name "worker-terminating" \
    --lifecycle-action-result "CONTINUE" \
    --region us-west-2
fi

```