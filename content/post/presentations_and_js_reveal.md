+++
title = "Presentations and the delight of js-reveal"
author = ["Alasdair McAndrew"]
date = 2018-03-11
draft = false
+++

Presentations are a modern bugbear. Anybody in academia or business, or
any professional field really, will have sat through untold hours of
presentations. And almost all of them are _terrible_. Wordy,
uninteresting, too many "transition effects", low information content,
you know as well as I do.

Pretty much every speaker reads the words on their slides, as though the
audience were illiterate. I went to a talk once which consisted of 60 --
yes, sixty -- slides of very dense text, and the presenter read through
each one. I think people were gnawing their own limbs off out of sheer
boredom by the end.
[Andy Warhol's
"Empire"](https://en.wikipedia.org/wiki/Empire%5F(1964%5Ffilm)) would have been a welcome relief.

Since most of my talks are technical and full of mathematics, I have
naturally gravitated to the LaTeX presentation tool
[Beamer](https://en.wikipedia.org/wiki/Beamer%5F(LaTeX)). Now Beamer is
a lovely thing for LaTeX: as part of the LaTeX ecosystem you get all of
LaTeX loveliness along with elegant slide layouts, transitions, etc. My
only issue with Beamer (and this is not a new observation by any means),
is that all Beamer presentations have a certain sameness to them. I
suspect that this is because most Beamer users are mathematicians, who
are rightly more interested in co[[<https://orgmode.org>][]]ntent than
appearance. It is quite possible of course to make Beamer look like
something new and different, but hardly anybody does.

However, I am not a mathematician, I am a mathematics educator, and I do
like my presentations to look good, and if possible to stand out a
little. I also have a minor issue in that I use Linux on my laptop,
which sometimes means my computer won't talk to an external projector
system. Or my USB thumb drive won't be recognized by the computer I'll
be using, and so on. One way round all this is to use an online system;
maybe one which can be displayed in a browser, and which can be placed
on a web server somewhere. There are of course plenty of such tools, and
I have had a brief dalliance with [prezi](https://prezi.com), but for
me prezi was not the answer: yes it was fun and provided a new paradigm
for organizing slides, but really, when you took the whizz-bang aspect
out, what was left? The few prezis I've seen in the wild showed that you
can be as dull with prezi as with any other software. Also, at the time
it didn't support mathematics.

In fact I have an abiding distrust of the whole concept of
"presentations". Most are a colossal waste of time -- people can read so
there's no need for wordiness, and most of the graphs and charts that
make up the rest of most slides are dreary and lacklustre. Hardly
anybody knows how to present information graphically in a way that
really grabs people's attention. It's lazy and insulting to your
audience to simply copy a chart from your spreadsheet and assume they'll
be delighted by it. Then you have the large class of people who fill
their blank spaces with cute cartoons and clip art. This sort of thing
annoys me probably more than it should -- when I'm in an audience I
don't want to be entertained with cute irrelevant additions, I want to
_learn_. This comes to the heart of presenting. A presenter is acting as
a teacher; the audience the learners. So presenting should be about
engaging the audience. What's in your slides comes a distant second. I
don't want new technology with clever animations and transitions,
bookmarks, non-linear slide shows; I want presenters to be themselves
interesting. (As an aside, some of the very worst presentations have
been at education conferences.)

For a superb example of attention-grabbing graphics, check out the
[TED
talk](https://www.ted.com/talks/hans%5Frosling%5Fshows%5Fthe%5Fbest%5Fstats%5Fyou%5Fve%5Fever%5Fseen) by the late [Hans
Rosling](https://en.wikipedia.org/wiki/Hans%5FRosling). Or you can admire the work of
[David McCandless](https://informationisbeautiful.net).

I seem to have digressed, from talking about presentation software to
banging on about the awfulness of presentations generally. So, back to
the topic.

For a recent conference I determined to do just that: use an online
presentation tool, and I chose [reveal.js](https://revealjs.com/#/). I
reckon reveal.js is presentations done right: elegant, customizable,
making the best use of html for content and css for design; and with
nicely chosen defaults so that even if you just put a few words on your
slides the result will still look good. Even better, you can take your
final slides and put them up on [github
pages](https://pages.github.com) so that you can access them from anywhere in the world with a
web browser. And if you're going somewhere which is not networked, you
can always take your slides on some sort of portable media. And it has
access to almost all of LaTeX via [MathJax](https://www.mathjax.org).

One minor problem with reveal.js is that the slides are built up with
raw html code, and so can be somewhat verbose and hard to read (at least
for me). However, there is a companion software for emacs org mode
called [org-reveal](https://github.com/yjwen/org-reveal), which
enables you to structure your reveal.js presentation as an org file.
This is presentation heaven. The org file gives you structure, and
reveal.js gives you a lovely presentation.

To make it available, you upload all your presentations to github.pages,
and you can present from anywhere in the world with an internet
connection! You can see an example of one of my short presentations at

<https://amca01.github.io/ATCM%5Ftalks/lindenmayer.html>

Of course the presentation (the software and what you do with it), is in
fact the least part of your talk. By far the most important part is the
presenter. The best software in the world won't overcome a boring
speaker who can't engage an audience.

I like my presentations to be simple and effect-free; I don't want the
audience to be distracted from my leaping and capering about.
Just to see how it works

[//]: # "Exported with love from a post written in Org mode"
[//]: # "- https://github.com/kaushalmodi/ox-hugo"
