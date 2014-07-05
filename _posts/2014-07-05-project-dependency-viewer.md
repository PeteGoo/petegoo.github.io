---
layout: post
title: A simple project dependency viewer
categories:
- blog
tags:
- project
- dependency
- visual studio
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

Recently I faced an issue trying to get my head around a large codebase consisting of multiple solutions and many many projects. The difficulty was in trying to understand the interdependencies between these projects, espectially the ones that are in different solutions. There are tools to do this. NDepend has some neat stuff, Visual Studio Ultimate Edition can do some things and there are others. For my simple scenario though I couldn't justify the licensing cost.

Luckily I knew that Visual Studio supports the [DGML file format](http://en.wikipedia.org/wiki/DGML) in all editions. DGML is essentially a file format where you specify a number of nodes and then links between them as below.

```xml
<?xml version='1.0' encoding='utf-8'?>
<DirectedGraph xmlns="http://schemas.microsoft.com/vs/2009/dgml">
  <Nodes>
    <Node Id="1" Label="MyCompany.Core" />
    <Node Id="2" Label="MyCompany.Area1.Service" />
    <Node Id="3" Label="MyCompany.Area1.Service" />
  </Nodes>
  <Links>
    <Link Source="2" Target="1" />
    <Link Source="1" Target="1" />
  </Links>
</DirectedGraph>
```

Open this file in Visual Studio and you get a nice designer view where you can arrange and change things as you require.

![dgml file in Visual Studio](/images/2014/07/2014-07-05 12_31_39-test.dgml.png)

So I created a simple tool that will generate a diagram like this for you when you give it a folder. It will search the child folders for any csproj, vcxproj and vbproj files, calculate their references and give you the relevant DGML file for you to analyse your dependencies. Simple stuff really.

The repo and binaries are on github at [PeteGoo/ProjectDependencyVisualiser](https://github.com/PeteGoo/ProjectDependencyVisualiser/). Enjoy.
