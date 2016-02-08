---
layout: post
title: "Okay, but What Does Your Work Actually Mean, Nikki? Part 3: Translating A Standard Into Code"
date: 2016-02-08T10:38:12-07:00
tags: outreachy, planet
---

Over my previous two posts, I described my introduction to work on Servo, and my experience with learning and modifying the Fetch Standard. Now I'm going to combine these topics today, as I'll be talking about what it's like putting the Fetch Standard into practice in Servo. The process is roughly: I pick a part of the Fetch Standard to implement or improve; I write it on a step-by-step basis, often asking many questions; then when I feel it's mostly complete, I submit my code for review and feedback.

I will talk about the review aspect in my next post, along with other things, as this entry ended up being pretty long!

## Where To Start? ##

Whenever I realize I'm not sure what to be doing for the day, I go over my list of tasks, often talking with my project mentor about what I can do next. There's a lot more work then I could manage in any internship - especially a 3 month long one - so having an idea of what aspects are the most important is good to keep in mind. Plus, I'm not equally skilled or knowledgeable about every aspect of Fetch or programming in Rust, and learning a new area more than halfway through my internship could be a significant waste of time. So, the main considerations are: "How important is this for Servo?", "Will it take too long to implement?", and "Do I know how to do this?".

#### "How important is this for Servo?" ####

Often, my Servo mentor or co-workers are the only people who can answer "How important is this?", since they've all been with Servo for much longer than me, and take a broader level view- personally, I only think of Servo in terms of the Fetch implementation, which isn't too far off from reality: the Fetch implementation will be used by a number of parts of Servo which can easily use any of it, with clear boundaries for what should be handled by Fetch itself.

I'm not too concerned with what's most important for the Fetch implementation, since I can't answer it by myself. There's always multiple things I could be doing, and I have a better idea for answering the other two aspects.

#### "Will it take too long to implement?" ####

"Will it take too long to implement?" is probably the hardest question, but one that gets just a bit easier all the time. Simply put, the more code I write, the better I can predict how long any specific task will take me to accomplish. There are always sort of random chances though: sometimes I run into surprising blocks for a single line of code; or I spend just half a day writing an entire Fetch step with no problems. Maybe with years of experience I will see those easy successes or hard failures coming as well, but for now, I'll have to be content with rough estimates and a lenient time schedule.

#### "Do I know how to do this?" ####

The last question, "Do I know how to do this?", depends greatly on what "this" is for me to be able to answer. Learning new aspects of Rust is always a closed book to me, in a way- I don't know how difficult or simple any aspect of it will be to learn. Not to mention, just reading something has a minimal effect on my understanding. I need to put it into practice, multiple times, for me to really understand what I'm doing.

Unfortunately, programming Servo (or any project, really) doesn't necessarily line up the concepts I need to learn and use in a nice order, so I often need to juggle multiple concepts of varying complexity at once. For areas not specific to Rust though, I can often have a better idea of. Generalized programming ideas though, I can better gauge my ability for. Writing tests? Sure, I've done that plenty- it shouldn't be difficult to apply that to Rust. Writing code that handles multiple concurrent threads? I've only learned enough to know that those buzzwords mean something- I'd probably need a month to be taught it well!

## An Example Of Deciding A Task ##

Right now, and for the past while, my work on Servo has been focused on writing tests to make sure my implementation of Fetch, and the functions written previously by my co-workers, conform to what's expected by the Fetch Standard. What factors in to deciding this is a good task though?

#### Importance ####

Most of the steps for Fetch are mostly complete by now. The steps that aren't coded either cannot be done yet in Servo, or are not necessary for a minimally working Fetch protocol. Sure, I can make the Rust compiler happy- but just because I can run the code at all doesn't mean it's working right! Thus, before deciding that the basics of Fetch have been perfected and can be built on, extensive test coverage of the code is significantly important. Testing the code means I can intentionally create many situations, both to make sure the result matches the standard, and that errors come up at only the defined moments.

#### Time ####

Writing the tests is often straightforward. I just need to add a lot of conditionals, such as: the Fetch Standard says a basic filtered response has the `type` "basic". That's simple- I need to have the Fetch protocol return a basic filtered response, then verify that the `type` is "basic"! And so on for every declaration the Fetch Standard makes. The trickier side of this though is that I can't be absolutely sure, until I run a test, whether or not the existing code supports this.

It's simple on paper to return a basic filtered response- but when I tell my Fetch implementation to do so, maybe it'll work right away, or maybe I'm missing several steps or made mistakes that prevent it from happening! The time is a bit open ended as a result, but that can be weighed with how good it would be to catch and repair a significant error.

#### Knowledge ####

I have experience with writing tests, as I like them conceptually very much. Often, testing a program by hand is slow, difficult, and hard to reproduce errors. When the testing itself is written in code, everything is accessible and easily repeatable. If I don't know what's going on, I can update the test code to be more informative and run it again, or dig into it with a debugger. So I have the knowledge of tests themselves- but what about testing in Rust, or even Servo?

I can tell you the answer: I hadn't foreseen much difficulty (reading over how to write tests in Rust was easy), but I ended up lacking a lot of experience with testing Servo. Since Servo is a browser engine, and the Fetch protocol deals with fetching any kind of resource a browser access, I need to handle having a resource on a running server for Fetch to retrieve. While this is greatly simplified thanks to some of the libraries Servo works with, it still took a lot of time for to have a good mental model to understand what I was doing in my head and thus, be able to write effective tests.

