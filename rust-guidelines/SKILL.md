---
name: rust-guidelines
description: >-
  Pragmatic Rust Guidelines (Microsoft) — 90 design rules for writing and
  reviewing idiomatic, scalable Rust. Use when writing, designing, refactoring,
  or reviewing ANY Rust code: crate/workspace layout, public API design, error
  handling (thiserror/anyhow), naming, modules & visibility, builders, traits
  vs generics vs dyn, async, panics & soundness/unsafe, FFI, macros,
  performance hot paths, docs, logging/telemetry, and AI-friendly API design.
  Triggers on "idiomatic Rust", "Rust API design", "review my Rust", Cargo
  workspace structure, or any M-* guideline id.
---

# Pragmatic Rust Guidelines

A collection of 90 pragmatic design guidelines (Microsoft) for producing
idiomatic Rust that scales — selected to be beneficial for **safety, cost of
goods sold (COGs), and maintenance**, agreeable to experienced Rust developers,
and comprehensible to novices.

Source: <https://microsoft.github.io/rust-guidelines> · Licensed MIT (see `LICENSE.md`).

## How to use this skill

1. **Writing or refactoring Rust** → follow the relevant guidelines below. When a
   choice is non-obvious (error type shape, builder vs constructor, trait vs
   generic, module split, API signature), look up the specific guideline.
2. **Reviewing Rust** → walk the checklist below for the code's category
   (library / application / FFI / macro / perf-critical) and flag violations by
   their `M-*` id so the author can look them up.
3. **Need the full rationale, code examples, and nuance** for a guideline → read
   the bundled reference file and search for the `M-*` id:
   - `reference/guidelines-full.txt` — the **complete** guidelines (~34k tokens).
     Don't load it wholesale; grep/search it for the specific `M-*` id(s) you need.
   - `reference/checklist.md` — the canonical grouped checklist with links.

Cite guidelines by id (e.g. "prefer `From` over `map_err`, **M-FROM-ERROR**") so
they can be verified against the reference.

## Foundational principles

- **M-RUST-SHAPED** — solve Rust problems with Rust patterns; don't transliterate
  Java/Go/C++ designs. (Especially relevant when porting code from another language.)
- **M-DESIGN-FOR-AI** — idiomatic, well-documented, strongly-typed APIs are easier
  for both humans and agents; lean on the type system and the compiler.
- **M-UPSTREAM-GUIDELINES** — these complement, and defer to, the official
  [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/).

## Checklist (all 90 guidelines)

### Universal
- [ ] Follow the upstream guidelines (M-UPSTREAM-GUIDELINES)
- [ ] Use static verification (M-STATIC-VERIFICATION)
- [ ] Lint overrides should use `#[expect]` (M-LINT-OVERRIDE-EXPECT)
- [ ] Public types are `Debug` (M-PUBLIC-DEBUG)
- [ ] Public types meant to be read are `Display` (M-PUBLIC-DISPLAY)
- [ ] If in doubt, split the crate (M-SMALLER-CRATES)
- [ ] Names are free of weasel words (M-WEASEL-WORDS)
- [ ] Names of items are short (M-SHORT-NAMES)
- [ ] Prefer regular over associated functions (M-REGULAR-FN)
- [ ] Magic values are documented (M-DOCUMENTED-MAGIC)
- [ ] Use structured logging with message templates (M-LOG-STRUCTURED)

### Library / Interoperability
- [ ] Types are `Send` (M-TYPES-SEND)
- [ ] Native escape hatches (M-ESCAPE-HATCHES)
- [ ] Don't leak external types (M-DONT-LEAK-TYPES)
- [ ] Items come from their original crate (M-FOREIGN-REEXPORTS)
- [ ] Accept `impl AsRef<>` where feasible (M-IMPL-ASREF)
- [ ] Accept `impl RangeBounds<>` where feasible (M-IMPL-RANGEBOUNDS)
- [ ] Accept `impl 'IO'` where feasible ('sans IO') (M-IMPL-IO)

### Library / UX
- [ ] Abstractions don't visibly nest (M-SIMPLE-ABSTRACTIONS)
- [ ] Avoid smart pointers and wrappers in APIs (M-AVOID-WRAPPERS)
- [ ] Prefer types over generics, generics over `dyn` traits (M-DI-HIERARCHY)
- [ ] Errors are canonical structs (M-ERRORS-CANONICAL-STRUCTS)
- [ ] Canonical error conversion uses `From`, not `map_err` (M-FROM-ERROR)
- [ ] Complex type construction has builders (M-INIT-BUILDER)
- [ ] Complex type initialization hierarchies are cascaded (M-INIT-CASCADED)
- [ ] Services are `Clone` (M-SERVICES-CLONE)
- [ ] Essential functionality should be inherent (M-ESSENTIAL-FN-INHERENT)
- [ ] Modules are balanced in size and scope (M-BALANCED-MODULES)
- [ ] Don't define preludes (M-NO-PRELUDE)
- [ ] Parameter ordering is consistent (M-PARAMETER-CONSISTENCY)
- [ ] Collections implement the appropriate iter traits (M-COLLECTION-TRAITS)
- [ ] Functions are `async` over returning a `Future` (M-ASYNC-FN)

