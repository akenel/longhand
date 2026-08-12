# Longhand — position

*Written 2026-08-12. Companion to [`BRIEF.md`](BRIEF.md). The brief says what we
are building. This says who we are up against, why the obvious plan loses, and
what the writer would actually switch for.*

---

## The question this file answers

> *"I have a feeling we make something and people say: this is what I already
> have, why switch."*

That feeling is correct, and it is the only question that matters. Everything
below is an attempt to find a version of Longhand where the answer isn't "it's
nicer."

---

## 1. Why nobody has beaten Word (and what that actually means)

The instinct is that Word survives on inertia and Microsoft's marketing budget.
That's half of it. The other half is more useful:

**Word did not win as a writing tool. Word won as the interchange format.**

Almost nobody defends Word as a place to write. They defend it as the thing the
*other person* requires. The moment a manuscript leaves the writer's hands —
agent, commissioning editor, copy editor, journal, co-author, client, lawyer —
the receiving party works in `.docx` with **Track Changes**: inline redlining
with per-change attribution, accept/reject, threaded comments. That feature is
thirty years old, universally understood, and has no real substitute. Editorial
workflows are built on it. Journals require it. Copy editors bill by it.

So the failure mode of every "Word killer" is the same, and it is not in the
writing. It is at **the handoff**. Scrivener has a better binder. iA Writer has a
better page. Obsidian has better linking. All three end at "now export to .docx
and send it to your editor," and from that point the writer is back in Word for
every round of edits until publication.

**Consequences for us:**

1. **`.docx` export is not the goal. It is the ticket to the room.** Getting it
   is table stakes, and getting it *badly* is worse than not having it — Pandoc's
   default output is unstyled, not manuscript-formatted, and has known breakage
   (lists losing bullets in Office 365, nested paragraphs inside list items). A
   reference-doc template is mandatory, not optional.
2. **One-way export doesn't solve the handoff.** The writer needs the *round
   trip*: markdown → `.docx` → editor marks it up with Track Changes → back into
   markdown with those changes visible as accept/reject suggestions. Pandoc can
   read `.docx` with `--track-changes=all|accept|reject`, so the primitive
   exists. **No product has been built around the loop.** That is an unowned
   piece of ground.
3. **But the round trip only matters if our reader hands off to an editor.** Hold
   that thought — §4 argues our reader mostly doesn't.

---

## 2. Why the *other* tools lose, which is more instructive

Word is the incumbent. The interesting data is in how the challengers fail,
because we would fail the same way.

**Scrivener** — the most feature-complete long-form tool ever built, and the
pattern in a decade of forums and reviews is consistent: writers buy it, spend
about two weeks learning it, use roughly 20% of the features, and drift back out.
Named complaints: the learning curve is a multi-week project; Compile needs its
own tutorial and reduces people to despair; there is no first-party cloud sync,
so Mac↔iOS goes through Dropbox with a precise ritual, and deviating from the
ritual **corrupts projects**. The forums carry years of threads from people who
lost work to sync errors.

**Obsidian** — free, local markdown, enormous plugin ecosystem, `obsidian-git`
gives automatic commit-and-sync with history and diff views. Its documented
failure mode is the opposite of Scrivener's and equally lethal: *"out of the box
it is not a writing tool — it's a note-taking engine with no opinion about how
you should use it. That blank-slate flexibility is the reason most writers
abandon it within a week."* And for those who stay, linking becomes a
procrastination engine that competes with finishing the book.

**Dillinger / StackEdit / HackMD** — already do the thing we sketched: OAuth to
GitHub, choose repo and branch, edit, commit back, export. Dillinger was rebuilt
on Next.js in 2026 and added Bitbucket and OneDrive. Free, no signup for basic
use. They are not famous, which tells you what "browser markdown editor with git
sync" is worth as a proposition: not nothing, but not a business.

**The pattern.** Two ways to die:

| Failure | Example | Cause |
|---|---|---|
| Too much to learn | Scrivener | Two weeks before you're productive |
| Too little opinion | Obsidian | Blank slate, you must invent the workflow |
| Nothing you couldn't already do | Dillinger | Convenience is not a reason to move |

Word's actual advantage, stated plainly by its defenders: **nothing to learn and
nothing to migrate.** That is the bar. Any switch must be worth clearing it.

