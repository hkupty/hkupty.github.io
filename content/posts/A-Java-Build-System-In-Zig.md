---
title: "I've been bitten by the zig bug"
slug: a-java-build-system-in-zig
date: 2025-08-14T13:34:40+02:00
tags: [code, tech, java, zig]
author: Henry John Kupty
---

I foresee different responses for this post. Hypothetical, of course, because no one ever reaches back (am I losing my mental faculties? Talking alone in a blog that no one ever reads as if someone was going to have a rational or, even worse, emotional reaction to a text they're not going to ever read? Oh well).

One would be of joy and relief that something is being done. Another reaction would be of anger and frustration that it is not done the way they wanted. Well, it is pretty obvious to me which Hypothetical responses I should listen to: those that ~will suggest I visit the psychiatrist~ praise the act of doing.

Now, I'm not doing this - this what? Stop being vague - out of sheer joy of voluntarism and the greater good. It's actually because I was bit by the zig bug - and I really liked it.

---

## Deciding to build the build system

A quick glimpse into how my mind works - and likely yours too, before you fall out of interest: Whenever we have a suboptimal experience with a tool, we're presented with a choice. We can settle for it, or try something better. But when we run out of options - either because none are good enough or because none really fit our ways - we're back at settling. **Or**, because we know how to write software, we can... Write our own!

I know, I know, controversial. Let's trade thousands of collectively employed hours of code reading and writing and reviewing and testing for a pile of virtual stones poorly arranged in a way that kind of looks like we built something. Seems like a good deal to me.

Why am I saying this? I've never been the kind of person that liked to settle. I've worked with python, java, clojure, scala, kotlin, go, and some other innominable tech - I have at least an eyelash of self respect. Learning new languages exercises our creativity, expands our problem-solving skills and can even positively impact how we write code in other languages. Win-Win in my books, even though I'm obviously biased.

And the new thing to be tried was zig. First because I always felt I was lacking on the low-level languages side. And second because I wanted to.

For those unaware of zig, it is a modern low-level language. It's built on the premise of no hidden control flow or hidden memory allocation, which are things I do enjoy a lot. The simpler to read, the better. Less cognitive load is fundamental for longer term happiness.

Now, finding the project is always a challenge. I hold myself to high standards of non-conformity so there's always something I'm unhappy about. And my new favorite nemesis is gradle.

I've been using gradle, on and off, for the past decade. And I'm amazed by my inability to feel happy about it. And this is where I think java lacks a better alternative. Because maven is quite frankly not for me as well. And bazel, well, that's just binary self-hatred with a cli interface.

So, as simple as that. There's not much thinking necessary, a new potential project pops up so I can vent, reduce my frustrations and feel less unhappy with the status quo. The fact that no one will use it is a potential future unhappiness that future me will have to deal with. Right now, I'm more interested in getting rid of the stone in my shoe.

## Building the build system

I can already hear the ravaging crowd calling me a heretic for writing a build system for the glorious java language in a plebeian language like zig. Well, keep your problems to yourself. Perhaps write something about it. But remember that even the all-mighty JVM is not written in java, so there's something to think about.

As I hinted above, the simplicity of the language when reading is a key factor, but that in itself doesn't promote a language to being a good candidate. Zig is a compiled language that produces binaries for all major platforms and architectures, with great cold-start performance. As if that wasn't enough, it has great interop with c code. Small binaries, fast startups, great performance. And the cherry on top is that it doesn't have the bootstrap problem that a JVM language has for writing a JVM build system: it needs to be updated before it can support a newer JVM.

And I must tell you that, the experience of writing zig is, as you might have guessed (since otherwise I wouldn't be writing about it) fantastic. I know, I sound just like another sheep in the herd. Or maybe it is you that is a difficult-to-please reader. It is, nonetheless, true. There's something to compiled languages that, once the very strict compilation phase succeeds, presents you with a long lasting peace of mind that your software will work. Not that it will be free of bugs, just that it will work. And this is good.

It is also very rewarding to notice that, with experience, you battle less and less with the compiler. Instead, you befriend it.

And this gives you a nice dopamine rush at every little milestone, every small achievement, that are, neurologically speaking, very beneficial. Some of that is, of course, part of the language's developer experience - LSP, docs, compiler feedback, etc. Some of that, however, is more related to how we organize the project. Small chunks, measurable progress, and we build ourselves a structure so we can reap the benefits of small dopamine rushes.

If you don't trust me on that, try building a boring project on a language with a bad developer experience.

## The build system

Ok, walking past the motivational tech-coach speech, how is my idea so much superior that I deemed it worth materializing it?

I believe, naturally, that my idea is fantastic _to me_. You're 100% free to dislike it.

It is based on a timeless principle, rooted in ancient, forbidden knowledge: Simplicity.

All the existing _java_ build systems are java-esque in their architecture: Complex structure, plugin-based extensibility, etc.

They bring into the build system a mindset that is present in the language itself: Many layers of abstraction modelling the whole experience as a rigid structure with careful concessions to extensibility.

[Lift](https://github.com/hkupty/lift), as I decided to call it, goes on a different route: Your _Project_ has _Steps_. Steps can depend on each other.
There's a contract between them that says: The output of a step becomes the input for all steps that depend on it.
Data is shared in JSON between steps. Done.

This means that lift has to provide some steps for the very basic stuff like `sources` - so we can feed the files to the compiler - instead of modelling what is a source-set and how that belongs in a project and interact with the dependencies.

This level of simplicity yields the user greater extensibility and flexibility, at the expense of defaults. Something something uncle Ben's quote to Peter Parker here. We know the deal. Sometimes, we want great defaults. Sometimes we want full control. I think it's hard to achieve both, at least not without sacrificing something else. And I'm not making that trade-off at the expense of increasing cognitive load onto the user.

Lift is at its very early stages in development yet, but soon nearing POC quality. I'll keep you all - my faithful two readers - posted once things improve.

## Build

And this is where I close my thoughts, back at personifying the coach. Build, get things out of your system.

I have absolutely no way of knowing where this is going to lead me to. There's a big chance that this is not going to fly and the world will keep on using gradle & maven. There's a big chance I'll not use zig professionally for the next decade. There's a huge chance that AI will make economy collapse and the software engineering discipline apocalyptically and autophagically implode.

I don't care. I got to take this out of my system. I got to increase my level of satisfaction and my self-confidence for building something. I got to sharpen my skills, to increase my knowledge both in core and peripheral topics to this problem space. I got to keep the muscle of learning strong. I got happier.

Build.

:x

