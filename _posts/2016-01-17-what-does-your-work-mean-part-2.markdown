---
layout: post
title: "Okay, But What Does Your Work Actually Mean, Nikki? Part 2: The Fetch Standard and Servo"
date: 2016-01-17T13:20:27-07:00
tags: outreachy, planet
---

In my previous post, I started discussing in more detail what my internship entails, by talking about my first contribution to Servo. As a refresher, my first contribution was as part of my application to Outreachy, which I later revisited during my internship after a change I introduced to the HTML Standard it relied on. I'm going to expand on that last point today- specifically, how easy it is to introduce changes in [WHATWG](https://wiki.whatwg.org/wiki/FAQ#What_is_the_WHATWG.3F)'s various standards. I'm also going to talk about how this accessibility to changing web standards affects how I can understand it, how I can help improve it, and my work on Servo.

## Two Ways To Change ##

There are many ways to [get involved with WHATWG](https://wiki.whatwg.org/wiki/What_you_can_do), but there are two that I've become the most familiar with: firstly, by opening a discussion about a perceived issue and asking how it should be resolved; secondly, by taking on an issue approved as needing change and making the desired change. I've almost entirely only done the former, and the latter only for some minor typos. Any changes that relate directly to my work, however minor, are significant for me though! Like I discussed in my previous post, I brought attention to [an inconsistency](https://github.com/whatwg/html/issues/296) that was resolved, giving me a new task of updating my first contribution to Servo to reflect the change in the HTML Standard. I've done that several times since, for the Fetch Standard.

## Understanding Fetch ##

My first two weeks of my internship were spent on reading through the majority of the [Fetch Standard](https://fetch.spec.whatwg.org/), primarily the various Fetch functions. I took many notes describing the steps to myself, annotated with questions I had and the answers I got from either other people on the Servo team who had worked with Fetch (including my internship mentor, of course!) or people from WHATWG who were involved in the Fetch Standard. Getting so familiar with Fetch meant a few things: I would notice minor errors (such as an out of date link) that I could submit a [simple fix for](https://github.com/whatwg/fetch/pull/173), or a bigger issue that I couldn't resolve myself.

## Discussions & Resolutions ##

I'm going to go into more detail about some of those bigger issues. From my perspective, when I start a discussion about a piece of documentation (such as the Fetch Standard, or reading about a programming library Servo uses), I go into it thinking "Either this documentation is incorrect, or my understanding is incorrect". Whichever the answer is, it doesn't mean that the documentation is bad, or that I'm bad at reading comprehension. I understand best by building up a model of something in my head, putting that to practice, and asking a lot of questions along the way. I learn by getting things wrong and figuring out why I was wrong, and sometimes in the process I uncover a point that could be made more clear, or an inconsistency! I have good examples of both of the different outcomes I listed, which I'll cover over the next two sections.

### Looking For The Big Picture ###

Early on in my initial review of the Fetch Standard's several protocols, I found a major step that seemed to have no use. I understood that since I was learning Fetch on a step-by-step basis, I did not have a view of the bigger picture, so I asked around what I was missing that would help me understand this. One of the people I work with on implementing Fetch agreed with me that the step seemed to have no purpose, and so we decided to [open an issue](https://github.com/whatwg/fetch/issues/174) asking about removing it from the standard. It turned out that I had actually missed the meaning of it, as we learned. However, instead of leaving it there, I shifted the issue into asking for some explanatory notes on why this step is needed, which was fulfilled. This meant that I would have a reference to go back to should I forget the significance of the step, and that people reading the Fetch Standard in the future would be much less likely to come to the same incorrect conclusion I had.

### A Confusing Order ###

Shortly after I had first discovered that apparent issue, I found myself struggling to comprehend a sequence of actions in another Fetch protocol. The specification seemed to say that part of an early step was meant to only be done after the final step. I unfortunately don't remember details of the discussion I had about this- if there was a reason for why it was organized like this, I forget what it was. Regardless, it was agreed that [moving those sub-steps](https://github.com/whatwg/fetch/issues/176) to be actually listed after the step they're supposed to run after would be a good change. This meant that I would need to re-organize my notes to reflect the re-arranged sequence of actions, as well as have an easier time being able to follow this part of the Fetch Standard.

## A Living Standard ##

Like I said at the start of this post, I'm going to talk about how changes in the Fetch Standard affects my work on Servo itself. What I've covered so far has mostly been how changes affect my understanding of the standard itself. A key aspect in understanding the Fetch protocols is reviewing them for updates that impact me. WHATWG labels every standard they author as a "[Living Standard](https://wiki.whatwg.org/wiki/FAQ#What_does_.22Living_Standard.22_mean.3F)" for good reason. It was one thing for me to learn how easy it is to introduce changes, while knowing exactly what's going on, but it's another for me to understand that anybody else can, and often does, make changes to the Fetch Standard!

### Changes Over Time ###

When an update is made to the Fetch Standard, it's not so difficult to deal with as one might imagine. The Fetch Standard always notes the last day it was updated at the top of the document, I follow a Twitter account that [posts about updates](https://twitter.com/fetchstandard), and all the history can be [seen on GitHub](https://github.com/whatwg/fetch/commits) which will show me exactly what has been changed as well as some discussion on what the change does. All of these together alert me to the fact that the Fetch Standard has been modified, and I can quickly see what was revised. If it's relevant to what I'm going to be implementing, I update my notes to match it. Occasionally, I need to change existing code to reflect the new Standard, which is also easily done by comparing my new notes to the Fetch implementation in Servo!

### Snapshots ###

From all of this, it might sound like the Fetch Standard is unfinished, or unreliable/inconsistent. I don't mean to misrepresent it- the many small improvements help make the Fetch Standard, like all of WHATWG's standards, better and more reliable. You can think of the status of the Fetch Standard at any point in time as a single, working snapshot. If somebody implemented all of Fetch as it is now, they'd have something that works by itself correctly. A different snapshot of Fetch is just that- different. It will have an improvement or two, but that doesn't obsolete anybody who implemented it previously. It just means if they revisit the implementation, they'll have things to update.

Third post over.