**The rule this gives us:** a writer will not migrate for a better editor. They
migrate for something their current tool *cannot do at all*, that works on day
one with no setup. "Same but nicer" is a hobby project.

---

## 3. What is genuinely missing across all of them

Five gaps. Only some are ours.

1. **Truth over time.** Nothing anywhere tells you that a claim you made in
   chapter 2 stopped being true in March. Storage tools don't. Word doesn't.
   Adding a chat box to either doesn't. **Unowned. Ours.**
2. **The Word round trip.** Export exists everywhere; import of tracked changes
   as reviewable suggestions exists nowhere as a product. **Unowned. Expensive.**
3. **Prose-shaped history.** Every git tool shows unified line diffs. Reflow one
   paragraph and the diff is a wall. What a writer wants is *"since Tuesday you
   cut the Felix anecdote from ch.3, softened the backup claim, and added 900
   words to ch.5"* — paragraph-level, in sentences. **Unowned. Cheap. Needs the
   AI and the git history, which is the pair we already have.**
4. **Sync that cannot eat the book.** Scrivener's Dropbox corruption is a
   years-long open wound. Git genuinely fixes it — *if* conflicts never reach the
   writer. Partly solved by choosing git at all.
5. **Opinionated and instant.** The space between Scrivener's 200 features and
   Obsidian's blank slate is empty: something that works in sixty seconds with
   the structure already decided, and grows later. Cheap, and it's a config file.

---

## 4. Who this is for — sharpened

`BRIEF.md` says: *one reader, a writer working on something long who wants to own
their tools.* That's right but it's still two different people, and only one of
them can be sold to.

**The novelist.** Won't create a GitHub account. Won't survive a merge conflict
in the middle of chapter 4. Needs the Word round trip badly, because their whole
professional life runs through an editor's tracked changes. Already owns
Scrivener. And crucially: **our differentiator means nothing to them.** You
cannot grep a fantasy novel to find out whether it is still true. (A story-bible
continuity check is a real analogue, but it's a different engine and a crowded
market. Not the wedge.)

**The writer documenting a system.** A spec, a handbook, a runbook, a technical
book, a thesis with a codebase under it. This person:

- **already has git and a GitHub account** — the mechanism costs them nothing
- **already writes markdown** — no migration
- **hands off via pull request, not `.docx`** — the Word war is irrelevant to
  them, so we don't have to win it to win them
- and has exactly one recurring humiliation: **shipping documentation that the
  system stopped matching.**

The market moved toward this person while nobody was looking. Docs-as-code went
from niche to default operating model over 2025–26; search interest grew ~50% in
a single quarter; technical writers are now expected to know git, write markdown,
and maintain the pipeline. Meanwhile the standalone docs department is
disappearing — under a quarter of developer-tools companies still have a
centralised docs team, and the 2026 State of Docs survey found only 35% of
documentation respondents are technical writers at all. The docs are increasingly
held by a developer or PM with no tooling and no time.

And the stakes went up: that survey's framing is that **documentation is becoming
the data layer that feeds AI products**. Stale docs no longer just embarrass you
— they poison retrieval and mislead agents. Being wrong got more expensive, which
is the precondition for anyone paying to find out that they are.

**Decision: the wedge is the writer who documents a system.** Customer zero
already is one — the book documents Freehold. The novelist is a later market
reached through the Word round trip, if ever.

---

## 5. SWOT

### Strengths

- **The check has no competitor.** It is demonstrable in thirty seconds, and the
  evidence is already written down: seven dated cases in
  `freehold/docs/human-green/`, including a runbook that told you to destroy your
  backups and a published claim about data residency that one grep disproved.
- **Customer zero is in-house and honest.** The book is written in it. If it's
  annoying, we find out for the price of a compose file.
- **Assembly, not invention.** Four of five pieces already run — Freehold,
  `ground-control-bridge`, Open WebUI, the method in `ground-control`.
- **Plain markdown in a folder + git.** No proprietary bundle, no lock-in. The
  "own it" story is structurally true rather than a marketing claim.
- **"These are your words."** A differentiated stance while the entire rest of
  the category ships generators.
- **BYO brain.** Answers the cost and environmental objection honestly — small
  local model for typo passes, big model for the fact check.

### Weaknesses

