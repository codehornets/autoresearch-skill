# Contributing to Autoresearch

Thanks for wanting to make autoresearch better. Whether you're fixing a typo in a reference file, adding a new sub-command, or improving the loop protocol, this guide will get you up and running fast.

## Quick Start

Autoresearch skills are Markdown files that Claude Code discovers from a `skills/` directory. No build step, no compilation — edit a `.md` file, invoke the skill, see your changes.

```bash
# 1. Clone the repo
git clone https://github.com/uditgoenka/autoresearch.git
cd autoresearch

# 2. Copy to your local Claude Code skills (for testing)
cp -r skills/autoresearch ~/.claude/skills/autoresearch

# 3. Edit any file, then re-copy to test
#    Or symlink for live editing (see below)
```

### Live Editing with Symlinks

For faster iteration, symlink instead of copying:

```bash
# Remove the copy
rm -rf ~/.claude/skills/autoresearch

# Symlink your working tree
ln -s $(pwd)/skills/autoresearch ~/.claude/skills/autoresearch

# Now edits to skills/autoresearch/* take effect immediately in Claude Code
```

When you're done developing, replace the symlink with a stable copy:

```bash
rm ~/.claude/skills/autoresearch
cp -r skills/autoresearch ~/.claude/skills/autoresearch
```

## Repository Structure

```
autoresearch/
├── README.md                                    <- Project documentation
├── CONTRIBUTING.md                              <- You are here
├── LICENSE                                      <- MIT License
└── skills/
    └── autoresearch/
        ├── SKILL.md                             <- Main skill file (loaded by Claude Code)
        └── references/
            ├── autonomous-loop-protocol.md      <- 8-phase loop protocol
            ├── core-principles.md               <- 7 universal principles
            ├── plan-workflow.md                  <- /autoresearch:plan wizard
            ├── security-workflow.md             <- /autoresearch:security audit
            └── results-logging.md               <- TSV tracking format
```

### What Each File Does

| File | Purpose | Edit when... |
|------|---------|-------------|
| `SKILL.md` | Main entry point Claude Code reads. Contains sub-command routing, setup phase, loop pseudocode, and domain table. | Adding new sub-commands, changing activation triggers, updating the loop behavior. |
| `references/autonomous-loop-protocol.md` | Detailed 8-phase loop protocol with rules for each phase. | Changing how the loop works (review, ideate, modify, commit, verify, guard, decide, log, repeat). |
| `references/core-principles.md` | 7 universal principles from Karpathy's autoresearch. | Refining the principles or adding new ones. |
| `references/plan-workflow.md` | `/autoresearch:plan` wizard protocol. | Changing the planning wizard flow, adding new question types, updating metric suggestions. |
| `references/security-workflow.md` | `/autoresearch:security` audit protocol. | Adding OWASP checks, new red-team personas, updating report structure, adding flags. |
| `references/results-logging.md` | TSV log format and reporting rules. | Changing log columns, summary format, or reporting intervals. |
| `README.md` | Public documentation with examples, commands table, and usage guide. | Adding examples, updating the commands table, documenting new features. |

## What to Contribute

### High-Value Contributions

| Type | Examples | Difficulty |
|------|----------|-----------|
| **New domain examples** | DevOps, data science, design, mobile | Easy |
| **Verification script templates** | Reusable scripts for common metrics | Easy |
| **Bug fixes** | Loop edge cases, incorrect behavior | Medium |
| **New sub-commands** | `/autoresearch:refactor`, `/autoresearch:test` | Medium |
| **OWASP check additions** | New security checks for `/autoresearch:security` | Medium |
| **Protocol improvements** | Better stuck-detection, smarter ideation | Hard |
| **MCP integration patterns** | Database, API, analytics verification | Hard |

### Low-Value (Please Don't)

- Reformatting or restructuring existing files without functional changes
- Adding comments to explain obvious things
- Changing naming conventions or code style

## Day-to-Day Workflow

```bash
# 1. Fork the repo and clone your fork
gh repo fork uditgoenka/autoresearch --clone
cd autoresearch

# 2. Create a feature branch
git checkout -b feat/your-feature-name

# 3. Symlink for live testing
ln -s $(pwd)/skills/autoresearch ~/.claude/skills/autoresearch

# 4. Make your changes
vim skills/autoresearch/references/autonomous-loop-protocol.md

# 5. Test in Claude Code — changes are live via symlink
#    Open Claude Code in any project and invoke /autoresearch

# 6. Commit with conventional format
git add -A
git commit -m "feat: add guard rework timeout after 60 seconds"

# 7. Push and create PR
git push -u origin feat/your-feature-name
gh pr create --title "feat: your feature" --body "## Summary\n- What changed\n- Why"
```

