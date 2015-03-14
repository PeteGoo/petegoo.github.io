---
layout: post
title: Tips and tricks for integrating GitHub + TeamCity
categories:
- blog
tags:
- teamcity
- slack
- github
- notifications
- builds
- chatops
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

For a while now I've been using the GitHub + TeamCity + Slack combination and I though it would be useful to write down the various tactics and tools for getting the most out of this pretty common configuration of tools.

You don't need to be using all these tools and services to get something out of these posts but the combination of all three can be pretty powerful.

This post will concentrate on getting the most out of TeamCity + GitHub

The first thing you probably have already done is to configure GitHub as a "VCS Root" in TeamCity. If you haven't then [follow the instructions](https://confluence.jetbrains.com/display/TCD8/Git+%28JetBrains%29) to get it setup. Note that now you can [create a project from a URL](https://www.youtube.com/watch?v=_FzdCC9imDs), this is often the easiest way to setup a new project around a GitHub repository.

## Matching TeamCity users with GitHub users.
Obviously GitHub and TeamCity have their own lists of users. With GitHub and other git services like BitBucket it is important to understand that your GitHub user account is not automatically stamped against each commit that you make. In fact it is up to you to make sure that the correct name and email is configured in the git clone on your machine in order for these commits to be correctly attributed to you. GitHub will then do it's best to show your avatar against your commits and track your stats by looking at your commits. If however this information is not configured in your account there is no easy way to update this information without rewriting history. 

So the first step is to [correctly configure](https://help.github.com/articles/setting-your-username-in-git/) your user.name and user.email git configuration.

    git config user.name "Joe Bloggs"
    git config user.email "joe.bloggs@example.com"

The next thing to do is to make sure that TeamCity is configured to correctly match this information with your TeamCity user. In the Advanced VCS Root settings you will see the following section:

![Username style settings in VCS Roots](/images/2015/03/teamcity.usernamestyle.png)

In my experience the best thing to do is to leave this setting at the default (UserId). This will take the first part of the email configured above and use that to match against TeamCity usernames. This results in the most predictable behaviour as people tend to have various names configured but the email address will probably be quite consistent. We then tend to match our teamcity usernames with our company email address names.

If the usernames don't match you can go to your user profile in TeamCity and customise the username that will be associated with all VCS roots, a specific VCS Root or even all Git VCS Roots.

The only place the above falls down is when you also use GitHub for personal projects and you end up committing with multiple different email addresses by accident because you have a global default set. Unfortunately TeamCity doesn't allow you to setup multiple alternative usernames so some of your commits won't get matches. Hopefully this will be resolved at some point in TeamCity.

## Reporting build status to GitHub 

One of the coolest features in GitHub is the ability to have your build process report progress to GitHub. The result of this is that your branches, commits and pull requests will be marked as pending, failed or succeeded. This really comes into it's own with Pull Requests.

![Branches view with build status](/images/2015/03/branches-with-status.png)

![Pull Request view with build status](/images/2015/03/pr-with-build-status.png)

To enable TeamCity to be able to tell GitHub about the build status you need to download and install the [TeamCity.GitHub plugin](https://github.com/jonnyzzz/TeamCity.GitHub). 

Note that you can upload plugin .zip files to the plugins folder using the administration pages on TeamCity, just remember to restart the service for the change to take effect. Also note that for the pull request part to work you will need to make sure you are building branches and PRs as required (see below).

## Building branches and pull requests

By default TeamCity will probably only be building `master`. To enable other branches to get built you will have to also add a [branch specification on the VCS Root settings](https://confluence.jetbrains.com/display/TCD8/Working+with+Feature+Branches#WorkingwithFeatureBranches-Configuringbranches).

![Branch specification](/images/2015/03/branch-specification.png)

The branch specification syntax takes a little getting used to but here are some useful examples. Note that each one should appear on a separate line.

* `+:<default>` - include master
* `+:refs/heads/(*)` - include all branches
* `-:refs/heads/(spikes-*)` - exclude any branches that start with `spike-`
* `+:refs/pull/(*)/head` - include all pull requests
* `+:refs/pull/(*)/merge` - include the merge result of pull requests *(see below)

The parenthesis `()` allow you to specify the part of the branch syntax that will be used as the branch name in the TeamCity UI.

[Building the merge result of pull requests](http://blog.jetbrains.com/teamcity/2013/02/automatically-building-pull-requests-from-github-with-teamcity/) with `refs/pull/(*)/merge` is a pretty cool idea. Basically it means that when GitHub knows that the potential merge result of a pull request would change then a build will trigger that not only looks at the PR but attempts to merge it into the parent branch as if someone had pressed the green `merge` button in GitHub, before building the code. This seems cool but there are number of problems mostly in that the builds will be triggered ALL THE TIME and your build queue gets swamped with all your PRs building. For example when someone looks at the PR on github.com [it will trigger a new build](https://twitter.com/bradwilson/status/574702084370509824) if it detects that something could change in the merge result, we found that as people were skimming over PRs on github.com our TeamCity server got completed swamped. Therefore, we don't use this feature, instead we just don't keep long running feature branches.

Note that if you setup a VCS Trigger to initiate your builds when someone has pushed code to GitHub then you can also also specify the same branch syntax in the trigger branch filter field.

## Triggering new builds when someone pushed code

By default TeamCity can be configured with a VCS trigger that polls the git repository looking for changes. If your TeamCity server can be reached on the open internet then you can ask GitHub.com to tell TeamCity that changes have been made the instant someone pushed coded to GitHub.

To do this you need to go to the Settings of your repository then add the TeamCity service from the WebHooks and Services panel. It may require a username and password unless you have guest access enabled.

# Using a Mac OS TeamCity agent with a Windows TeamCity Server

This is more of a warning around a very specific set of circumstances. If the following is true:

- You have a Windows TeamCity server
- You setup a Mac OSX TeamCity agent (e.g. to run iOS builds)
- Your repo has symlinks in it (like in cucumber / calabash tests)

The JGit client used in TeamCity can be a royal pain-in-the-ass sometimes. In the above scenario it will turn those symlinks into useless empty files that freak your build out. You will need to change the VCS settings to "checkout on agent" instead of "checkout on server" meaning that the Windows server will not be trying to send the file changes to a Mac OSX client and failing horribly.


# Links

- [Git in TeamCity documentation](https://confluence.jetbrains.com/display/TCD8/Git+%28JetBrains%29)
- [Creating a TeamCity project from a Git Url](https://www.youtube.com/watch?v=_FzdCC9imDs)
- [Setting up your username in git](https://help.github.com/articles/setting-your-username-in-git/)
- [TeamCity plugin for GitHub Status display](https://github.com/jonnyzzz/TeamCity.GitHub)
- [Working with feature branches in TeamCity](https://confluence.jetbrains.com/display/TCD8/Working+with+Feature+Branches#WorkingwithFeatureBranches-Configuringbranches)
- [Building Pull Requests with TeamCity](http://blog.jetbrains.com/teamcity/2013/02/automatically-building-pull-requests-from-github-with-teamcity/)






