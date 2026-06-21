# Claude Code Skills

[![skills.sh](https://skills.sh/b/warot-a/skills)](https://skills.sh/warot-a/skills)

A repository of custom skills for [Claude Code](https://claude.com/claude-code), Anthropic's command-line IDE for Claude.

## Skills Included

### `teach-framework`

A specialized skill that teaches new frameworks through hands-on, project-based learning.

**Purpose:** Learn frameworks like NestJS, Next.js, and others by building real projects, not just reading documentation.

**How it works:**

- The skill guides you step-by-step through building a real mini-project
- Each step teaches one core concept with concrete code examples
- You're never given incomplete code — only tested snippets and precise placement instructions
- The skill never runs commands or edits files for you; you do the work while it coaches

**Supported frameworks:**

- **NestJS** — Backend REST APIs with modules, controllers, services, and guards
- **Next.js** — Full-stack React apps with file-based routing and server components
- **Generic frameworks** — Other frameworks by following their official getting-started docs

**When to use this skill:**

- You say "teach me NestJS" or "learn Next.js"
- You want to build a real project while learning, not just follow a tutorial
- You prefer learning by doing, guided by an expert coach
- Type: `/teach-framework` (or use the skill selector in Claude Code)

---

### `github-release`

A skill that guides you through the full release workflow end-to-end.

**Purpose:** Bump version, commit, tag, push, and publish a GitHub release with a formatted changelog — all in one flow.

**How it works:**

1. Asks bump type (patch / minor / major), release mode (draft or publish), and source code preference
2. Runs `pnpm version` to bump `package.json`, commit, and tag automatically
3. Pushes commit and tag with `git push --follow-tags`
4. Collects commits since the last tag and lets you pick which ones to include in release notes
5. Categorises selected commits into New Features, Bug Fixes, and Chores
6. Creates the GitHub release via `gh release create`

**When to use this skill:**

- You say "release", "publish version", or "สร้าง release"
- Type: `/github-release` (or use the skill selector in Claude Code)

---

### `git-stage-message`

A skill that generates a conventional commit message from staged files without committing.

**Purpose:** Get a ready-to-use `feat:/fix:/chore:/etc.` commit message from whatever you've staged with `git add`.

**How it works:**

1. Inspects `git diff --cached` to see what's staged
2. Determines the commit type, optional scope, and a short imperative subject
3. Outputs a single conventional commit message line

**When to use this skill:**

- You ask "what should my commit message be?"
- You say "generate commit message" or "suggest message for staged files"
- Type: `/git-stage-message` (or use the skill selector in Claude Code)

---

## Getting Started

### Installation

Install this skill package from the repository:

```bash
pnx skills@latest add warot-a/skills
```

Or using npm:

```bash
npx skills@latest add warot-a/skills
```

### Using This Skill in Claude Code

1. **CLI:** Run `claude-code` with `/teach-framework` skill

   ```bash
   claude
   ```

   Then type: `/teach-framework`

2. **IDE Extensions:** Available in VS Code, JetBrains, and other supported IDEs

### How the Teaching Process Works

When you invoke the skill:

1. The agent asks which framework and version you want to learn
2. You describe the project you want to build (e.g., "a Todo API", "a blog with comments")
3. The agent confirms you have the prerequisite tools
4. You get a full roadmap showing all steps ahead
5. For each step:
   - **Concept:** What you're learning and why it matters
   - **Command:** The exact command to run
   - **Code:** Copy-paste ready snippets with precise placement (file path, line number, or class)
   - **Verification:** You confirm it worked before moving to the next step

## Core Design Principles

- **Never run commands for you** — You type commands, learning as you go
- **Never auto-edit files** — You paste code, understanding what each line does
- **One concept per step** — Small, accomplishable milestones (under 5 minutes each)
- **Real projects** — Build something useful, not toy examples
- **Verified progress** — Confirm each step works before continuing

## Repository Structure

```
skills/
├── git-stage-message/
│   └── SKILL.md
├── github-release/
│   └── SKILL.md
├── teach-framework/
│   └── SKILL.md
└── README.md
```

## Framework Learning Paths

### NestJS Learning Path

**Prerequisites:** Node.js, npm/yarn

**Concept Order:**

1. Module system (app organization)
2. Controllers (HTTP endpoints)
3. Services (business logic)
4. DTOs & Validation (type safety)
5. Guards (authentication/authorization)
6. Interceptors (cross-cutting concerns)
7. Exception filters (error handling)

**Typical starter project:** REST API (Todo, Blog, or Notes app)

### Next.js Learning Path

**Prerequisites:** Node.js, npm/yarn

**Concept Order:**

1. File-based routing (pages structure)
2. Pages vs App Router (routing systems)
3. Server Components (rendering strategy)
4. Client Components (interactivity)
5. API Routes (backend endpoints)
6. Data fetching patterns (getServerSideProps, fetch)

**Typical starter project:** Multi-page site with dynamic routes

## Tips for Success

1. **Choose a real project** — Not "hello world", something you'd actually use
2. **Take your time** — Each step should feel manageable, not rushed
3. **Ask for hints** — If stuck, ask before asking for the full solution
4. **Run the code** — After each step, actually test and verify it works
5. **Experiment** — Once a concept lands, try variations before moving on

## Resources

- [Claude Code Documentation](https://claude.com/claude-code)
- [Claude API / Anthropic SDK](https://docs.anthropic.com/)
- [NestJS Docs](https://docs.nestjs.com/)
- [Next.js Docs](https://nextjs.org/docs)
