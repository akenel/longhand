# The check

*This file is the product. Everything else is a text editor.*

Point the assistant at a chapter and say **"run the check."** It does the
following, in order, and writes the result to `check-report.md`.

## What it does

**1. Pull every factual claim.** A number, a date, a name, a quotation, a
version, a measurement, a "we did X and Y happened." Not opinions. Not argument.
Claims that could be false.

**2. Find the source for each one.** Not from memory — by going and looking:

- `grep` the repo
- read the file it names
- read the commit that made the change
- call the endpoint, query the database, check the config that's actually running

**3. Sort each claim into exactly one of four buckets.**

| Verdict | Means |
|---|---|
| **VERIFIED** | Found the source. Quote it with a path and a line or commit. |
| **STALE** | Was true. Isn't now. Show both, old and current. |
| **UNSOURCED** | Can't find any basis for it anywhere. |
| **UNCHECKABLE** | No source exists on this machine — a person said it, or it's external. Mark it and move on; this is not a failure. |

**4. Report.** Every claim, its verdict, and its evidence. Verified claims get one
line. Anything not verified gets the detail.

## The rules that make it worth running

**Go and look. Never answer from memory.** The assistant wrote most of these
claims. It will remember meaning them. Memory is exactly what's being tested — a
verdict with no path, line number or command behind it is not a verdict.

**UNSOURCED includes invented colour.** The first run of this check on chapter 1
found *"the fix he made last Tuesday about which grinder is which."* The word
"grinder" appeared nowhere in the repo except in that sentence. It was invented
because it read well. That is the failure mode this check exists for, and it will
happen again, because plausible detail is what fluent writing is made of.

**A config value is not a fact about a person.** The same run found "his 52
categories" — which was `MAX_TAXONOMY` raised from 40 to 52 in a script. A number
in a repo is not a number in someone's shop.

**Report UNCHECKABLE honestly.** *"I just want to scan stuff and know what I
sold"* has no source on disk. A man said it. That's fine, and pretending to
verify it would be worse than admitting it can't be.

**Say what you did not check.** A check that quietly skips half a chapter reads
exactly like a clean one. If you ran out of room, say where you stopped.

## Files

```
your-book/
  01-chapter.md          the writing
  references.md          claim -> source, built up as you go
  sources/               material gathered before you started
  research/              material gathered along the way
  check-report.md        output. overwritten each run.
  CHECK.md               this file
```

## Why the whole thing exists

A spec is stale the moment it's written and nothing tells you.

`freehold/docs/human-green/` has the receipts: a disaster-recovery runbook that
told you to throw your backups away, and a published claim that no data left the
country while the model endpoint sat in the United States. Both were confident,
professional, and wrong. Both were found the same way — someone went and looked.

The check is that person, made repeatable.
