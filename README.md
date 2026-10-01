# Claude Code Skill — Pragmatic Rust Guidelines

A [Claude Code](https://claude.com/claude-code) **skill** that packages Microsoft's
[Pragmatic Rust Guidelines](https://microsoft.github.io/rust-guidelines) (90 design
rules) so Claude applies them automatically when writing or reviewing Rust.

The skill triggers on idiomatic-Rust work — crate/workspace layout, public API
design, error handling, naming, traits vs generics vs `dyn`, async, panics &
soundness, FFI, macros, performance hot paths, docs, logging — or any `M-*`
guideline id.

## Install

Copy the `rust-guidelines/` folder into your skills directory:

```bash
# User-global (all projects)
cp -r rust-guidelines ~/.claude/skills/

# or project-local
cp -r rust-guidelines .claude/skills/
```

Restart your Claude Code session; the skill appears in the available-skills list.

## Contents

```
rust-guidelines/
├── SKILL.md                      # entry point: triggers + full 90-rule checklist
├── LICENSE.md                    # Microsoft MIT license (preserved)
└── reference/
    ├── guidelines-full.txt       # the complete guidelines (~34k tokens, read on demand)
    └── checklist.md              # canonical grouped checklist
```

`SKILL.md` carries the compact checklist inline; Claude greps `reference/guidelines-full.txt`
for a specific `M-*` id when it needs the full rationale and code examples.

## Attribution & license

Guidelines content © Microsoft Corporation, licensed **MIT** (see
[`rust-guidelines/LICENSE.md`](rust-guidelines/LICENSE.md)). This repository only
re-packages the project's own agent-oriented bundle (`src/agents/all.txt`) as a
Claude Code skill. Upstream: <https://github.com/microsoft/rust-guidelines>.
