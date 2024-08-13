+++
title = "An approach to shot lighting in Houdini pt.1"
date = 2022-05-11
[taxonomies]
  tags = ["houdini", "lighting", "arnold", "workflow"]
[extra]
  toc = true
  
+++

### A preface.
I’ve wanted to write this series for a while but never quite got round to it, or thought that putting it down into written form was a waste of time. However, I’ve realised over the years that there are many different schools of thought on this so perhaps writing this up will offer (yet) another way to tackle it. This post will focus on the ideas and train of thought, and there’ll be a follow up with it in practice.
<!--more-->

I will preface this with yes Solaris is here now; it has it’s place in a studio and brings a lot of benefits, but it’s not going to be for everyone. The current reality is that studios are still figuring out USD and how to approach it in a way that provides the best benefits.

### Structure is nice.
Starting with a blank scene can be lovely… when you’re at home and the idea of deadlines are but a distant thought. When you’re up against production deadlines then anything to help hit the ground running is not only appreciated, but essential. Consistency between artists, shots and departments is crucial; if everyone started off with a blank scene and told to work as they see fit you’d probably get as many different ways of doing things as there are artists!

### Templates are nicer (if done right).
Introducing one answer: templates! Considered a taboo subject sometimes, a good template should not get in your way and keep you feeling free to work as you please. With the often mundane setup of a production render scene ready to go, an artist can  start lighting a shot from the word go. It’s a balancing act, and often templates will be customised per show/sequence/shot depending on lots of factors. Having a baseline template to work off though can ensure there is always some level of consistency across the board. So what exactly does a good template do and not do?

### What’s in a scene?
There are things that pretty much every production lighting scene needs to have set up, no matter the simplicity or complexity of the shot. A typical lighting scene could be described as having the following flow of data:

```
Scene Input > Scene Assembly > Scene Output
```

**Scene Input** could be anything external used such as textures, models etc. everything used to construct the scene. These might be published through an asset management system.

**Scene Assembly** is where the scene is constructed and lit. This could be extremely complex or simple, it all depends on the show at this point. Camera, environment, characters, lighting, etc. the shot comes together here.

**Scene Output** is where the output is setup, so here it would be the renderer of choice and a typical production output would incorporate these:

- Render Settings
- Beauty Render
- Utility Render Pass
- AOV’s

Scene Output also overlaps in some ways with Scene Assembly. You need the end of Assembly to be organised in such a way that the start of Output will receive standardised conventions so it can be connected. This will make sense in practice but it’s good to think about.

### Why think like this?
Scenes can get complex, really complex. Having a clear flow of how the scene will be put together and how data flows through it can help to picture structure before you’ve even started. 

Standardised scene structure opens the door to automating and tooling even more of the day to day workflow, and to work in a batch/sequence/multi-shot manner it’s absolutely crucial to have something like a template in place. Building 100 shots without any standards in place is just simply not possible.

It can also inform decisions about standards beyond shot lighting too; what AOV’s should comp be getting from the start, how should passes be named, how should light groups be split, etc.

### So what does it look like in practice?
In the next part I’ll show the outline of a structure using in Houdini that I’ve personally used many times in production. It’s not the only way to do things but it is one way of organising a scene, one that has been proven on many jobs from small to extremely complex.

I’ll keep it renderer agnostic, but I do have a personal preference using Arnold that opens up a lot of flexibility using Operators. That’s for another blog post in itself though… in the mean time check out part 2 when it’s up and expect some images to look at!
