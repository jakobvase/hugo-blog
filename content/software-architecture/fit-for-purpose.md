---
title: Fit for purpose
date: 2023-08-08
---

> This is still a work in progress.

The most important rule in software development is to think. Build the thing
that best solves the problem I have in the environment I have. I often consider
this list of things, and I'll go in depth on them in the following sections.

- What is the purpose of the software?
- What is the longer term goal for the software?
- Who is going to maintain it, and how much time will they have?
- Am I part of a team? How big is the team?
- How much time do we have?
- How critical is the software? Can people die or lose work or just be slightly
  annoyed if it doesn't work?

## Example: This blog

This blog is a product like all other software. I'll respond to the above
questions here.

### What's the purpose of this blog?

To have a place where I can write down my thoughts on software development. To
have a place where I can discuss my thoughts on software development with the
world. To be able to point to my thoughts when I'm discussing at work the best
approach to something. It should be dead simple to write things down.

### What is the long term goal for the software?

In time, I hope to have a sort of library or book of best practices that I can
refer to. I hope that I can inspire some developers to consider how they write
software in new ways, hopefully leading to better software.

### Who is going to maintain it, and how much time will they have?

Me! And I don't have a lot of time, so it should be amazingly simple to
maintain. That means no cloud: in the cloud, you don't control when updates have
to happen. That also means really simple logic: if I have to code anything,
it'll be the simplest I can possibly make. And deployment should be a script
that I can maintain myself. No github actions, because those might change too.
Basically keep the number of third party integrations to an absolute minimum.

### Am I part of a team?

Nope, just me.

### How much time do we have?

Very little. Evenings and weekend mornings. So very little time to research
anything.

### How critical is the software?

Not critical. It will be at most annoying if it doesn't work. I want to make
sure that it is secure, so only I can update things. The comments section might
be overrun by bots, but then I'll work something out.
