# Longhand — the brief

*Written 2026-08-11. Read this before building anything. If the code argues with
this file, the code is wrong.*

---

## The one sentence

**A writing desk you own, with an assistant that checks your work against the
real thing — not against its own memory.**

## Where the idea came from

Michael Webb (Jisc National Centre for AI) wrote up his setup on 2026-08-07: VS
Code as a distraction-free markdown editor, Claude Cowork given access to the
folder, and a small pile of markdown files holding references, sources and
research. Claude fixes typos, tracks references, fact-checks against the sources,
and gives feedback. It does **not** write for him.

His closing point is the important one:

> *"Trying to shoehorn our existing familiar tools into an AI-assisted workflow
> might well be the wrong direction of travel."*

He hand-rolled it because no product does this. That's the gap.

## What makes it different

Webb's assistant checks his writing against **a folder of sources he collected**.

Longhand checks your writing against **the thing it describes**.

That's the whole product. A document about a system can be checked against the
system: grep the code, read the config, call the endpoint, query the database.
Not "does this read well" — *is this still true.*

Because the honest problem with documents isn't writing them. It's that **a spec
is stale the moment it's written**, and nothing tells you. Storage tools don't.
Word doesn't. Adding a chat box to either doesn't.

This is not a guess. `freehold/docs/human-green/` has seven dated cases:

- A disaster-recovery runbook that told you to throw away your backups. Correct
  when written. Never revisited after backups existed.
- A published claim that the AI stack kept every prompt in the country. One grep:
  the endpoint was in the US, and two of the three named components existed
  nowhere in the repo.
- A backup job, green every night, that silently didn't cover half the data.

Every one was found by a human going and looking. That looking is the product.

## Who it's for

**One reader: a writer working on something long, who wants to own their tools.**

A book, a thesis, a spec, a team handbook. Long enough that they lose track of
what they claimed in chapter 2. They already dislike Word. They don't want their
draft on someone else's server. They do **not** want the machine writing for them.

Everyone else is secondary and gets no accommodations:

- **Teams** are the same product with Keycloak switched on. Freehold gives that
  for free the day a team turns up. Do not build for them first.
- **Developers** already have Cursor and Claude Code. Not the target.

The standing risk is drifting toward teams because that's where the money looks
like it is. That builds a worse product for nobody. Writer first.

## What it is NOT

- **Not an AI writing tool.** It does not generate your prose. That market is
  saturated and the product is the opposite promise: these are your words.
- **Not a Cursor competitor.** Cursor is for code, for developers.
- **Not a Confluence/Notion clone.** Those store documents. This checks them.
- **Not a VS Code fork.** See below — this one matters most.

## The build: assembly, not invention

Four of the five pieces already exist and run:

| Piece | Where it is |
|---|---|
| The method — memory, worklist, standing rules | `ground-control` (public template) |
| An AI with real filesystem + exec access | `ground-control-bridge` (FastAPI over Tailscale) |
| Platform: Postgres, Keycloak SSO, MinIO, Caddy | `freehold`, running on the box |
| Chat UI, SSO'd, bring-your-own-brain | Open WebUI at `ai.wolfhold.app` |
| **The writing surface** | **Missing. This is what we build.** |

### Do not fork VS Code

`code-server` / `openvscode-server` are open source and run VS Code in a browser.
They drop into the compose file, sit behind Caddy, and authenticate through
Keycloak like everything else.

A fork means a permanent merge treadmill against Microsoft. Cursor did that with a
funded team. We are one person. Fork later if the product ever earns it; it
almost certainly won't.

Everything Webb configured is a settings file we ship as the default:

- Light, low-contrast theme
- Serif font for markdown, ~17pt, 1.7 line spacing
- Wrap at 80 characters
- No line numbers, no minimap, no autocomplete
- Spellcheck + a personal dictionary

That "distraction-free writing environment" is a config, not a feature. Ship it as
the default and the first-run experience is already better than his.

### The folder convention IS the data model

Straight from Webb, and it's good. Don't invent a schema — use the filesystem:

```
book/
  01-chapter.md          the writing. the only file the user thinks about
  references.md          every reference, with where it's used
  sources/               core material, extracted to markdown up front
  research/              things gathered along the way
  HOW-THIS-WORKS.md      describes the setup, for the human AND the assistant
```

Everything except the chapter is invisible unless you go looking. That's the
point.

## Deployment shape

- **Separate repo**, built *on* Freehold — which makes it Freehold's first real
  proof, instead of bloating Freehold with an app.
- **Same box**, own subdomain. The `*.wolfhold.app` wildcard cert already exists
  (Porkbun DNS-01), so a new subdomain costs nothing.
- **Shared Keycloak**, same as Open WebUI today.
- **Bring your own brain.** The user supplies the key, or points at a local
  model. This is also the honest answer to Webb's stated worry about cost and
  environmental impact — he's on $100/month and uneasy about it. Let people run
  a small local model for typo passes and save the big model for the fact check.

## First milestone

**Chapter 2 of the book gets written in Longhand by the weekend.**

Not a feature list. If writing in it is annoying, we learned that for the price of
a compose file. If it isn't, the book is customer zero and the tool is real.

That also means the tool never competes with the book for hours. It's the desk
the book gets written on.

## Two things Webb hit that we should fix

1. **"I have to remember to save before I ask Claude to do anything."** He calls
   it awkward and easily forgotten. Auto-save on assistant invoke. Small, and it
   removes the one sharp edge in his whole workflow.
2. **Context rot.** He works around it by making Claude write references and
   evidence to files instead of trusting the chat window. Good instinct — make it
   the default, not a workaround.

## Open questions

- Does the check run on demand ("check this chapter") or continuously? Start on
  demand; continuous is a nice-to-have that could get expensive fast.
- What does a failed check look like on screen? An inline mark, a report, a diff?
- Export: markdown → Word/PDF/EPUB/HTML. Webb has Claude write a Python script per
  project. We should ship one that works.
- Name. `Longhand` is the working title. Alternatives: `Wordhold`, `Scriptorium`.
  Cheap to change while the repo is empty.
