---
layout: post
title: "Programming For Servo: Experience And Knowledge Gained"
date: 2016-02-22T13:45:26-07:00
tags: outreachy, planet
---

# Talking About Learning #

So far I've used my Outreachy blog to talk about how I got started on Outreachy, what the start of my work for Servo was like, and some sketches of what most of my experiences have been. Today I'm going to go more in depth, by talking about what code review has been like for me, and some programming tidbits I've learned during my internship.

I assume many of the programmers reading this have had plenty of experience with code review already, but the amount of it in contributing to Servo is new to me, and I'm interested in talking about my experience! As for things I've learned- I only cover a couple of things, but that doesn't mean that's the total of everything I've gotten out of my internship. I just have a hard time breaking down ideas that wouldn't take me too long to figure out how to teach in a blog post.

## Mentoring ##

The majority of my programming experience is from projects I chose to do on my own: with my own goals, and answering to myself. It's often quite difficult to think of tasks that I'm interested in doing that *also* help me practice a new technique or a concept that I'm still rusty with. Plus, I'm not often the best judge of how "good" my code style, functionality, or understanding of concepts used is (although, all of those can be very subjective!). I occasionally compare my latest projects to earlier code and reflect on what has changed in my approach and knowledge, which is great for seeing how much I've improved, but doesn't always tell me what I need to learn next.

My Outreachy internship has by far been my best experience for having constant goals and other people to answer to. My relationship with the Servo project manager has always described him as a mentor to me, and I think that's the most fitting title. If I referred to him as my boss, I think it would likely give an inaccurate or incomplete understanding of how big of an impact he's directly had on my performance.

Having a mentor means having somebody I can directly go to with any questions I have, and also somebody who knows very well what I'm working on; any time I think I've hit a wall, my mentor has a solution, or knows where to start finding a solution, if nobody else available can figure it out! This all applies heavily to the feedback I receive on my work. 

### Reviews ###

Servo, like most any sizable programming team I reckon, has all changes to Servo peer reviewed by at least one other person before it's accepted. Now, I've of course had my code for assignments in university looked over by teachers, but the feedback was always broad and impersonal, since there are so many students in every class. I've also had several code review sessions with facilitators at Recurse Center, but it was never a complete line-by-line read-through of code I'd written, since that would have taken too much time for both of us to do together.

In contrast to either of those, everything I write for Servo receives in-depth review and feedback, something I can't receive in a normal classroom setting or from occasional review sessions. This has been fantastic for me, both as a newbie to Rust and a growing programmer in general. If I misunderstand a feature of Rust, or lack confidence in trying out a new (for me) feature that would make for more effective code, other people (including my mentor of course) suggest to me what I can do better. If I don't understand the suggestion, or if I think it's not helping solve the initial problem, I can talk about that with the reviewer or other Servo contributors until a resolution is found.

Through all of this review and discussion, my mental understanding of Rust grows every day, helping me tackle more difficult problems (my final large task is going to deal with making the Fetch implementation asynchronous!) while making fewer mistakes. Likewise, my personal tool belt of ways to approach complex programming has grown considerably thanks to all the unique problems I've handled. I'm going to spend the rest of the post detailing two such of these.

# Two Programming Examples #

Working on Servo has given me ample room to learn new concepts and what their applications are, as well as to further the knowledge of resources I already had. An example of each would be how I've learned to use a new (for me) abstract data representation, and how much better I now understand what pointers *mean*.

## A Bad Use Case For Strings ##

For a long time, whenever I made a program that used settings, I'd store them in a string. This example is actually from my last large Python project, just by the way. My code would have lines like `database = "local"`. Then, when I need to care about the `database` value, I'd compare it to one of the possible settings, such as `"local"`, or `"global"`. There are quite a few problems with this approach, although for me it wasn't obvious how bad it was, until I knew better!

#### What Makes This Difficult ####

1) A typo is a silent error. If I ever make a typo in setting `database` or the string I compare it to, that typo won't be easily caught while compiling or running the program. Setting `database` to `"lpcal"` or `"Local"` means it won't match `"local"`, through no fault of how strings work. Likewise, I could have it spelled correctly, but have a mismatch of case where I try to compare it to `"LOCAL`" by mistake. I could always convert both `database` and the value it's being compared to to lowercase or uppercase to prevent that error, but that's a tad annoying to do every time, plus that still doesn't resolve spelling typos.

2) I have to memorize what all the possible values are. In this instance, database has to be one of just `"local"` or `"global"`. What if, like a variable I use for storing commands in the same program, I have more than two possibilities? Five? A dozen? By that point, I should write down every valid value somewhere. That means I have to keep track of a non-code file, which isn't actually important to my project. And even with such a solution, typos are still possible while copying listed values.

3) Writing tests for `database` is tedious. If I write tests to make sure the value is always `"local"` or `"global"`, I might make a typo in the test itself, making it raise false alarms, or accepting an instance of the same typo. While not impossible to test properly, it would take considerate attention to detail since I often, without meaning to, correct minor typos in my head as I read. For this project, despite any tests I made, I certainly had a few frustrating to track down bugs where one of the values wasn't being set properly.

#### An Alternative ####

What's a good solution to all of these? Enums! Enums are the perfect replacement for what I'm trying to make strings do. I'll respond to each problem I listed above with how using enums solves it. If you don't know what an enum is, I don't think I'd be the best teacher for it right here- but you can take a look at how [the C language does it](https://en.wikipedia.org/wiki/Enumerated_type#C), which is where I first learned about them.

#### What Makes This Work ####

1) Enums don't have silent errors for typos. If a typo for an enum value is made, it should either A) raise a compiler error (such as in Rust), or B) cause an exception while running the program because the value doesn't exist (such as a Python implementation might do it). If I made `database` an enum with values `local` and `global`, and declared an instance of it as `locol`, an error will be raised when that line is compiled or run, pinpointing it as an invalid enum. No longer do I have to hunt down the source of a silent error!