## Commit Messages

We use [conventional commits](https://www.conventionalcommits.org/):

| Prefix | When |
|--------|------|
| `feat:` | New feature or sub-command |
| `fix:` | Bug fix in existing behavior |
| `docs:` | Documentation-only changes (README, examples, comments) |
| `refactor:` | Restructuring without behavior change |
| `chore:` | Maintenance (CI, config, tooling) |

Examples:
```
feat: add /autoresearch:refactor sub-command
fix: guard rework attempts not decrementing correctly
docs: add Docker image optimization example to README
```

## Pull Request Guidelines

1. **One PR = one feature.** Don't bundle unrelated changes.
2. **Branch from `master`.** Target `master` as base.
3. **Descriptive title.** Use conventional commit format in the PR title.
4. **Write a good body.** Explain what changed, why, and how to test it.
5. **Update README.** If you add a feature, add it to the Commands Reference table and include usage examples.
6. **Update SKILL.md.** If you add a sub-command, register it in the subcommands table and activation triggers.
7. **Bump the version.** Use semver: patch for fixes (1.0.x), minor for features (1.x.0).
8. **Keep files focused.** Don't modify files unrelated to your change.

### PR Template

```markdown
## Summary
- What changed and why

## Files Changed
| File | Change |
|------|--------|
| `SKILL.md` | ... |
| `README.md` | ... |

## How to Test
1. Copy skill to ~/.claude/skills/autoresearch
2. Run /autoresearch in a project
3. Verify [specific behavior]
```

## Adding a New Sub-Command

If you're adding a new sub-command (like `/autoresearch:plan` or `/autoresearch:security`), follow this pattern:

### 1. Create the reference file

```
skills/autoresearch/references/your-workflow.md
```

This file contains the full protocol — phases, rules, examples, error recovery.

### 2. Register in SKILL.md

Add to the subcommands table:

```markdown
| `/autoresearch:yourcommand` | Description of what it does |
```

Add the sub-command section with:
- `Load: references/your-workflow.md` directive
- Quick summary (numbered steps)
- Usage examples
- Key behaviors

### 3. Update activation triggers

In the "When to Activate" section of SKILL.md:

```markdown
- User invokes `/autoresearch:yourcommand` → run your workflow
- User says "relevant trigger phrases" → run your workflow
```

### 4. Update README

- Add to Commands Reference table (with version)
- Add to Quick Decision Guide
- Add a dedicated section with examples
- Update the Repository Structure tree
- Add FAQ entries if relevant

### 5. Bump version

Update `version:` in SKILL.md frontmatter.

## Testing Your Changes

There's no automated test suite — autoresearch is a skill (Markdown instructions), not code. Testing means using it:

1. **Symlink your working tree** (see Quick Start above)
2. **Open Claude Code in a real project**
3. **Invoke the skill** (`/autoresearch`, `/autoresearch:plan`, etc.)
4. **Verify behavior matches your changes**
5. **Try edge cases** — what happens when the metric is wrong? When scope matches 0 files?

### What to Check

- Does Claude follow your updated instructions?
- Does the output format match what you specified?
- Are error cases handled gracefully?
- Does backward compatibility hold? (Existing commands still work)

## Things to Know

- **No build step.** Everything is Markdown. Edit → test → commit.
- **SKILL.md is the entry point.** Claude Code reads this first. References are loaded on demand via `Load:` directives.
- **References are lazy-loaded.** Only loaded when the relevant sub-command is invoked. This keeps context usage low.
- **Version in frontmatter matters.** Claude Code uses this for skill identification.
- **README is the public face.** Keep it updated — it's what users see on GitHub.
- **The repo is MIT licensed.** Your contributions will be under the same license.

## Getting Help

- **Questions?** Open an [issue](https://github.com/uditgoenka/autoresearch/issues)
- **Ideas?** Open an issue with `[Idea]` prefix
- **Bug reports?** Open an issue with reproduction steps
- **Discussion?** Tag [@uditgoenka](https://github.com/uditgoenka) in your PR

Thanks for contributing!
