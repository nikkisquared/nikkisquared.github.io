---
layout: post
title: "Okay, But What Does Your Work Actually Mean, Nikki? Part 1: Starting On Servo"
date: 2016-01-03T18:24:31-07:00
---

In my first post, I tried talking about what I'm working on, but ended up talking about everything that lead to my internship, starting about a year ago! That's pretty cool, especially since it was the only way I could think of talking about it, but it was kind of misleading. I had a couple short paragraphs about Servo and Fetch, respectively, but I didn't feel up to expanding on either since my post was already so long. So, I'm going to be trying that now! Today I'll start writing about what I've done for Servo.

## Servo At A Broad Level ##

There's so much to Servo, I don't assume I could adequately talk about it at a broad level beyond quoting what I said two weeks ago: "Servo is a web rendering engine, which means it handles loading web pages and other specifications for using the web. To contrast Servo to a full-fledged browser: a browser could use Servo for its web rendering, and then put a user interface on top of it, things like bookmarks, tabs, a url bar."

What I do feel more confident talking about is the parts of Servo I've worked with. That's in three areas: my first contribution to Servo as part of my Outreachy application (and its follow-up), working on the Fetch implementation in Servo, and changes I've made to parts of Servo that Fetch relies on. Today I'm going to talk about the first part, and the other two together in the next post.

## My First Contribution ##

Like I briefly mentioned in my first post, I first got acquainted with Servo by [adding more functionality](https://github.com/servo/servo/issues/8213) for existing code dealing with sending data over a WebSocket, which is a well defined web communication protocol. My task was to understand a [small part of the specification](https://html.spec.whatwg.org/multipage/comms.html#dom-websocket-send) ("If the argument is a Blob object"), and the existing implementation for "If the argument is a string". A Blob can represent things like images or audio, but the definition is pretty open-ended, although most of what I had to do with it was pass it on to other functions that would handle it for me.

As with everything I've done in Servo, what first seemed like a simple goal - make a new function similar to the existing code that just handles a different kind of data - ended up becoming pretty involved, and needing changes in other areas that I didn't have the time to fully comprehend. A big part of this is that working on Servo is my first time using the programming language [Rust](https://www.rust-lang.org/), so I was learning both Rust and the WebSocket specification at the same time! Thankfully, I had a lot of help from other members of the Servo team on anything I didn't understand, which was a lot.

Outside of slowly learning how to work with Rust, I spent most of my time on this talking about exactly *how* I should be implementing anything. I like to make sure I do the right thing, and the best way for me to do that is to understand the situation as well as I can, then present that and any questions I have to other people. 

## The Buffer Amount Dilemma ##

One instance of this was that the specification for WebSocket didn't say what to do if the size of a Blob being sent is larger than the maximum size a WebSocket buffer (where data is temporarily stored while being passed on) can hold. A Blob can hold up to 2^64 bits of data, whereas the WebSocket buffer could only hold 2^32 bits of data. The "obvious" solution would be to handle it like I would if the buffer overflowed by being fed many Blobs smaller than 2^32 bits- but that seemed to me a different situation, and so I asked around for advice on what to do.

The consensus was that for now I should 1) truncate the data to 2^32 bits, and then raise an error in response to the buffer overflow 2) open an issue at [WHATWG](https://wiki.whatwg.org/wiki/FAQ#What_is_the_WHATWG.3F) (the authority behind the current HTML Standard) about the seeming gap in the specification about this, to find out what part of the specification, if any, needs updating.

And so, in approaching everything I was unsure about in such a way, I slowly made progress on ironing out a decent function that covered previous and new needs, follows the specification as much as possible, and has reasonable solutions to areas either not covered by the specification or are just unique challenges to programming in Rust.

## Return Of The Buffers ##

Since all of this was meant to be part of my application to Outreachy, after it was accepted for Servo, I stopped thinking about it while waiting to hear back on my acceptance status. Between then and my being chosen as the intern for Servo, that [issue I had opened](https://github.com/whatwg/html/issues/296) at WHATWG had been discussed and resolved, with the decision being to let the WebSocket buffer hold up to 2^64 bits of data, meaning that there would be no need to intentionally lose any amount of data sent at one time, and preventing (incredibly) large files from instantly raising errors.

This change also meant that the code I had wrote earlier would need to be updated, since it was now incorrect! It made the most sense for me to do that, since it was my code, and my question that had changed the specification. I was grateful to be able to rectify a situation that had before seemed to have no good answer, especially since it also made my code simpler.

So, starting and ending over a period of a couple months, that's the story of the first work I ever did on Servo!

End of second post.