### Library / Resilience
- [ ] I/O and system calls are mockable (M-MOCKABLE-SYSCALLS)
- [ ] Test utilities are feature gated (M-TEST-UTIL)
- [ ] Integration tests live under `tests/` (M-INTEGRATION-TESTS)
- [ ] Integration test utilities live in a separate crate (M-INTEGRATION-TEST-UTILS)
- [ ] Use the proper type family (M-STRONG-TYPES)
- [ ] Newtypes guard their invariants (M-STRONG-TYPES-GUARD)
- [ ] Builders validate in final `.build()` (M-BUILD-RESULT)
- [ ] Don't glob re-export items (M-NO-GLOB-REEXPORTS)
- [ ] Avoid statics (M-AVOID-STATICS)
- [ ] Production code uses telemetry, not `println` (M-LOG-NOT-PRINT)

### Library / Building
- [ ] Libraries work out of the box (M-OOBE)
- [ ] Native `-sys` crates compile without dependencies (M-SYS-CRATES)
- [ ] Features are additive (M-FEATURES-ADDITIVE)

### Macros
- [ ] Macros are a last resort (M-MACRO-LAST-RESORT)
- [ ] Prefer 'macros by example' over proc macros (M-EXAMPLE-OVER-PROC)
- [ ] Macros don't lie about signatures (M-MACROS-DONT-LIE)
- [ ] Macros assume main crate (M-MACRO-MAIN-CRATE)
- [ ] Third party items come from hidden `_private` module (M-MACRO-HELPERS)
- [ ] Proc macros should have separate impl crate incl. tests (M-PROC-IMPL)
- [ ] Proc macros don't produce implied or hidden items (M-PROC-IMPLIED-ITEMS)

### Applications
- [ ] Use mimalloc for apps (M-MIMALLOC-APPS)
- [ ] Applications may use Anyhow or derivatives (M-APP-ERROR)
- [ ] Applications target highest viable `target-cpu` (M-TARGET-CPU)

### FFI
- [ ] Isolate DLL state between FFI libraries (M-ISOLATE-DLL-STATE)
- [ ] Business logic belongs in core crates, FFI only translates (M-FFI-TRANSLATES)
- [ ] FFI crates follow established naming conventions (M-FFI-NAMING)

### Correctness
- [ ] Unsafe needs reason, should be avoided (M-UNSAFE)
- [ ] Unsafe implies undefined behavior (M-UNSAFE-IMPLIES-UB)
- [ ] All code must be sound (M-UNSOUND)
- [ ] Panic means 'stop the program' (M-PANIC-IS-STOP)
- [ ] Detected programming bugs are panics, not errors (M-PANIC-ON-BUG)
- [ ] Panic continuation is last resort (M-PANIC-CONTINUATION)
- [ ] Custom panics have a helpful message (M-PANIC-MESSAGE)

### Performance
- [ ] Optimize for throughput, avoid empty cycles (M-THROUGHPUT)
- [ ] Identify, profile, optimize the hot path early (M-HOTPATH)
- [ ] Long-running tasks should have yield points (M-YIELD-POINTS)
- [ ] Reuse allocations where possible (M-MEM-REUSE)
- [ ] Library telemetry does not tank performance (M-LOG-OVERHEAD)
- [ ] Nested type hierarchies should avoid needless indirection (M-AVOID-INDIRECTION)
- [ ] Use boxed slices and strings for immutable owned sequences (M-BOX-DST)
- [ ] Shrink collections to fit after building (M-SHRINK-TO-FIT)
- [ ] Use a fast hasher where possible (M-FAST-HASHER)
- [ ] Collections are created with sufficient initial capacity (M-INITIAL-CAPACITY)
- [ ] Hot `async` functions reduce stack size (M-ASYNC-STACK-SIZE)

### Project
- [ ] Common settings come from the workspace `Cargo.toml` (M-CARGO-WORKSPACE)
- [ ] The workspace lists and versions all crates (M-CRATES-IN-WORKSPACE)
- [ ] All crates are siblings in one folder (M-CRATES-FLAT-FOLDER)
- [ ] New crates target latest edition (M-LATEST-EDITION)
- [ ] MSRV is conservatively updated (M-MSRV)

### Documentation
- [ ] First sentence is one line; approx. 15 words (M-FIRST-DOC-SENTENCE)
- [ ] Has comprehensive module documentation (M-MODULE-DOCS)
- [ ] Documentation has canonical sections (M-CANONICAL-DOCS)
- [ ] Mark `pub use` items with `#[doc(inline)]` (M-DOC-INLINE)

### AI
- [ ] Design with AI use in mind (M-DESIGN-FOR-AI)
- [ ] Items are only visible through one path (M-SINGLE-ITEM-PATH)
- [ ] Avoid meta design documentation (M-NO-META-DESIGN-DOCUMENTATION)
- [ ] Tests do not assert ground truth (M-TAUTOLOGICAL-TESTS)
- [ ] Rust code solves Rust problems (M-RUST-SHAPED)

---

*Guidelines content © Microsoft Corporation, MIT License. This skill packages the
project's own agent-oriented bundle (`src/agents/all.txt`) for use in Claude Code
sessions. Upstream: <https://github.com/microsoft/rust-guidelines>.*
