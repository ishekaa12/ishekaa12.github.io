---
layout: post
title: "Lied Again: I Did Not Stay Consistent"
date: 2026-08-19
categories: [life, dsa, c, java]
---

Remember when I said DSA was eating my brain? It's been eating my brain for a while now. Just not always in a row.

## The Drop Year and the Big Decision

I took a drop year. Which meant I had a lot of time on my hands and a lot of internet to scroll. And everything I read said the same thing: CSE major means DSA. Good engineer means strong DSA. That was it. That was the whole career plan according to the internet.

So I told myself I'd start in 1st sem itself. No drama, no waiting, no "I'll do it later." Just start.

## The Girl Who Codes in the Hallway

I started with Java. Learned about JVM, JRE, JDK, all of it. Don't remember any of it now, lol. Then I found Kunal Kushwaha's videos and just followed along. Solved questions in sem 1. I was way too motivated. Everyone in the hostel knew me as the girl who codes in the hallway. That was my whole identity for a while.

## Then February Happened

I was consistent until February 2026. Into 2nd sem. Then something happened — not going into it here, but it knocked me sideways more than I expected. DSA didn't stand a chance after that. I just stopped.

## Coming Back

Now it's 3rd sem. It's been about 2 months since I got back to it. Consistent again. What have I covered? Loops, functions, searching, a few sorting techniques, pattern questions, basic maths, recursion and its types, strings, StringBuilder, ArrayList. Still not 100% clear on all of it, lol. But working at it.

Consistent this time means something more concrete too — I've been tracking every problem I solve with dates and attempt counts, and sorting them into pattern clusters (sliding window, binary search, two pointers) instead of just grinding a random problem list. 57 problems in so far, mostly Easy, with Strings and DP flagged as the gaps I still need to close.

## The C Twist

Here's the thing nobody told me: college teaches DSA in C, not Java. And honestly? It's been the best part. Java hides how things actually work. C shows you everything. You see how the data structure is built, not just how to use it.

The catch is I solve everything in Java first — that's where Kunal's course lives, that's what I'm fluent in — and then I have to re-derive the same logic in C for college. It's basically learning DSA twice, and the gap between the two isn't small. Java gives you `ArrayList` and garbage collection and pretends pointers don't exist. C makes you build the thing yourself and then clean up after it yourself.

Linked lists are where that gap showed up hardest. In Java, you just call `.add()` and move on. In C, I had to actually sit with *why* a function that modifies a linked list needs a pointer to a pointer — not just `Node *head`, but `Node **head` — because if you only pass `head` by value, any change you make inside the function disappears the moment it returns. That's not a syntax detail, that's the whole mental model of "who owns this pointer" clicking into place. Once it clicked, I could finally see what Java's `ArrayList` had been hiding from me the entire time.

Right now we're at linked lists in college. The algorithms are overwhelming. But we'll get through.

## Where I'm At

Not an expert. Not even close. Just further than I was in February. That's it. That's the post.

Next up: probably a real linked-list-in-C post, since that double-pointer thing deserves more than one paragraph.