## Actually Writing The Code ##

So I've said a lot about what goes into picking a task, but what about *actually* writing the code? That requires knowing how to a translate a step from the programming language-abstracted Fetch Standard into Rust code. Sometimes this is almost exactly like the original writing, such as step 1 of the [Main Fetch](https://fetch.spec.whatwg.org/#main-fetch) function, "Let response be null", which in Rust looks like this: `let response = None;`.

In Rust, the `let` keyword makes a variable binding- it's just a coincidence that the Main Fetch step uses the same word. And Rust's null operator is called `None` (which declares a variable that is currently holding nothing, and cannot be used for anything while still None, but it sets aside some memory now for `response` to be used later).

## A More Complex Step ##

Of course, not every step is so literal to translate to Rust. Take step 10 of Main Fetch for instance: "If main fetch is invoked recursively, return response". The first part of this is knowing what it's saying, which is "if main fetch is invoked by itself or another function it invokes (ie., invoked recursively), return the response variable". Translating that to Rust code gives us `if main fetch is invoked recursively { return response }`. This isn't very good- `main fetch is invoked recursively` isn't valid code, it's a fragment of an English sentence.

The step doesn't answer this for me, so I need to get answers elsewhere. There's two things I can do: keep reading more of the Fetch Standard (and check personal notes I've made on it), or ask for help. I'll do both, in that order. Right now, I have two questions I need answers to: "When is Main Fetch invoked recursively?", and "How can I tell when Main Fetch is invoked recursively?".

#### Doing My Own Research ####

I often like to try to get answers myself before asking other people for help (although sometimes I do both at the same time, which has lead to me answering my own questions immediately after asking a few times). I think it's a good ideal to spend a few minutes trying to learn something on my own, but to also not let myself be stuck on any one problem for about 15 minutes, and 30 minutes at the absolute worst. I want to use my own time well- asking a question I can answer on my own shortly isn't necessary, but not asking a question that is beyond my capability can end up wasting a lot more time.

So I'll start trying to figure out this step by answering for myself, "When is Main Fetch invoked recursively?". Most functions in the Fetch Standard invoke at least one other function, which is how it all works- each part is separated so they can be understood in smaller chunks, and can easily repeat specific steps, such as invoking Main Fetch again.

What I would normally need to do here is read through the Fetch Standard to find at least one point where Main Fetch is invoked from itself or another function it invokes. Thankfully, I don't have to go read through each of those two functions, and everything they call, and so on, until I happen to get my answer, because of part of my notes I took earlier, when I was reading through as much of the Fetch Standard as I could.

I had decided to make short notes declaring when each Fetch function calls another one, and in what step it happens. At the time I did so to help give me an understanding of the relations between all the Fetch functions- now, it's going to help me pinpoint when Main Fetch is invoked recursively! First I look for what calls Main Fetch, other than the initial [Fetch](https://fetch.spec.whatwg.org/#fetching) function which primarily serves to set up some basic values, then pass it on to Main Fetch. The only function that does so is [HTTP Redirect Fetch](https://fetch.spec.whatwg.org/#http-redirect-fetch), in step 15. I can also see that [HTTP Fetch](https://fetch.spec.whatwg.org/#http-fetch) calls HTTP Redirect Fetch in step 5, and that Main Fetch calls HTTP Fetch in step 9.

That was easy! I've now answered the question: "Main Fetch is invoked recursively by HTTP Redirect Fetch."

#### Questions Are Welcome ####

However, I'm still at a loss for answering "How can I tell when Main Fetch is invoked recursively?". Step 15 of HTTP Redirect Fetch doesn't say to set any variable that would say "oh yeah, this is a recursive call now". In fact, no such variable is defined by the Fetch Standard to be used!* So, I'll ask my Servo mentor on IRC.

This example I'm covering actually happened a month or so ago, so I'm not going to pretend I can remember exactly what the conversation was. But the result of it was that the solution is actually very simple: I add a new boolean (a variable that is just `true`, or `false`) parameter (a variable that must be sent to a function when invoking it) to Main Fetch that'll say whether or not it's being invoked recursively. When Main Fetch is invoked from the Fetch function, I set it to `false`; when Main Fetch is invoked from HTTP Redirect Fetch, I set it to `true`.

#### Repeat ####

This is the typical process for every single function and step in the Fetch Standard. For every function I might implement, improve, or test, I repeat a similar decision process. For every step, I repeat a similar question-answer procedure, although unfortunately not all English to code translations are so shortly resolved as the examples in this post.

Sometimes picking a new part of the Fetch Standard to implement means it ends up relying on another piece, and needs to be put on hold for that, or occasionally my difficulty with a step might result in a request for change to the Fetch Standard to improve understanding or logic, as I've described in previous posts.

This, in sum with the other two parts of this series, effectively describes the majority of my work on Servo! Hopefully, you now have a better idea of what it all means.

Fourth post ending.

* After sharing a first draft of my post, it was pointed out that adding this to the Fetch spec would be simple to do, so this case will likely soon become irrelevant for understanding Fetch. But it's still a neat, concise example, if easily outdated.