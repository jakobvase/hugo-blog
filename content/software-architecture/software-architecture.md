---
title: Software architecture
date: 2023-08-07
---

> This is still a work in progress.

I've been building software for 11 years now, counting from when I started at
uni. I've been in lead positions for 6 of those years, because I can't keep my
mouth shut when I see something stupid. I've worked for 5 years on a gigantic,
20+ years old system, and I've started a bunch of new ones, seen them grow, and
many of them fail.

This is my list of things you should do when building software. I'm writing it
for myself, so I can look up why I'm doing things I'm doing, and for people I
work with, so they can see why I do the things I do.

## Most important things

1. Use the best tool for the job
   - Are you building a blog? Probably use wordpress.
   - Are you building a webshop? Probably use shopify.
   - Are you building an Instagram or a Facebook? Probably use aws.
   - Are you building something that's only supposed to work for 6 months? Use
     whatever you're already good at.
   - Are you building something for fun? Do whatever you want and have fun!
   - Are you building something where security or correctness is critical? Use a
     high-level, typed programming language and proven, well-known frameworks.
   - Are you building something that's performance critical? Use Rust or C, or
     even assembler if it's that critical.
   - Are you prototyping? Don't spend too much time picking the right tools,
     just build. Untyped or loosely typed languages work best here. Don't write
     tests.
2. Insist on talking to the customers
   - Your boss doesn't know what the customers want. Your product owner doesn't
     know what they want. You need to meet and talk with them and understand
     them, or you're going to be building what your boss or product owner wants,
     not what the customer wants.
3. Work in increments
   - You will always build the wrong thing first. Get a tiny thing working, show
     it to the customer, and iterate on it.
   - Your increments must be as small as absolutely possible. Can you get it up
     and running in a few hours? Great! Build it and show them. Don't let an
     iteration take more than a few days.
   - If you need to work on something the customer won't see, tell them that
     they'll hear from you again when you're done.

## Good architecture for security and correctness

I've spent most of my time in security and correctness-critical systems, where
performance and design take the back seat, and where the customers are usually
willing to wait for the correct thing, rather than a quick thing.

For that kind of system, it's important to optimize for code readability and
testability.

- The code will be read thousands times more than it will be written. Spend an
  extra minute making it more legible.
- Make it as easy as possible to test the system.
- Test the most important things end-to-end. Write tests that act as the user
  would, and asserts what the user would.

  ```
  login();
  navigateToAccount();

  ```
