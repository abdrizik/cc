# Conventional Commits (cc)

An Agent Skill based on the [Conventional Commits v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) specification.

This skill teaches AI coding assistants (Claude Code, Codex, etc.) to generate standardized git commit messages, branch names, PR titles, and more — all following the Conventional Commits spec.

## What it covers

- Commit messages in `<type>[scope]: <description>` format
- Branch naming with `<type>/<description>` convention
- Commit message validation against the spec
- Squash commit message generation from branch commits
- PR title and description generation
- Revert commits with proper `Refs:` footer
- All 11 commit types (feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert)
- Breaking change detection with `!` and `BREAKING CHANGE:` footer
- SemVer mapping (feat → MINOR, fix → PATCH, BREAKING CHANGE → MAJOR)

## Installation

```bash
npx skills add abdrizik/cc
```

## Usage

Once installed, Claude will automatically apply Conventional Commits when creating commits or naming branches.

You can also invoke it manually:

| Command            | What it does                                                 |
| ------------------ | ------------------------------------------------------------ |
| `/cc`              | Suggests branch name + commit message based on current state |
| `/cc commit`       | Analyzes diffs, generates conventional commit message        |
| `/cc branch`       | Suggests `<type>/<description>` branch name                  |
| `/cc validate`     | Checks branch commits against the spec                       |
| `/cc squash`       | Generates squash commit message from branch commits          |
| `/cc pr`           | Generates PR title + description from branch commits         |
| `/cc revert [ref]` | Writes revert commit with `Refs:` footer                     |

## License

Based on the Conventional Commits specification, licensed under [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/).
