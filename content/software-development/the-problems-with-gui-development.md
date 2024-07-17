---
title: The problems with GUI programming
date: 2016-01-28
---

Hey.

So I’m a developer, I do a lot of GUI, it sucks.

There are lots of problems with GUI programming. The worst ones include: The
complexity of the frameworks in use, how fiddly it is to get something to behave
the way you want it to, and the problem of separating GUI layout, GUI design,
GUI behaviour, model rules, and model data.

Let’s go through some of the frameworks I’ve worked with, how they tackle these
problems and what they could do better.

## WinForms

WinForms is the ‘old’ Microsoft GUI framework. Many people still swear by it
though, so it’s still actively updated and maintained.

Features:

- Complexity: It’s not a complex framework initially. It follows the well-known
  idea of OO programming to the end, which makes it pretty easy to understand.
  But when you dive in deeper, it’s terrible.
- Fiddlyness: It still communicates internally with Microsoft’s WIN32 api, which
  is outdated. That’s an understatement. But as long as you can live with not
  being able to do anything, it’s not hard to grasp.
- Separation: It doesn’t support any separation or binding. You have to take
  care of that yourself.

WinForms is not a complex framework – at least not initially. Most of the
day-to-day stuff programmers want to do in GUI is readily supported, and that’s
a huge help.

## WPF (Windows Presentation Foundation)

What a name. WPF is the new school (new as in 2007 new. Not new new), where
bigger, better ideas were had. I believe it is in fact more usable than WinForms
– but there’s a catch…

Features:

- Complexity: Terrifying. It’s hard to even get a program to start, and much
  worse to wrap your head around all the weird things you can and cannot do. And
  even though you push through and start to understand key concepts, there are
  so many and they are so strange.
- Fiddlyness: Thankfully, a lot better than WinForms. Once you have the text box
  where you want it, bound to the things you want it bound to, styled the way
  you want, it’s a lot easier to get the behaviour you want. Both thanks to
  Dependency Properties and to the horde of events suddenly thrown your way.
  There are lots of good ideas in WPF, which is probably what makes it so
  intimidating to start with.
- Separation: WPF lives and breathes the Microsoft ‘MVVM’ paradigm, where the
  View (what the user sees) automagically updates the ViewModel (a
  wrapper/translator/facilitator between View and Model) which then manipulates
  the Model (the model). They also separated layout and behaviour, but it’s not
  unproblematic. It’s very hard to use correctly, and the limitations between
  classes is hard to work with. You can’t put this kind of binding on that, you
  can’t have multiple bindings easily. You also have to learn two languages – at
  least one of which could have been html so easily. Why did you have to make
  your own goddamn xml-format?

## HTML, CSS, JavaScript

The holy trinity of web development. It’s not terrible.

Features:

- Complexity: Initially, not bad. By far the easiest of the ones mentioned here.
  There is the drawback of having to learn three languages and how they
  integrate – but they integrate quite well and there are MANY GOOD tutorials on
  this. And frameworks to help – but they add to the complexity… The one problem
  here is that no one holds your hand. How should your HTML look? What goes in
  CSS and what goes in HTML? What goes in JavaScript? Where should you use JS,
  and where should you not? What can you even do with CSS? What can’t you do?
  There are so many pitfalls.
- Fiddlyness: Pretty annoying. Floats are made by the devil, there are so many
  browsers to care for, how do I make my text box accept only numbers again? Am
  I sure that works in IE 9, 10, Edge, Chrome, Firefox, Mozilla, Opera, Safari?
  What about mobile phones? Maybe you’ll have to redo your fiddle to work with
  what you want, or you’ll have to just give up somewhere. But it’s not as bad
  as WinForms.
- Separation: Again, you’re on your own. The layout and design are neatly
  separated (if you know what you’re doing), but from there to the model, or
  getting behaviour right, it’s on your head to separate it.

## So what do we do?

I think part of the problem of GUI programming is that no one language easily
encompasses all we want to do. Web development has the right idea with so neatly
separating layout and design, but JavaScript is not the best language to write
big production code in. The data binding of WPF is really interesting, but has
other problems – I have to put my validation logic in the GUI, or let my model
get into bad states while people go crazy in the GUI, and then fix it when they
leave me alone. Somehow, validation belongs in the model. The GUI should only
care whether something is valid or not, not what it is that makes it valid.

Another problem with both WPF and HTML (but not so much with WinForms) is the
prevalence of ‘magic strings’ – string names for things that has some meaning
elsewhere in the application. These are the things that break when code is
updated. How should I know that this string was used over there for that
purpose?

I think we need a complete package. Where GUI is taken as seriously as the
deeper model, and where the communication between GUI and model is easily
facilitated. This is not easy.

Layout – HTML has proven to be a great way to layout and structure an interface
– but not so much for binding data to a model behind it. And it has the problem
of magic strings. If these two problems could be eliminated, I believe HTML
could be one cornerstone of every GUI in the future.

Design – CSS has problems – but it also does things right. I believe the CSS
people (over at Mozilla) are too focussed on it being purely the way it is. I
believe it would benefit greatly from variables, from using XML-selectors
instead of the weird pseudo-selector language it uses now, and from a more clear
focus on what is applied where. I think a new language based very much on CSS
should be made, that could take design to the next level.

Behaviour – There is nothing I know of that tackles behaviour right. I think
behaviour should be able to be added much like CSS-design, with maybe a library
of standard behaviours that is all that most people want?

Data binding – WPF has the right idea with binding the text or variables of a
control with some underlying model behind it, but the separation between View
and Model hurts it some, and the problem of the Model not being able to apply
validation on it’s text hurts more. I believe it is time for a programming
language that allows for validation on variable-basis. If my Match class has a
Score field of int, I want to be able to set some limitations on that. It can’t
go below 0. It can only increment by 1 at a time (assuming soccer). Or whatever.
It’s max value is tied to the number of socks in my drawer. While I still
believe there should be a layer of separation between GUI and model (Eric Evans
calls it the Application layer), I believe the GUI should still be able to
update the model quite directly and vice versa. Maybe something like int
currentGoals = bind Match.Score. Yeah.

What about Client-Server architecture? What about CQRS? What about some other
stuff I don’t know? I don’t know yet. Haven’t got that far. I don’t think what
I’m thinking is impossible though. I think I’ll formalise a list for good GUI
frameworks later.

## Other stuff I’ve tried

I’ve made minor GUI stuff in Java’s Swing and Apple’s cocoa too, but I haven’t
dug deep enough to tell you how fiddly they are. Swing was pretty hard to
understand and pretty fiddly, but it was my first, so maybe I would just
understand it if I went back. Cocoa is C++, and I’m no shark there. Maybe the
perfect GUI-framework already exists somewhere, and I’m just not aware of it. Oh
the travesty.
