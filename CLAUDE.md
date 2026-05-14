# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a **pure documentation repository** — there is no build system, no test runner, and no package dependencies. The "product" is a set of four behavioral guidelines for AI coding assistants, derived from Andrej Karpathy's observations on LLM coding pitfalls.

## Architecture: Three Synchronized Files

The core content (the four principles) is distributed across three files that must stay in sync whenever the principles change:

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Per-project usage via Claude Code's `CLAUDE.md` convention |
| `.cursor/rules/karpathy-guidelines.mdc` | Cursor IDE project rule (has YAML frontmatter with `alwaysApply: true`) |
| `skills/karpathy-guidelines/SKILL.md` | Claude Code plugin skill (has YAML frontmatter with `name` and `description`) |

The plugin manifest files (`.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`) define how this repo is distributed as a Claude Code plugin. They point to `skills/karpathy-guidelines/` and do not contain principle content.

**When editing the four principles:** update all three content files above. The frontmatter in `.mdc` and `SKILL.md` should not be changed unless metadata (name/description) is being updated.

## Installation Methods (for end users, context for contributors)

- **Plugin**: `/plugin marketplace add forrestchang/andrej-karpathy-skills` then `/plugin install andrej-karpathy-skills@karpathy-skills`
- **Per-project**: `curl -o CLAUDE.md https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md`

## Behavioral Guidelines

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
