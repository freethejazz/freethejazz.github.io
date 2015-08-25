---
layout: post
title: "Who nailed the principles of great UI design? Microsoft, that's who"
date: 2013-05-02
---

One of the best articles I've ever read on user interface design is [this
12-year-old classic](http://msdn.microsoft.com/en-us/library/ms997506.aspx) --
written by Microsoft, no less. Published long before smartphones and modern
tablets emerged, it fully explains the essence of good UI design. Amazingly, it
criticizes Microsoft's own UIs and explains why they are bad, though it was
written at a time when Microsoft was not known for its humility.

Because my company has a mobile application division -- and increasingly does
full application development in our enterprise open source division -- I often
have to explain what makes a good or bad UI to customers. I've frequently
referred to this article by way of explanation.

See, Microsoft decided it wanted to solve a big problem with software: It's
hard to use. So it conducted research on how its customers use and learn
software and guessed the reasons why it was so hard for users. Microsoft
realized users weren't understanding the conceptual model of the product,
weren't learning common tasks after repetition, and found it hard to figure out
what each screen was supposed to do. Deductive user interfaces, as Microsoft
called them, were the culprit.

### The deductive user interface

The basic concept of a deductive user interface is that a user is presented
with a pile of possible actions on a single screen. Then the user has to go
through bit by bit figuring out (or deducing) what does what, what's related,
and what actions can be taken. Power users might breeze through this
introductory period and get to work because that sort of exploration is
comfortable for them. Unfortunately, the majority of your user base doesn't
fall in that category. A deductive interface is just a pain for the average
user.

Two great examples of deductive interfaces come immediately to mind. The first,
given that tax season just ended, is the almighty IRS 1040 U.S. Individual
Income Tax Return form. This form has instructions, but in general, you have to
figure out what to do and what the terms mean. The form comes close to assuming
you are an accountant familiar with tax law. This obscurity has created a whole
industry of tax preparation experts who not only know the form but everything
behind it.

<figure>
  <img src="{{ '/assets/img/irs-1040.png' | prepend: site.baseurl }}" alt="IRS 1040 tax form. Ugh.">
  <figcaption>Look depressingly familiar? It's page 2 of the classic deductive UI, the IRS 1040 tax form, ready to stymie you like it does every year.</figcaption>
</figure>

The second example is Windows Explorer. When Microsoft originally came up with
this UI motif in Windows 95, it suggested everyone use it. There were controls
for Visual Basic that helped you apply the Explorer interface to your custom
applications.

Windows Explorer is almost intuitive if you've grown up in the paper world. You
have data, which you think of as documents that go in folders -- except folders
and documents are a sort of deductive user interface in themselves. You had to
know how the filing system worked, what to find, and what to do with what you
found. You see a bunch of stuff, but what can you do with it? Right-click it
and find out. How did you know to right-click, let alone alter folder or
filename attributes? Someone showed you or you took the tutorial at least once.

A user interface shouldn't leave users with questions like "what am I supposed
to do?" or "what is this icon for?" or "why/how did I get here?" The UI should
answer those questions and guide users through the process.

### The inductive user interface

Microsoft coined the term IUI (inductive user interface) to describe a type of
interface that solved the problems described above. It fit Web design
naturally, years before AJAX as we know it: A workflow was broken out into
separate pages, each with the sole purpose of completing a simple, well-defined
task. After each task was completed, information would be sent to the server
and the next page would be loaded. The one-to-one ratio between a task and a
screen became the cornerstone of IUI.

Microsoft's guidelines document spells out specific steps to take when
designing using IUI, but the core concepts can be distilled down to a single
idea: Each screen should have a single, clear purpose, with everything on the
screen supporting that purpose -- including the title of the screen. The only
items allowed on the screen that don't directly support the purpose are closely
related tasks. This exercise in simplicity is actually difficult, but worth it.
Simplicity means happy users.

<figure>
  <img src="{{ '/assets/img/iui-turbotax.png' | prepend: site.baseurl }}" alt="IUI form TurboTax">
  <figcaption>Hey, that's better. The reason for TurboTax's popularity is that it parses the income tax filing process into discrete, understandable tasks.</figcaption>
</figure>

If the 1040 tax form is a deductive UI, then TurboTax is an IUI. TurboTax
breaks down the tax filing workflow into small, very clear tasks. Each task has
a dedicated screen that is clearly labeled, with a short description of what
information is needed and why, and often asks the user a direct question.
Instead of forcing the user to jump around worksheets and add line 18b on page
2 to line 42c on page 3, the user is guided through the process in a logical,
linear way. User interfaces that embrace these guidelines are easy and
enjoyable to use, even for doing something like filing your taxes.

### That was then...  A decade or so is an eternity when it comes to
technology. So how pertinent is IUI today? The limitations of the Internet 12
years ago were informing Web design decisions, and those decisions drew heavily
upon the concepts of IUI. IUI fit naturally with slow Internet connections.

The Internet isn't slow anymore, and single-page Web applications are springing
up left and right thanks to advances in browser technology and the widespread
adoption of JavaScript for more than just frilly enhancements. Now that the
limitations have dissolved, is IUI even worth practicing anymore?

Absolutely. For those developing desktop software, IUI is just as pertinent as
it was when the article was released. For Web developers, IUI is crucial to
remember. Now that Web technology has progressed so much, the distinction
between "it can happen" and "it should happen" can easily be forgotten.

IUI is particularly relevant for mobile developers because real estate is
precious on smaller screens. The purpose of each screen on a mobile device
needs to be very concise -- clearly labeled, and often inline with a single
navigation button (if needed) and a maximum of one supporting action button.


<figure>
  <img src="{{ '/assets/img/iui-1.jpg' | prepend: site.baseurl }}" alt="Buttons on a mobile header">
  <figcaption>More actions can be placed at the bottom within a button bar, or available just off screen through a hamburger button.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/iui-2.jpg' | prepend: site.baseurl }}" alt="Buttons on a mobile header">
</figure>

<figure>
  <img src="{{ '/assets/img/iui-3.jpg' | prepend: site.baseurl }}" alt="Buttons on a mobile header">
  <figcaption>A well-designed screen should not require scrolling except in specific cases, such as the necessary display of a long list of related details.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/img/iui-4.jpg' | prepend: site.baseurl }}" alt="Extra settings hidden behind a hamburger button">
</figure>

Although IUI is still pertinent today, it's not a complete solution. It
provides a great foundation for design, but problems arise when you want to
give users more power. Take Photoshop as an example of one of the most
deductive interfaces in widespread use. It's filled with toolbars that contain
countless unlabeled icons, and it's notoriously arduous to learn. Users of
Photoshop have accepted interface struggles in return for eventual speed and
power.

What if the learning process were smoother? A screen-by-screen interface
walking a user through customizing a color gradient would be great for
beginning users. That same "gradient wizard" would just get in the way of that
same user once the workflow had been mastered. The ultimate solution is a user
interface that adapts, gauging user experience and adjusting the interface and
user experience accordingly. Some preliminary attempts at this, such as
[progressive
reduction](http://layervault.tumblr.com/post/42361566927/progressive-reduction),
will help to advance our understanding of how to measure user experience (and
experience decay) and how to adapt our interfaces to account for it.

Regardless of whether IUI is right for the current project, your whole team
should be familiar with it. It embodies basic UX principles that help everyone
from project managers to developers make better decisions. Even though
"Microsoft Inductive User Interface Guidelines" was published more than a
decade ago, spend the time to go through it. Understand the problems identified
and the approaches to solve those problems. Your users will thank you.

_This was originally published at [InfoWorld.com](http://www.infoworld.com/article/2614315/application-development/who-nailed-the-principles-of-great-ui-design--microsoft--that-s-who.html)._
