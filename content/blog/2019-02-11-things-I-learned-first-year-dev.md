---
title: "Things I learned during my first year as a Junior Developer"
date: 2019-02-11T19:42:41+02:00
description: "experiences"
keywords: [junior]
draft: false
tags: [meta]
math: false
toc: false
---

It's been an interesting 2018: it was the first year I have coded full-time while getting also paid for it.
Before that, I worked mainly as internal IT support for a couple different companies, where my coding skills weren't being tested as much as right now.

I won't lie, the year wasn't always easy. I am surrounded by some brilliant people and I have very often felt some sort of inferiority complex that brought me down many a times; I thought that my experience (or lack thereof) was slowing down the team or not allowing us to achieve the goals we were supposed to achieve.

Again though, I work with brilliant people! They have helped me out immensely and brought me to the point where I am today;

Of course, I do not feel I have a firm grasp of whatever is going on,

![dog picture](https://i.imgur.com/gjSgBW6.jpg)

But I do feel like maybe I can understand and figure out things with a little bit more confidence than before.

The following are a few observations and reflections, on some of the things I have learned/am learning, and have thought about in this time.

### 1. The more I know, the more I know how much I don't know

Cliche'? sure. But it is very, very true.
The best devs in my team are just as clueless as I am when facing a new problem, they are just more experienced at figuring it all out.

Noone knows everything by heart.
I (and many others i'm sure) still need to google how to create symlinks or tarballs from time to time.

### 2. Master the fundamentals

Pick your programming language of choice, and drill those basic concepts into your brain like there is no tomorrow.
Write notes, collection of blog posts, your own wiki, or even your own notebook.
Do stupid challenges in your downtime, like finding new ways to write [FizzBuzz](https://www.tomdalling.com/blog/software-design/fizzbuzz-in-too-much-detail/), or going through examples, exercises.

Like a carpenter and his tools, you want to get to a point where these basics are so ingrained in your head that they just come naturally to you.

You want to come to a point where you know the internals of python lists or dicts inside and out, and where you would use them and why.

And like the carpenter, you want to keep these skills honed and ready all the time.

### 3. Pair programming is awesome

The concept of pair programming was pretty alien to me at first. I didn't need to do this in my previous roles, or maybe not quite how it is done in the software development world.
Also some social anxiety/awkardness prevented me from embracing this practice fully, at least in the beginning.

The gist of it is this: you and a fellow developer sit down at one computer, and you hack together on a problem, thinking out loud and discussing ways to solve it; you take turns "driving", or using the keyboard.

This can be done face to face but also remotely, with the correct tools; for example, [ngrok](https://ngrok.com/), [tmux](https://hackernoon.com/a-gentle-introduction-to-tmux-8d784c404340), and any voice chat app.

Pairing with more experienced developers will allow you to pick their brain and "leech" off of their workflow, learning patterns, details, things to look at, tools.

Pairing with less experienced devs is also highly beneficial; their fresh mindset will see things that maybe you don't. And there is no better way to learn something than by explaining it to someone else.

Overall pair programming is a win for everyone, and you should do more of it!

### 4. The "Sherlock Holmes" factor

I really have no idea how else to call this.

So you are sitting next to a senior developer, and you explain to them a problem you have been having.
Immediately they go to the part of the code where presumably the problem is happening, and start following the code through imports, checking out inherited properties, overridden attributes, digging and digging to other parts of the code you didn't even know existed.

Their environment is setup in such a way where it is easy for them to go-to the definitions of any part of the code, being in the same codebase, in third party modules, or in the standard library; they manage to go through to the root of the issue and from there suggest the best way(s) forward.

The first time I saw this being done it blew my mind, although admittedly, the developer in question was doing this so fast that I could barely keep up with his train of thought.

In any case, they didn't have the solution to my problem straight off the top of their head, but they had the experience, the mindset and the tools to figure it out there and then.

This skill is one of the most important I have seen and what defines to me a great developer from a good one; It is also one I have been trying to incorporate into my workflow more and more.

This also brings me to the next couple points:

### 5. Read the docs

Don't skim through the examples and be like, "I got this, let me hack on it, I'll figure it out eventually".

You will spend the next 6 hours getting nowhere, just to, as a last resort, more attentively read that line in the docs you skimmed through.... and find there the answer to your question (true story).

These days documentation for your programming language, framework, API or what have you, is really well written. And if it's not, maybe you can be a good open source citizen and help out by submitting your own PR with better docs :)

Luckily you can do something about having docs for whatever language you are using, easily accessible to you and perhaps hooked up to your IDE.

As an example, I make heavy use of the excellent [Dash](https://kapeli.com/dash);
I can choose which docsets to have available, and anytime I can highlight a word, press a shortcut and it will take me to the docs for that particular term.

But yeah, just read the effin' docs :)

### 6. Setup an efficient dev environment

As described above, a good developer has easy access to docs, definitions, most times at the click of a button or shortcut.
Code snippets, auto-completion, goto definitions are all features you want to have easy access to.
These can be summarised in a good IDE.

There are many of these out there, both paid and open-source; you should pick one and stick with it for a month or two, learn it inside and out to make your workflow as fast and efficient as possible.

Other things you could do is setup keyboard shortcuts for your git flow, write aliases for those long commands you are typing into your terminal 50 times per day. Anything to make your development environment an extension of your brain.

### 7. Have side projects

Well, the best way to get better at coding is reading and writing code;
So then, build something cool!
Document it, share it, break it and fix it again.

Even if it is a very old project, make it continuosly better.
You can write tests for it, adapt it to the latest version of your preferred programming
language, or maybe write it for other programming languages.

There's the carpenter analogy in here again, you need to keep your tools honed and sharpened.
The best is to do so doing something you really enjoy :)

### 8. Keep up with what's going on

You need to be in the know of what is going on, both in your team, and in general.
Don't close yourself in your bubble; it might seem like your cozy and safe space but it is actually very limiting for your own progress.

Get your company to send you to some of those fancy conferences; once there, take notes, talk to people, build your network.
If you can't go to conferences for whatever constraint, very very often conference talks are available online for you to watch in the following days.
Really take the time to sit through some of those and absorb ideas, concepts and news.

Listen to podcasts, have a twitter feed, or check out hacker news or ars technica or something.
The tech world is changing all the time, and you need to keep up to date or be left behind.

### 9. Ask questions

No questions are dumb, just ask them;
At the same time however, you need to know to ask the *right* questions.

What does that even mean?
Well, if there is something you don't know, you should first research the matter on your own, make sure you read the fucking docs (see point 5), and have a clear idea of what it is you are trying to do.

Then and only then you should ask.
So ask questions, but also don't waste yours or anyone else's time.

### 10. Share the knowledge

Well, I have, this year, attended several events, where knowledge was shared:
Local meetups, conferences, webinars, you name it.

I am astounded by the quality talks some people can give on topics they are passionate about!

Giving talks and writing blog posts is a great way to learn more about a certain topic.
Not only that, they give you exposure and may be your gateway to find the next step in your career.

In some cases also, you end up helping out someone in need, sometimes months after you published your talk/blog post.

### Conclusion

I have the feeling I have barely scratched the surface, but feel like I had a very productive first year. I plan to continue on this course, and hopefully add to this article at the end of my second year as a software developer. :)

![confused](https://i.imgur.com/Xwp41UT.png)