2) I don't need to memorize the values. As the enum is declared in the code, I can always look at that to know what the values are. Since point #1 is solved, if I don't remember a value correctly and try to guess, it will raise an error and point me towards the right direction. Also, this is dependent on the (often language-specific) IDE or other coding tool used, but it's possible to have all valid values for an enum shown to me as I type, since IDEs can find connections across an entire project.

3) Tests become significantly less stressful. I do not need to worry about typos in my tests, since checking an enum value means referring to that enum. I don't have to test that an enum instance always uses *any* valid value, since the enum type does that for me. I can focus on ensuring that the enum instance is set to the correct value at certain points of the program!

#### How Did I Learn This? ####

Like I said when I introduced enums, I first learned about them in C. However, knowing about a programming concept fluently doesn't mean I understand *why* I would want to use it, or help me think about applying it when I'm tackling a new problem. I did not ever think about why enums would be used, until I saw their usage in Servo- every option or variable with multiple values has an enum for it! This is very helpful for my implementing the Fetch protocol, since there are many, many possible values for dozens of variables. I never have to worry about tedious testing or wasting time looking up possible values, since the Rust compiler will catch my mistakes for me!

## Missing Garbage ##

Rust, the programming language Servo is written in, has over the past few months become very impressive to me. I'm not going to try to explain it at a high level, since I know Rust [can speak better for itself](https://www.rust-lang.org/) than a newbie, but I'd like to focus on one aspect of it, one that stood out to me early on. Namely, how the language approaches garbage collection (basically, how a program automatically deals with freeing up memory not in use), and how that has helped me appreciate better what references are at a low level.

Some of the [first sentences](https://doc.rust-lang.org/book/README.html) I read when I started learning Rust state that "Rust is a systems programming language focused on three goals: safety, speed, and concurrency. It maintains these goals without having a garbage collector...". This made me think about my experience with the (in)famously low-level programming language C, where everything needs to be handled by the programmer, including much of the memory management. C is renowned for speed, but while I can't comment on concurrency in C, I do know it has very little safety built in to the language.

#### Memory ####

Since Rust doesn't force you to manually allocate or deallocate memory (which can easily lead to memory leaks and other dangerous bugs), my first idea of how Rust could manage having no garbage collector was that it must automatically handle all memory allocation. However, it seemed impossible to me that any language could not include an automated garbage collector, while also not making programmers deal with memory personally. That's pretty much how it actually works though though! One of the biggest features of Rust, to me, is the compiler, which has a highly developed static analyzer. A static analyzer for code will analyze code to see what it does and catch many errors, without actually compiling and running it.

Where am I going with this? Well, the static analysis means the compiler can insert calls to memory handlers, assuming the code is valid Rust. If it's not valid, and there's an error with it, then the code won't compile with a log of what's wrong. It might sound from a glance that it'd be faster and simpler to handle memory by hand, and assume that the code is correct, instead of trying to convince the compiler that it's good. But I think that 1) you have to program in languages like C for years to be that capable with handling memory, and 2) Rust's compiler is one of the the friendliest (so to speak) out there- it offers solutions for commonly identified issues, and most errors in my experience come from misunderstanding how Rust works.

#### References In Rust ####

It is that final point, trying to understand how Rust *works*, that I appreciate the compiler for. By removing the intensity of juggling memory allocation, while still offering low-level benefits like C has, lets me focus on learning Rust. A lot of thinking energy I would have otherwise spent on figuring out why my pointer allocations are causing memory errors has been used instead for getting a good grasp on what Rust is doing, and why. I'm very grateful for this, since it has helped me learn much better than before how references in Rust (and in general) work!

So, I've said a lot about *how* I've been learning Rust, but I haven't said much on what those references are actually doing, to reflect my knowledge. I'll do so now, although I might get something a bit wrong, so please don't take my word over what the [Rust documentation says](http://doc.rust-lang.org/book/references-and-borrowing.html).

#### Changes In Understanding ####

Previously, I first got acquainted with references and pointers (a variable that holds a reference) by way of C (just like enums, above!). My grappling with the complexity of programming with pointers led to my understanding being summed up as "a pointer 'points' to another value somewhere else in memory". That's certainly a workable definition, but since I have a hard time using a concept I can't explain to myself, I was held back in trying to write working code that uses pointers. As I've described above, it's working with Rust that has given me more space to learn.

I would now define a reference declaration as "a variable whose value is the memory address offset of another value"; that is to say, a pointer is a small value that holds a reference to another value that might be of any size in memory. The benefit of this is being able to pass around or copy the pointer much more efficiently than what is being pointed to. It's important to understand the difference between the pointer itself (a position in memory) and what it points to (which could be anything, including another pointer).

In Rust, pointers are frequently used for anything more complicated than a number or a boolean. Knowing what's going on when pointers are created or passed around makes it much easier to both right valid code the first time around, and also to figure out pointer errors. Rust has two simple ways to interact with pointers: with `&`, `let y = &x` makes `y` a reference to `x`; and with `*`, `*y` gives the value that `y` is pointing to, which is `x`.

When I get an error regarding references, I used to approach this by cycling through configurations of `&` and `*` on the variables raising problems until the compiler was happy, but this was an often frustrating and time consuming experience. By slowly better learning how Rust is handling references, I've become able to trace through the code myself to check my own logic, and be able to figure out the correct answer!

Fifth post concluded.