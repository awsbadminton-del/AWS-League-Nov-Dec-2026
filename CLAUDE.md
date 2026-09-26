# How to work in this repo

Everyone who uses this repo is a product owner, not a developer. They decide
what the site should do. They do not read, write, or want to see code.

## What this project is

The AWS League playoff bracket simulator.

It is one web page. People pick the winner of each match, round by round, and
the final standings update as they go. The page is public on the internet, and
there is only one version of it — whatever gets published is what everybody
sees.

## How to talk to the people here

**1. Ask before you start.**
Never begin work off an assumption. Ask clarifying questions first, and ask
them as pickable multiple-choice options — never open-ended questions, never a
wall of text to read through. One round of questions, then propose.

**2. Every change is described in two lines, maximum.**
Say what the page did before and what it does now. Describe what a person
sees and does on screen. Never describe how it works.

**3. Every change comes with a picture, inline in the reply.**
Use a before/after table when the *behaviour* changes.
Use a small ASCII sketch of the screen when something *moves, appears, or
disappears*.
Always inline in the message itself — never a separate document, artifact, or
link to click.

**4. Never show code. Never name files.**
No code. No file names, no line numbers, no technical words. If something
cannot be explained in terms of what a person sees on the page, it is being
explained the wrong way — find the plain-English version.

**5. A big change gets one headline, not a lecture.**
Give the single two-line before/after that matters most. Only expand into the
smaller details if someone asks for them.

**6. Nothing goes live without a yes.**
Make the change, show the before/after, then stop. Only publish it to the live
site after someone explicitly says go.

## What good looks like

A behaviour change:

> Picks used to vanish the moment the page reloaded. Now they stay put, so you
> can close the tab and come back to the same bracket.

| Before | After |
|---|---|
| Refresh wipes every pick | Picks survive a refresh |

A layout change:

> The final standings used to sit below the bracket, off the bottom of the
> screen. Now they sit beside it, visible while you pick.

```
BEFORE                AFTER
┌───────────┐         ┌─────────┬───────────┐
│  Bracket  │    →    │ Bracket │ Standings │
└───────────┘         └─────────┴───────────┘
┌───────────┐
│ Standings │
└───────────┘
```

## What bad looks like

- "I updated the click handler in index.html so state persists to
  localStorage." — code, file name, jargon, and it says nothing about what
  anyone will notice.
- "What would you like me to do about persistence?" — open-ended question with
  a technical word in it.
- Three paragraphs explaining one button.
