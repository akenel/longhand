# Longhand

**A writing desk you own, with an assistant that checks your work against the
real thing — not against its own memory.**

Longhand is a distraction-free markdown editor in your browser, plus an AI
assistant that can read the same files you're editing. It does **not** write for
you. It fixes your typos, keeps track of your references, and — the part nothing
else does — checks whether what you wrote is still true.

Built on [Freehold](https://github.com/akenel/freehold). Self-hosted. Bring your
own brain.

> Working title. See [`docs/BRIEF.md`](docs/BRIEF.md) for what this is, who it's
> for, and what it deliberately isn't.

## Why

Michael Webb of Jisc's National Centre for AI [wrote up his
setup](https://nationalcentreforai.jiscinvolve.org/wp/2026/08/07/how-i-use-claude-cowork-as-a-writing-assistant/)
on 2026-08-07: VS Code as a plain markdown editor, an AI given access to the
folder, and a few files holding references and sources. His closing line is the
one that matters —

> *"Trying to shoehorn our existing familiar tools into an AI-assisted workflow
> might well be the wrong direction of travel."*

He built it by hand because no product does it. That's the gap.

His assistant checks his writing against a folder of sources he collected.
Longhand checks it against **the thing it describes** — grep the code, read the
config, call the endpoint. Because the hard problem with documents isn't writing
them. It's that a spec is stale the moment it's written, and nothing tells you.

## Start writing (5 minutes, local)

```bash
cp .env.example .env
# point WRITING_DIR at what you're working on
docker compose up -d
```

Open <http://127.0.0.1:8443>.

It binds to loopback only. Nothing else on your network can reach it, which is
why it needs no password.

First run, install one extension: **Code Spell Checker**
(`streetsidesoftware.code-spell-checker`). It persists.

## The folder convention

Borrowed from Webb, because it's right. The filesystem is the data model.

```
your-book/
  01-chapter.md          the writing. the only file you think about
  references.md          every reference, and where it's used
  sources/               core material, gathered up front
  research/              things picked up along the way
  HOW-THIS-WORKS.md      describes the setup — for you AND the assistant
```

Everything but the chapter stays out of your way unless you go looking.

## On the box (later, and deliberately)

`docker-compose.prod.yml` puts it behind Keycloak on your own domain.

**Read its header first.** code-server is an editor with a terminal in it —
publishing it means whoever gets through the login has a shell on the box. That's
a decision, not a default, which is why local is the default and prod is opt-in.

## Status

Early. The editor stands up; the checking assistant is the next piece.

The first milestone is honest and small: **chapter 2 of the Human-Green book gets
written in this.** If that's annoying, we learned it for the price of a compose
file.

## Licence

MIT.
