# GPU-Passthrough

Welcome to my approach of setting up a machine that runs multiple operating systems simultaneously and each of it works
nearly as good as if you installed and ran it on a normal system. Curious what I'm talking about? Maybe you already heard
of something like gpu-passthrough, looking-glass or vfio. It doesn't matter if not since I try to explain what I know and
will learn about it. Of course, it's a complex topic and you could become confused while reading some of my texts if you
don't know some basic things. But as I already said, I'll try to explain as much as possible. In the first instance I want
to document everything I do. Thus I should be able to create a similar machine in the future. On the other hand I want to
share my experiences and my knowledge with other people. Therefore I'll appreciate any contribution, no matter if it's some
sort of correction or an addition.

## Disclaimer

If some applications look different from picture to picture don't be disctracted by that. I tested some themes, fonts and
icons while working on this project and didn't want to revert everything I configured so far just to make one or two
screenshots. However, the placement of UI elements and the functionality of most applications is still the same. You should
be able to follow my steps anyway.

## Overview

I divided this repo in several sub-sections. Don't expect a perfectly fine structure but most of the things I wrote down
, should be in the right document.

- [my plan](explanations/overview.md)
- [used hardware](hardware) for this build (regularly updated)
- [actual passthrough](passthrough)
  - [Pop!_OS](passthrough/arch/)
  - [Arch](passthrough/pop_os/)
- [ideas](ideas.md) for further steps
- [glossary](explanations/glossary.md), if you want to look up some synonyms
- [the problems](problems.md) I went into
- [all sources](sources.md) that I used and helped me while working on this machine
- [games](games.md) I tested so far
