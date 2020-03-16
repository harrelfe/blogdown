+++
title = "Integrating Audio, Video, and Discussion Boards with Course Notes"
date = 2017-08-01T10:12:00Z
updated = 2020-03-16
tags = ["collaboration", "teaching", "r", "reproducible", "2017"]
+++
As a biostatistics teacher I've spent a lot of time thinking about inverting
the classroom and adding multimedia content. My first thought was to
create YouTube videos corresponding to sections in my lecture notes.
This typically entails recording the computer screen while going through
slides, adding a voiceover. I realized that the maintenance of such
videos is difficult, and this also creates a barrier to adding new
content. In addition, the quality of the video image is lower than just
having the student use a pdf viewer on the original notes (**Plea for help**: does anyone know of a way to create html files with audio where slides are "self-advancing" and synchronization with audio is maintained?). For those
reasons I decided to create audio narration for the sections in the
notes to largely capture what I would say during a live lecture. The
audio `mp3` files are stored on a local server and are streamed on
demand when a study clicks on the audio icon in a section of the notes.
The audio recordings can also be downloaded one-at-a-time or in a batch. (**Note**: I have given in to mostly producing traditional YouTube videos but would still like to find a better approach).

The notes themselves are created using `LaTeX, R`, and `knitr` using a
`LaTeX` style I created that is a compromise format between projecting
slides and printing notes. In the future I will explore using `bookdown`
for creating content in `html` instead of `pdf`. In either case, the
notes can change significantly when R commands within them are
re-executed by `knitr` in `R`.

An example of a page of `pdf` notes with icons that link to audio or
video content is in Section 10.5 of
[BBR](https://hbiostat.org/doc/bbr.pdf). I add red letters
in the right margin for each subsection in the text, and occasionally
call out these letters in the audio so that the student will know where
I am.

There are several student activities for which the course would benefit
by recording information. Two of them are students pooling notes taken
during class sessions, and questions and answers between sessions. The
former might be handled by simultaneous editing or wiki curation on the
cloud, and I haven't thought very much about how to link this with the
course notes to in effect expand the notes for the next class of
students. Let's consider the Q&A aspect. It would be advantageous for
questions and answers to "grow", and for future students to take
advantage of the Q&As from past students. Being able to be looking at a
subsection in the course notes and quickly linking to cumulative Q&A on
that topic is a plus. My first attempt at this was to set up a
[slack.com](http://slack.com) team for courses in our department, and
then setting up a channel for each of the two courses I teach. As
`slack` does not allow sub-channels, the discussions need to be
organized in some way. I went about this by assigning a mnemonic in the
course notes that should be mentioned when a threaded discussion is
started in `slack`. Students can search for discussions about a
subsection in the notes by searching for that mnemonic. I have put
hyperlinks from the notes to a slack search expression that is supposed
to bring up discussions related to the mnemonic in the course's `slack`
channel. The problem is that `slack` doesn't have a formal URL
construction that guarantees that a hyperlink to a URL with that
expression will cause the correct discussions to pop up in the user's
browser. This is a work in progress, and other ideas are welcomed. See
Section 10.5.1 of [BBR](https://hbiostat.org/doc/bbr.pdf)
for an example where an icon links to slack (see the mnemonic
`reg-simple`).

Besides being hard to figure out how to create URLs o get the student
and instructor directly into a specific discussion, `slack` has the
disadvantage that users need to be invited to join the team. If every
team member is to be from the instructor's university, you can configure
`slack` so that anyone with an email address in the instructor's domain
can be added to the team automatically.

I have entertained another approach of using [disqus](http://disqus.com)
for linking student comments to sections of notes. This is very easy to
set up, but when one wants to have a separate discussion about each
notes subsection, I haven't figured out how to have `disqus` use
keywords or some other means to separate the discussions.

[stats.stackexchange.com](http://stats.stackexchange.com) is the world's
most active Q&A and discussion board for statistics. Its ability to
format questions, answers, comments, math equations, and images is
unsurpassed. Perhaps every discussion about a statistical issue should
be started in `stackexchange` and then linked to from the course notes.
This has the disadvantage of needing to link to multiple existing
`stackexchange` questions related to one topic, but has the great
advantage of gathering input from statisticians around the world, not
just those in the class.

No mater which method for entering Q & A is used, I think that such
comments need to be maintained separately from the course notes because
of the dynamic, reproducible nature of the notes using `knitr`. Just as
important, when I add new static content to the notes I want the
existing student comments to just move appropriately with these changes.
Hyperlinking to Q & A does that. There is one more issue not discussed
above - students often annotate the `pdf` file, but their annotations
are undone when I produce an update to he notes. It would be nice to
have some sort of dynamic annotation capability. This is likely to work
better as I use `R bookdown` for new notes I develop.

I need your help in refining the approach or discovering completely new
approaches to coordination of information using the course notes as a
hub. Please add comments to this post below, or short suggestions to
`@f2harrell` on `twitter`.

--------
Resources to investigate:

* [SideComment.js](http://aroc.github.io/side-comments-demo)
* [Medium](http://medium.com)
* [genius.it](http://genius.it)
