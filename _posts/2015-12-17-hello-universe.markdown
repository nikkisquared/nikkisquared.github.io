---
layout: post
title: "Hello Universe! First Days Of Outreachy, December 2015"
date: 2015-12-20T21:25:56-07:00
---


For the past two weeks, I've been getting started on my internship with [Outreachy](https://www.gnome.org/outreachy/)! I'm working with Mozilla. My project for the internship is to implement more of the Fetch standard into Servo! Before explaining what the heck all that means, I'm going to talk about everything leading up to my internship.

## Before Outreachy ##

So let's back up a bit. I first heard about Outreachy a year ago, when my girlfriend mentioned it. I could have applied then, but I was caught up in applying to [Recurse Center](https://www.recurse.com/) (RC), and banking on it. I got accepted into RC and spent three great months there! By the time I got back home, Outreachy no longer on my mind.

Until a few months ago, when I started taking more seriously my search for a first job. Thanks to the people I could reach through RC, I was reminded (several times!) about the possibility of applying to Outreachy- and not just reminders about it, now I knew people who had done internships under Outreachy, who they could help me get started! Wow!

## Preparing For Outreachy ##

Back in August, to get acquainted with working with Freeware Open Source Software (FOSS), one of my Outreachy-RC friends suggested I try making a first contribution to the project she worked on for her internship, which happened to be a project under Mozilla! It was a great experience, and I learned a lot about the process for FOSS contributions (at least to this one team!) in a friendly environment. The history of it is [available here](https://github.com/mozilla/mozilla_ci_tools/pull/342) on Github! Shortly after, I tested what I learned (mostly by forgetting and needing to ask for guidance again, which nobody complained about!) by making a [small patch](https://github.com/mozilla/pulse_actions/pull/18) to an adjacent project.

After that, I was right on time for the projects working with Outreachy to be announced on September 16! I wanted to try to stay within Mozilla, since all my experiences with them so far had been so positive. Thankfully, they had a stunning 8 projects available for Outreachy internships! I bounced around several, but I spent my time on two in particular: contributing to [the HTML standard](https://github.com/whatwg/html/pull/282) (exciting to make such an impact, even small!), and [getting my feet wet with Servo](https://github.com/servo/servo/pull/8218). I enjoyed working with both of them greatly, but since Servo felt like it would help me grow as a programmer the most, I decided to apply for it directly in my Outreachy application.

## Accepted! ##

After finishing my application (a day before the deadline, on November 1) there wasn't anything for me to really do but wait for a bit over two weeks, and hope that I would get accepted. Like the section header already spoils, I was very pleased to see announced on November 17 that I was one of the accepted participants! I had several more works to prepare for starting the internship, which is a nice breather. I ended up spending most of that time [making a new game](http://www.glorioustrainwrecks.com/node/9928) (my first real one in about 10 months), which was a great opportunity to make something creative, and start getting used to working a set amount of hours most every day.

My first day of the internship was on December 7, which I spent of re-acquainting myself with the goals of my project, meeting more of the people who contribute to Servo, and beginning to read the Fetch Standard. Like I promised at the start of this post, I'm going to explain what my internship *is* now.

## What Am I Working On? ##

Servo is a web rendering engine, which means it handles loading web pages and other specifications for using the web. To contrast Servo to a full-fledged browser: a browser could use Servo for its web rendering, and then put a user interface on top of it, things like bookmarks, tabs, a url bar.

For my internship, [as described here](https://wiki.mozilla.org/Outreachy/2016/December_to_March#Servo:_Complete_implementation_of_Fetch_standard), I'll be implementing more of the [Fetch Standard](https://fetch.spec.whatwg.org/) in Servo, which specifies a generalized and broad way for a web rendering engine to retrieve resources. My role is to understand as much of the Fetch Standard as I can, then start translating that into code for Servo!

## So Far, and Upcoming ##

I've spent two weeks working with Servo and Fetch so far. Getting a bit ahead of my schedule, I had finished reading through and taking notes on most of the Fetch Standard halfway through my second week! Most of it was preparation for understanding how I would even begin coding it. Thankfully, I'm not the first to start implementing Fetch in Servo, so I have all the previous work to build on, as well as several people acquainted with Fetch who I've asked many, many questions. Towards the end of my second week, I got to start applying my knowledge for Servo, which was quite exciting! I've so far made a rough draft of one of the major pieces in the Fetch Standard, which I feel proud about.

In future posts, now that I've reviewed everything before Outreachy, I'll talk in more detail about my daily experiences, as well as try to find examples of code or ideas I work with that I find cool.

End of my first post.