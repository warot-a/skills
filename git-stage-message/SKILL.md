---
name: git-stage-message
description: Generate a conventional commit message (feat:/fix:/chore:/etc.) from staged files without committing. Use when the user wants a commit message for staged changes, asks "what should my commit message be", or says "generate commit message", "suggest message for staged files".
---

# Git Stage Message

Generate a conventional commit message from staged files (`git add`) without committing.

## Workflow

1. **Inspect staged changes**
   ```bash
   git diff --cached
   ```

2. **Analyze the diff** — determine:
   - **Type**: pick from the table below based on what changed
   - **Scope** (optional): the module, component, or file area affected
   - **Subject**: short imperative description of what the change does

3. **Output the message**
   ```
   <type>(<scope>): <subject>
   ```
   Examples:
   - `feat(auth): add OAuth2 login flow`
   - `fix(api): handle null response from payment endpoint`
   - `chore(deps): upgrade react to v19`

## Type Selection

| Type     | When to use |
|----------|-------------|
| feat     | New feature or capability visible to users |
| fix      | Bug fix |
| refactor | Code restructure with no behaviour change |
| chore    | Tooling, deps, config, CI, non-src changes |
| style    | Formatting, whitespace, lint fixes only |
| docs     | Documentation only |
| test     | Adding or fixing tests |
| perf     | Performance improvement |
| revert   | Reverting a previous commit |

## Rules

- Subject is imperative, lowercase, no trailing period ("add X", not "Added X.")
- Keep subject under 72 characters
- If staged changes touch multiple unrelated concerns, suggest splitting into separate commits
