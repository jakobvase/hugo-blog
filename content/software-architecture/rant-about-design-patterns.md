---
title: Rant about design patterns
date: 2023-08-11
---

There are tonnes of principles in software development.

Object Oriented Programming (OOP), Functional Programming (FP), Don't Repeat
Yourself (DRY), Single Responsibility, Low Coupling, High Cohesion,
(collectively, SOLID), Pragmatic Programming, Code First, Api First, Security
First, Mobile First, Performance First, Simplicity First, Write for Readability,
Fix Performance When You Have To, Think First, Iterate, The Rule of Three,
Domain Driven Design, and so on.

What I have found is, they all have conditions under which they apply, and
determining whether those conditions apply to my current situation is often
harder than applying the principle.

## Conditions

Take the Don't Repeat Yourself (DRY) principle. Intuitively, this seems like a
good idea. If a piece of logic or an algorithm only exists in one place, there
is only one place you need write tests for, and any bugs you fix will
automatically apply to all users.

When does this principle make sense to use? When does it not? If I have two
pieces of logic that are _almost_ identical, should I generalize and pull them
out into a single place? What if performance is critical? What if memory is a
constraint? What if I need to use the logic in two different modules of my
codebase?
