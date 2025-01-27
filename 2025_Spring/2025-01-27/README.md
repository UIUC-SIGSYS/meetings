---
title: "What is Computer Systems?"
---

# Intro

<style>
.kinda-small {
  font-size: 80%;
}
.preserve-text {
  text-transform: none;
}
.code {
  background-color: black;
  color: darkgrey;
}
</style>

## [`*this`]{.kinda-small .preserve-text}

This presentation is available in markdown at

:::: {.kinda-small}
[https://github.com/UIUC-SIGSYS/meetings](https://github.com/UIUC-SIGSYS/meetings/blob/main/2025_Spring/2025-01-27/README.md)
::::

Compile with

```
pandoc --standalone --to=revealjs --output=README.html README.md --slide-level=2
```

Then open `README.html` in browser.

## Disclaimer

Nobody on the internet can seem to agree on the *exact* meaning of computer systems. Which is largely why I made this presentation, but it also means that this is largely my opinion, so feel free to raise your hand and disagree with me.

# Systems Overview

## Goal of computer systems

Doing computing [to make [more]{.fragment .show-up}[, better]{.fragment .show-up}[, faster]{.fragment .show-up}[, [`%s`]{.code}]{.fragment .show-up} computing possible]{.fragment .show-up}

## A non‑exhaustive list of sub‑fields in systems

:::: {.kinda-small .incremental}
- Kernels
- Operating Systems
- Compilers
- Networking
- Distributed Systems
- HPC
- Heterogeneous systems
::::

# SIG structure

## meetings

:::: {.incremental}
- seminars most weeks
- if there's interest, I'd love to do:
  - interactive workshops
  - projects (OS dev, Compilers, etc.)
::::

## participant involvement

:::: {.kinda-small}
I'm only one person—and a pretty busy one—who is only knowledgeable about *some* systems topics. I will do my best to bring in presenters to cover topics that are outside of my personal knowledge, but I'm certain at least [`%u`]{.code} of you know something interesting that I don't, so if you have an idea for a talk, *please* bring it to me.
::::

#

```
$ ./a.out
Bad system call (core dumped)
```