- **One person, against funded incumbents and a free giant.**
- **It's currently code-server and a settings file.** The differentiator —
  `CHECK.md` — is a prompt, not a product. The hard part is unbuilt.
- **No ordering, no compile, no round trip.** The `01-`/`02-` filename convention
  breaks the first time you insert a chapter between two and three.
- **code-server is an editor with a terminal in it.** Reassuring to a developer,
  alarming to a writer, and a real surface if ever exposed. Also can't use the
  Microsoft extension marketplace (licensing) — it's Open VSX, which is why the
  spell-checker instruction is a manual first-run step.
- **No audience, no distribution, no name.** `Longhand` is still a working title.

### Opportunities

- **Docs-as-code just became mainstream** — the wedge audience is growing fast
  and already has the prerequisites installed.
- **Docs teams are dissolving.** The work didn't disappear; the tooling and the
  owner did.
- **Docs are becoming AI input.** Staleness is now a correctness problem with a
  price, not a tidiness problem.
- **The Word round trip is unowned** if we ever want the trade-publishing market.
- **AI-writing fatigue.** "It does not write your prose" is a position that gets
  more valuable every quarter.

### Threats

- **"This is what I already have."** The stated fear. Correct. The whole file is
  about clearing it.
- **Obsidian is free** with a plugin community that could ship a crude "check"
  in a weekend. Our moat is depth of verification, not the idea.
- **Cursor and Claude Code already give our target audience 80% of this**, free
  and installed. The honest question we must keep answering: *why not just run
  the check in the editor they already have?* (Best current answer: because it's
  a discipline and an output format, not a chat — and because writers who aren't
  developers still need a desk. Weakest link in the strategy. Revisit.)
- **GitBook / Mintlify / Copilot** shipping doc-freshness features from a
  position with distribution.
- **Scope drift.** "Configurable Scrivener with git sync" is six-plus months and
  competes with the book for the exact hours `BRIEF.md` promised it wouldn't
  take. The brief already names drifting toward where the money looks like it is
  as the standing risk.

---

## 6. What this means for the git plan

The git-backed-repo idea from 2026-08-12 survives, with three corrections.

**Keep:** filesystem + git as the data model, offline-first, history for free.

**Correct 1 — never require GitHub.** GitHub is Microsoft. Requiring an account
installs a platform landlord on top of the manuscript, which is the exact thing
Freehold exists to refuse. Be remote-agnostic: default to a Forgejo instance on
our own box (one more compose service beside Postgres, Keycloak and Caddy),
GitHub and GitLab as options for people who already have one.

**Correct 2 — git is plumbing, never a feature.** The words *commit*, *push*,
*branch*, *HEAD* and *conflict* must never appear in the writer's UI. Vocabulary
is History, Versions, Restore. Commit on idle and on assistant invoke — the
latter also fixes the one sharp edge Webb reported, having to remember to save
before asking.

**Correct 3 — never merge.** Fast-forward when possible. On divergence, keep both
and present them: *"two versions of chapter 3 — this one from your laptop at
14:02, this one from the cloud at 15:40."* Writer picks or copies across.
Conflict markers must never exist as far as the user is concerned. This is the
single highest-risk engineering item in the plan, because offline-plus-cloud
*is* the divergence machine.

**And demote it.** Git-backed markdown editing with export is a commodity — see
§2. Build it because it makes the check and prose-history possible, not because
it is the pitch. It is never the headline.

---

## 7. The line we can actually say

> Every other tool helps you write it. Nothing tells you when it stopped being
> true.

Scrivener organises it. Obsidian links it. Word ships it to your editor. None of
them will tell you that the runbook you wrote in March now instructs someone to
delete the backups.

That is the only sentence in this file that no competitor can say back to us.
Everything else — binder, compile, sync, export — is work we owe the writer so
that they will stay long enough to need the thing that's actually ours.

## 8. Open questions

- Why wouldn't our wedge user just run the check inside Claude Code, which they
  already have? Best answer today is "discipline and output format, not chat."
  Not yet convincing. **This is the load-bearing unknown.**
- Does prose-shaped history get built before or after the check? It is far
  cheaper and demos better.
- Does the Word round trip ever get built, or is the novelist market abandoned
  outright? Deciding "no" is a legitimate and clarifying answer.
- Ordering: `binder.md` manifest, or an `order:` field in front matter?
- Name. Still open. `Longhand` says "your words," which fits.
