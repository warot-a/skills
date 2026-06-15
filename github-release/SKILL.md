---
name: github-release
description: 'Guides the user through the full release workflow: bumping version, committing, tagging, pushing, and creating a GitHub release with changelog. Use when the user wants to create a release, publish a version, or says "สร้าง release", "release", "publish version".'
---

# GitHub Release

## Full Workflow

### Step 1 — Bump version & options

Ask the user (use AskUserQuestion tool) three questions at once:

- **Bump type** (single select): `patch` · `minor` · `major`
  - patch = bug fixes (1.0.0 → 1.0.1)
  - minor = new features, backwards-compatible (1.0.0 → 1.1.0)
  - major = breaking changes (1.0.0 → 2.0.0)
- **Release mode** (single select): `Draft ก่อน` · `Publish ทันที`
- **Source code** (single select): `ไม่อัพโหลด source code (default)` · `อัพโหลด source code`

Run:
```bash
pnpm version <patch|minor|major>
```

This bumps `package.json`, creates a git commit, and creates a git tag automatically.

### Step 2 — Push

```bash
git push --follow-tags
```

`--follow-tags` pushes both the commit and the version tag.

### Step 3 — Collect release notes from git log

Run the following to get commits since the previous tag:

```bash
git log $(git describe --tags --abbrev=0 HEAD^)..HEAD --oneline
```

If there is no previous tag, use all commits: `git log --oneline`.

Present the commits as a **multi-select** question using `AskUserQuestion`. Each commit should be one option with:
- `label`: the commit subject (short message)
- `description`: the commit hash

Ask: "เลือก commits ที่จะใส่ใน release notes (เลือกได้หลายอัน)"

### Step 4 — Build release notes

From the selected commits, Claude categorises each into one of:
- **✨ New Features** — commits that add new functionality
- **🐛 Bug Fixes** — commits that fix bugs
- **🔧 Chores / Maintenance** — everything else (refactors, style, docs, etc.)

Format (skip sections with no items):

```markdown
## ✨ New Features
- <item>

## 🐛 Bug Fixes
- <item>

## 🔧 Chores / Maintenance
- <item>
```

Rewrite each commit message into a clean, human-readable sentence (title case, no issue numbers unless meaningful).

### Step 5 — Create GitHub release

Read the new version from `package.json` after the bump.

Write release notes to a temp file to avoid shell escaping issues:

```bash
cat > /tmp/release_notes.md << 'EOF'
<release notes>
EOF
```

Build the `gh release create` command:
- Always include: `--title "v{version}" --notes-file /tmp/release_notes.md`
- Draft flag: add `--draft` if user chose "Draft ก่อน"
- Source code: by default GitHub auto-attaches source archives — to **disable** them is not possible via `gh` CLI, so omit any `--assets` flags and just note to the user that source archives are attached by GitHub automatically. If user chose "ไม่อัพโหลด source code", inform them that GitHub always attaches source archives and no extra assets will be uploaded.

Run:
```bash
gh release create "v{version}" --title "v{version}" --notes-file /tmp/release_notes.md [--draft]
```

### Step 6 — Report

Print the release URL from the command output.

## Error handling

- If `pnpm` is not available, try `npm version` instead.
- If `gh` CLI is not installed or not authenticated, show the error and stop.
- If `git push` fails (e.g. diverged branch), show the error and ask the user to resolve before continuing.
- If there are no commits since the last tag, inform the user and ask if they want to continue anyway.
