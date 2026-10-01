# Claude Code Plugin — Pragmatic Rust Guidelines

A [Claude Code](https://claude.com/claude-code) **plugin** that ships Microsoft's
[Pragmatic Rust Guidelines](https://microsoft.github.io/rust-guidelines) (90 design
rules) as a skill, so Claude applies them automatically when writing or reviewing Rust.

The skill triggers on idiomatic-Rust work — crate/workspace layout, public API
design, error handling, naming, traits vs generics vs `dyn`, async, panics &
soundness, FFI, macros, performance hot paths, docs, logging — or any `M-*`
guideline id.

## Install

### Option A — as a plugin (recommended)

In an interactive Claude Code session:

```
/plugin marketplace add horacioskrp/claude-skill-rust-guidelines
/plugin install rust-guidelines@horacioskrp-skills
```

### Option B — copy the skill folder

```bash
git clone https://github.com/horacioskrp/claude-skill-rust-guidelines
cp -r claude-skill-rust-guidelines/skills/rust-guidelines ~/.claude/skills/   # all projects
# or into a project:  cp -r .../skills/rust-guidelines .claude/skills/
```

Restart your Claude Code session; the skill appears in the available-skills list.

## Layout

```
.claude-plugin/
├── plugin.json                   # plugin manifest
└── marketplace.json              # single-plugin marketplace manifest
skills/
└── rust-guidelines/
    ├── SKILL.md                  # entry point: triggers + full 90-rule checklist
    ├── LICENSE.md                # Microsoft MIT license (preserved)
    └── reference/
        ├── guidelines-full.txt   # the complete guidelines (~34k tokens, read on demand)
        └── checklist.md          # canonical grouped checklist
.github/workflows/
└── sync-upstream.yml             # weekly re-sync from microsoft/rust-guidelines
```

`SKILL.md` carries the compact checklist inline; Claude greps
`reference/guidelines-full.txt` for a specific `M-*` id when it needs the full
rationale and code examples.

## Staying up to date

`.github/workflows/sync-upstream.yml` runs weekly (and on manual dispatch) to pull
the latest `all.txt` and checklist from `microsoft/rust-guidelines@main`, and commits
to `main` only when the content changed.

## Attribution & license

Guidelines content © Microsoft Corporation, licensed **MIT** (see
[`skills/rust-guidelines/LICENSE.md`](skills/rust-guidelines/LICENSE.md)). This
repository only re-packages the project's own agent-oriented bundle
(`src/agents/all.txt`) as a Claude Code plugin/skill. Upstream:
<https://github.com/microsoft/rust-guidelines>.
