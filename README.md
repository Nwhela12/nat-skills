# nat-skills

Recipes I worked out once and wrote down so I never have to work them out again.
Built by [Nat Walstead](https://savedyouaseatstudios.com). Take what's useful.

These are **Claude Code plugins**. They install in the terminal or desktop app —
not a browser tab. If you don't have Claude Code, the instructions inside each
skill still read fine as plain English; you can follow them by hand.

## Install

```
/plugin marketplace add Nwhela12/nat-skills
/plugin install cram-sheet@nat-skills
```

Restart Claude Code once, and the slash command shows up.

## What's in here

### `/cram-sheet`

Build a single-page study guide for something you have to know cold by a
deadline — a trivia night, a certification, a new job's org chart, a wedding
toast where you cannot mix up the cousins.

Sources go in. What comes out is a phone-ready web page with the facts, a
self-quiz, and audio you can listen to in the car, live on the internet for
free and installable on your home screen.

```
/cram-sheet Quicksilver trivia night, Sep 27, covering both books
```

It will ask you four questions first. The important one is **where do you
actually panic** — that answer is what the page gets built around. The first
one of these was for a *Fourth Wing* trivia night; the honest answer was "names
and family trees," so the page got a hand-drawn family tree that nobody asked
for and everybody used.

**One step is yours.** Claude can't reach NotebookLM — there's no API and
notebooks aren't files. So partway through it hands you a prompt and a source
list and stops. You paste those in, generate the audio and video and report,
hit download, and say "done." It takes over again from there.

## Why these exist

A recipe you've only run once is a guess. Writing it down is how the second time
takes an hour instead of an afternoon — and the useful part is never the steps,
it's the traps. The button that doesn't exist. The flag you have to pass. The
thing that fails silently. Those are in here.
