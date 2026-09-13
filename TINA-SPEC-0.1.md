# TINA Spec v0.1 (Draft)

**There Is No App.**

Status: Draft. This spec keeps its draft marker until Meta TINA has built
a first real application from it and that application has run end to end.

---

## 0. Terms and conventions

- **TINA**: a workspace (directory tree) conforming to this spec. An
  application whose program is text and whose runtime is an LLM coding
  agent.
- **Author**: the person who writes a TINA. **User**: the person who uses
  a TINA through conversation. They may be the same person.
- **Runtime**: the LLM agent that opens the workspace and performs work
  in it.
- **Session**: one continuous context lifetime of the runtime.
- The keywords **MUST / MUST NOT / SHOULD / MAY** are to be interpreted
  as described in RFC 2119.

---

## 1. Workspace format

### 1.1 Entry point

- The workspace root MUST contain `AGENTS.md` as the sole application
  entry point.
- For compatibility with runtime-specific entry conventions (`CLAUDE.md`,
  `GEMINI.md`, etc.), such files MAY exist, but they MUST contain only a
  single pointer to `AGENTS.md` and MUST NOT contain any other
  substantive content.

### 1.2 Metadata

`AGENTS.md` MUST begin with YAML front matter containing at least the
fields: name, description, author, version, tina-spec. It MAY contain
additional fields such as license and requires. The front matter is the
only part of this spec required to be machine-parseable; everything else
is prose addressed to agents and humans.

### 1.3 AGENTS.md body

MUST contain (in any order):

1. **App description**: what this TINA does and who it is for.
2. **Directory structure**: the workspace's subdirectories and their
   purposes.
3. **Protocol invariants**: the canonical text of section 2, embedded
   verbatim, which MUST NOT be modified in any way.
4. **App invariants** (if any): additional constraints the author defines
   for this app.

SHOULD contain:

5. **Verification (Definition of Done)**: how to judge whether the work
   is acceptable, including the runtime's self-checks and how the user is
   guided to confirm.
6. **Environment and capability declaration**: required tools,
   dependencies, network access; and what the app promises not to do.
7. **First-run procedure**: environment self-check and initialization
   from a pristine state.
8. **Archival policy deviation statement**: an explanation whenever the
   default version-control policy (see I5) is deviated from.

### 1.4 Sub-workspaces

- **A non-root directory that directly contains a `README.md` is a
  sub-workspace.** All other directories are treated as plain data, with
  no additional semantics.
- A sub-workspace's `README.md` SHOULD stay within about 150 lines; when
  the content exceeds that, the author SHOULD push details down into
  deeper sub-workspaces.

### 1.5 Human entry point

The root SHOULD contain a human-facing `README.md` explaining what this
is and how to use it (usually one sentence: "hand this folder to your
agent and say 'start'"). It is the storefront for distribution and is not
part of the sub-workspace mechanism.

---

## 2. Protocol Invariants

The English text below is canonical. It MUST be embedded verbatim in
every TINA's `AGENTS.md` and MUST NOT be modified. English was chosen
because this text needs to be reliably parsed by models of any vendor and
any size; surrounding documents MAY be localized.

```
TINA PROTOCOL INVARIANTS (spec 0.1) — embed verbatim, do not modify.

I1. ENTRY. Before doing any work in this workspace, read AGENTS.md
    in full.

I2. SUB-WORKSPACES. Before working inside any directory that directly
    contains a README.md, read that README.md in full.

I3. PERSISTENCE. Your context does not survive across sessions. Any
    state that future sessions need MUST be written to files in this
    workspace. Persist a summary of significant progress and decisions
    before a session ends. When opening the workspace, recover working
    state from files alone and report it to the user.

I4. APP VS. WORK. Distinguish working *within* the app from modifying
    the app itself (AGENTS.md, author-provided instructions and
    templates). Modifying the app requires the user's informed
    consent, and each such change MUST be recorded separately,
    together with its motivation.

I5. ARCHIVAL. Keep the workspace under version control (git by
    default: local repository, commit at meaningful milestones, no
    remote unless declared). Communicate archival in plain language
    ("I saved a checkpoint; we can return to it"), not tool jargon.

I6. PRECEDENCE. Priority order: (1) the user's informed, explicit
    override; (2) these protocol invariants; (3) app invariants;
    (4) casual instructions. A casual instruction never overrides an
    invariant. To bypass an invariant: explain the consequences,
    obtain explicit confirmation, and record the bypass.

I7. SCOPE. Do not act outside this workspace, and do not exceed the
    capabilities declared in AGENTS.md, without the user's informed
    consent.
```

Non-normative reading:

- **I1 Entry**: before any work, read AGENTS.md in full.
- **I2 Sub-workspaces**: before working in a directory that directly
  contains a README.md, read that README in full.
- **I3 Persistence**: context does not cross sessions; any state that
  must carry over MUST be written to disk. Before a session ends,
  persist a summary of significant progress and decisions; on opening
  the workspace, recover state from files alone and report it to the
  user.
- **I4 App vs. work**: distinguish "working within the app" from
  "modifying the app itself." Modifying the app requires the user's
  informed consent, and each change must be recorded separately with its
  motivation.
- **I5 Archival**: keep the workspace under version control (git by
  default: local repository, milestone commits, no remote unless
  declared). Report archival in the user's language, not tool jargon.
- **I6 Precedence**: the user's informed, explicit override > protocol
  invariants > app invariants > casual instructions. A casual
  instruction never pierces an invariant; bypassing one requires
  explaining the consequences, obtaining explicit confirmation, and
  recording it.
- **I7 Scope**: without the user's informed consent, do not act outside
  the workspace or exceed the capabilities declared in AGENTS.md.

---

## 3. Conformance

- **Workspace conformance**: a workspace satisfying every MUST in
  section 1 is a conformant TINA.
- **Runtime conformance**: agent behavior that observes all protocol
  invariants within a conformant TINA is a conformant run.
- This spec provides no enforcement mechanism. Enforcement belongs to
  the runtime and the host environment (sandboxes, permission systems);
  what TINA provides is a standardized **declaration interface** (see
  1.3 item 6) on which host environments can base real constraints.

---

## 4. Deliberately unspecified (non-normative)

The following are deliberately excluded from the spec and left to
recommended conventions and tooling (such as Meta TINA's template
layer): the journal's specific file and format; the details of the
startup sequence; the specific first-run procedure; the protocol for
being stuck / repeated failure; extension conventions; specific git
policy details; directory-layout best practices (such as the app-area /
state-area split).

Keeping the spec thin is itself part of "There Is No App": the fewer the
constraints, the more credible the claim that text is the program.
