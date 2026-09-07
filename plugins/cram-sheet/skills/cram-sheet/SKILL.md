---
name: cram-sheet
description: Build a single-page study guide for something you have to know cold by a deadline — a trivia night, a certification, a new job's org chart, a wedding toast. Turns sources into a NotebookLM pack (audio, video, poster, dossier) plus a self-contained HTML page, deployed free on Vercel and installable on a phone. Use when someone says they need to cram, study, memorize, or "know this by <date>".
---

# Cram sheet

The trick that makes this work: **no single tool does all of it.** Claude works out what to
ask for, NotebookLM makes the content, Claude builds the container, Vercel hosts it. The
handoffs are the whole thing. Worked example: `fourth-wing-trivia.vercel.app`, built in one
afternoon for a trivia night four days out.

## Step 1 — Interview, do not accept the brief

The person will say "make me a study guide." That is not the brief. Ask, in their words:

- What is the event, and **what day**? The date sets how much is worth building.
- What do they **already know**, so the page does not waste space on it.
- Where do they actually **panic**? Names, dates, family trees, who-betrayed-who, acronyms.
  This is the real spec. For Fourth Wing it was names and family trees, so the page got a
  hand-drawn family tree and a who's-who — neither was asked for.
- How will they **use** it? In the car, on a phone, in a waiting room five minutes before?
  That decides audio vs. text, and phone-first vs. laptop.

- **What form do the sources take?** A file they own (ebook, PDF, slide deck, handbook), or
  nothing but what is on the web? NotebookLM needs something to chew on, and the answer changes
  the whole source list. Three cases:
  - **They own a file** — that is the best source there is. It goes in whole.
  - **Audio only, or they took it in by ear** — there is no file. Feed published recaps, wikis,
    reviews and summaries instead. This is what the *Fourth Wing* guide ran on, and it was enough.
  - **Nothing yet** — go find the recaps yourself before writing the prompt.

**Watch for a scope mismatch.** People answer with what they have read, not with what they will be
tested on. If someone says "book one only" and the event covers two, say so once, plainly, then
build what they asked for. Their reading being behind is usually the reason the guide exists.

**"I've heard the names but never seen them written"** deserves its own note. Anyone who listened
rather than read knows the story cold and cannot spell a single name. For them the page opens with
names — spelled, with pronunciation — before any plot at all.

Write the source list and the NotebookLM prompt out of those answers, not out of the topic.

## Step 2 — Hand off to NotebookLM (this step has human hands in it)

**You cannot drive NotebookLM.** No API, and notebooks are not Drive files, so no token
reaches them. Hand the person a prompt and a source list; they paste and click.

Ask them to generate, then download:

| Studio item | Download? |
|---|---|
| Audio Overview | Yes — ⋮ menu → Download |
| Video Overview | Yes — ⋮ menu → Download |
| Reports (e.g. a dossier) | Yes — exports as PDF |
| Data tables | Yes — exports to Google Sheets (readable via her token) |
| Mind map, flashcards, quiz | **No download exists.** Interactive only — link to them instead |

Files land in `~/Downloads`, which you can read directly. "Hit download, say done" — no
filename needed.

If the page will link the notebook, they must set it to **Anyone with a link** (Share →
Notebook Access), and tick **Allow copies** so people can duplicate it for their own topic.
Note honestly in the page or post: viewers still need to be signed in to *some* Google
account. A signed-out visitor always lands on a Google sign-in, even when the share is
correct — so **never diagnose the share with curl**, and never tell them to re-click Save on
the strength of one. The real test is an incognito window.

## Step 3 — Re-encode before hosting

NotebookLM ships audio at 256kbps stereo. It drops ~4x with no perceptible cost for two
people talking:

```
ffmpeg -i in.m4a -ac 1 -c:a aac -b:a 64k out.m4a
```

Fourth Wing went 40MB→11, 79MB→21, and the video 51MB→22. A phone on venue wifi is the
target, so this is not optional.

## Step 4 — Build one self-contained HTML file

One `index.html`. Fonts inlined as base64, no CDN, no build step — it has to open on a bad
connection in a parking lot.

The spine that worked, in this order:

1. **If you only remember five things** — the panic-button section, first on the page.
2. **The world in ninety seconds** — orientation.
3. **Terms the host will use** — vocabulary, so nothing else is confusing.
4. **Who's who** — hand-authored SVG family trees beat any prose here.
5. **Section per unit** — book by book, module by module, team by team.
6. **The easy mix-ups** — the two things everyone confuses. High value, always.
7. **Self-quiz** — `<details>` per question, plus a "reveal all" button.
8. **The media** — audio and video embedded, poster inline.
9. **Everything else** — link the interactive NotebookLM items you could not download.

A collapsible sidebar (drawer under 1120px, fixed above it) is what makes it usable on a
phone. Match the subject's own palette — dark and gold for dragons — so it feels made, not
generated.

## Step 5 — Make it installable

`manifest.webmanifest` plus 192/512 icons and an apple-touch-icon. Being able to add it to
the home screen is most of why it gets used on the day.

## Step 6 — Deploy

```
cd <project> && npx vercel deploy --prod --yes --scope lwhela12s-projects
```

The `--scope` flag is mandatory; there is no global `vercel` on this machine. Use
`vercel domains add`, never `vercel alias set` — alias set puts the page behind a Vercel
login. `curl` the live URL after deploying.

## Step 7 — Footer, and the honest bit

Footer carries the Saved You a Seat plug and copyright. If the subject leans on someone's
IP — a novel, a franchise — say so plainly: unofficial fan-made study aid, not affiliated
with the author or publisher. Also say where the facts came from and where the fandom
disagrees. It costs two sentences and it is the difference between a study aid and a
knock-off.

## Step 8 — Walk it before calling it done

Open the deployed URL yourself. Click the notebook link. Play the audio. Take the quiz.
Shoot it at 1280, 768 and 390 before saying the UI is done. Then hand over the link.
