# There Is No App (TINA)

**TINA** is a specification for applications whose *program is text* and
whose *runtime is an LLM coding agent*. A TINA is just a folder: no
installer, no interface, no server. You hand the folder to an agent
(Claude Code, Gemini CLI, or any agent that can read and write files),
say "start", and the agent runs the app by reading the text inside.

This repository holds the specification itself.

## Contents

- [TINA-SPEC-0.1.md](TINA-SPEC-0.1.md) — TINA Spec v0.1 (Draft). Defines:
  - the **workspace format**: `AGENTS.md` as the sole entry point, its
    front matter and required sections, sub-workspaces, the human-facing
    README;
  - the **protocol invariants** (I1–I7): the canonical English block that
    every TINA embeds verbatim in its `AGENTS.md`;
  - **conformance** for workspaces and for runtime behavior;
  - what the spec **deliberately leaves unspecified**.

## Status

Version 0.1 is a draft. It keeps its draft marker until Meta TINA has
built a first real application from it and that application has run end
to end.

## Related

- [Meta TINA](https://github.com/Pal-AI-Lab/META-TINA) — the reference
  application: a TINA that interviews domain experts and produces new
  TINAs. Its `app/spec/` directory carries the same spec text; the two
  copies are kept identical.

## Versioning

Each spec version is one file, `TINA-SPEC-<version>.md`. The protocol
invariants block inside it is versioned with the spec and must never be
edited in place; a change to the invariants is a new spec version.
