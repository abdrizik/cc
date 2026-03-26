---
name: cc
description: >
  Conventional Commits helper for git workflow. Analyzes diffs to suggest
  branch names, commit messages, PR titles, and squash messages following
  the Conventional Commits v1.0.0 spec. Also validates commit messages
  and helps write revert commits. Use when creating commits, naming branches,
  generating PR descriptions, validating commits, squashing commits, or when
  user says "commit", "branch", "validate", "squash", "pr title", "revert",
  "conventional commit", "cc".
argument-hint: "[commit|branch|validate|squash|pr|revert]"
allowed-tools: Bash(git *)
license: CC-BY-3.0
compatibility: "Requires git 2.x+"
metadata:
  author: craftful
  version: 0.1.0
---

# Conventional Commits Helper

Based on [Conventional Commits v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/).

## Dispatch

- `$ARGUMENTS` is `commit` → **Commit Workflow** (side effect)
- `$ARGUMENTS` is `branch` → **Branch Workflow** (side effect)
- `$ARGUMENTS` is `validate` → **Validate Workflow** (read-only)
- `$ARGUMENTS` is `squash` → **Squash Workflow** (read-only)
- `$ARGUMENTS` is `pr` → **PR Workflow** (read-only)
- `$ARGUMENTS` starts with `revert` → **Revert Workflow** (side effect)
- `$ARGUMENTS` is empty → **Full Workflow**

Should NOT trigger on: general git questions, git blame/log, repo setup, git configuration.

---

## Commit Workflow

1. Run `git status` and `git diff --staged`.
2. **Staged changes exist**:
   - Analyze the diff. Determine type and scope.
   - Generate: `<type>[scope]: <description>` with optional body.
   - Present in a fenced code block. Ask to confirm, adjust, or proceed.
3. **Unstaged changes only**:
   - Run `git diff`. Summarize changes, suggest what to stage, then suggest commit message.
4. **No changes**: Tell user, ask what they'd like to do.

---

## Branch Workflow

1. Run `git branch --show-current` and `git status`.
2. **On main/master/develop with changes**: Suggest `<type>/<short-kebab-description>`.
3. **On feature branch**: Check name follows convention, suggest rename if not.
4. **No changes on main**: Ask what they plan to work on, then suggest name.

---

## Validate Workflow

Read-only — checks branch commits against the spec.

1. Run `git log main..HEAD --format="%H %s"` (try `master` if `main` fails).
2. If no branch divergence, check last 10 commits.
3. For each commit subject, check:
   - Valid type prefix
   - Optional scope in parentheses
   - `: ` (colon + space) separator
   - Description present, lowercase, no trailing period
4. Report violations with short SHA. Show summary: `X of Y commits pass`.

---

## Squash Workflow

Read-only — generates a single commit message from branch commits.

1. Run `git log main..HEAD --oneline` (try `master` if `main` fails).
2. Pick the most significant type: `feat` > `fix` > `refactor` > `perf` > others.
3. Determine scope from the common area affected.
4. Generate subject + body with bullet list of individual commits.
5. Output in a fenced code block.

---

## PR Workflow

Read-only — generates PR title and description from branch commits.

1. Run `git log main..HEAD --format="%s%n%b---"` and `git diff main..HEAD --stat`.
2. Determine primary type and scope for the PR title.
3. Group commits by type for the description. Flag breaking changes.
4. Generate:
   - PR title: `<type>[scope]: <summary>` (under 72 chars)
   - PR body with Summary, Changes (grouped by type), Breaking Changes sections.
5. Output in fenced code blocks.

---

## Revert Workflow

Side effect — creates a revert commit after confirmation.

1. If `$ARGUMENTS` has a commit ref after `revert`, use it.
2. Otherwise, show recent commits and ask which to revert.
3. Generate:
   ```
   revert: <original description>

   Refs: <SHA>
   ```
4. For multiple commits, list all SHAs comma-separated in `Refs:`.
5. Present and ask to confirm.

---

## Full Workflow

1. Run `git status`, `git diff --staged`, `git branch --show-current`.
2. **Diffs exist**: If on main/master/develop → suggest branch name, then commit message. If on feature branch → suggest commit message.
3. **No diffs**: Ask what to do.

---

## Format Reference

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Types

| Type | Purpose | SemVer |
| --- | --- | --- |
| `feat` | New feature | MINOR |
| `fix` | Bug fix | PATCH |
| `docs` | Documentation only | — |
| `style` | Formatting, no logic change | — |
| `refactor` | Neither fix nor feature | — |
| `perf` | Performance improvement | — |
| `test` | Adding/correcting tests | — |
| `build` | Build system, dependencies | — |
| `ci` | CI configuration | — |
| `chore` | Other (no src/test change) | — |
| `revert` | Reverts a previous commit | — |

### Breaking changes

`!` after type/scope and/or `BREAKING CHANGE:` footer. Maps to MAJOR.

```
feat!: remove deprecated API endpoints
```
```
feat: migrate to new auth provider

BREAKING CHANGE: old JWT tokens no longer accepted
```

### Scope

Noun in parentheses: `feat(parser):`, `fix(api):`.

### Footers

Git trailer format: `token: value` or `token #value`. Use `-` for spaces in tokens except `BREAKING CHANGE`.

---

## Rules

1. Type + optional scope + optional `!` + `: ` + description
2. Imperative mood, lowercase, no trailing period
3. Subject line under 72 characters
4. Body: blank line after description, free-form
5. Footers: blank line after body, `token: value` format
6. Breaking changes: `!` and/or `BREAKING CHANGE:` footer
7. One logical change per commit — suggest splitting if needed

---

## Troubleshooting

**Commit rejected by linter**: Check type is lowercase, space after colon, no trailing period.

**Multiple types fit**: Split into separate commits. Primary intent determines type.

**Wrong type used**: Before merging, use `git rebase -i` to edit history.

---

## Reference

- [spec.md](spec.md) — Full Conventional Commits v1.0.0 specification (all 16 requirements)
- [examples.md](examples.md) — Good/bad examples, branch naming, workflow output patterns
