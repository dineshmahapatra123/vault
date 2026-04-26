---
type: Note
_organized: true
---

# Vault Constitution

This is the primary source of truth for the vault's conventions, structure, and agent instructions. All AI agents (Antigravity, Claude, etc.) must follow these rules exactly.

## Core Conventions

- **One Markdown note per file.**
- **The first H1** in the body is the preferred display title.
- Store note type in the `type:` frontmatter field.
- Most standalone notes live at the **vault root** as flat `.md` files.
- Any frontmatter field containing [[wikilinks]] is treated as a relationship (e.g., `belongs_to`, `related_to`, `has`).
- Use **kebab-case** for filenames: `my-note-title.md`.

## Vault Structure

| Location | Purpose |
|---|---|
| Vault root (flat `.md`) | Standalone notes, personal contacts (People) |
| `Getting Started/` | Type definitions and foundational documentation |
| `project-name/` | A project and all its directly related notes (3+ notes) |
| `raw/` | Reading inbox — uncompiled source material |
| `wiki/` | Compiled knowledge base — LLM-maintained |

## Project Folders

When a project has 3 or more directly related notes, create a dedicated folder:
- The main project note lives **inside** the folder (e.g., `project-name/project-name.md`).
- Folder name = kebab-case version of the project title.
- People notes stay at the vault root — they are shared across projects.

## Note Types

| Type | File | Purpose |
|---|---|---|
| **Project** | `Getting Started/project.md` | Goal-oriented work with a clear outcome |
| **Note** | `Getting Started/note.md` | General notes, ideas, captures |
| **Topic** | `Getting Started/topic.md` | Running threads across reading/thinking |
| **Person** | `Getting Started/person.md` | Personal contacts (root) or intellectual references (`wiki/`) |
| **Learning** | `Getting Started/learnings.md` | Insights and lessons captured from experience |
| **Event** | `Getting Started/event.md` | Time-bound events, meetings, occasions |
| **Wiki Note** | `Getting Started/wiki-note.md` | Compiled KB articles in `wiki/` (concepts, papers, etc.) |

## People Convention

- **Personal contacts**: Live at the vault root. Simple structure: role, context, relationship.
- **Intellectual references**: Live in `wiki/people/`. Full SCHEMA: Known For, Their Lens, Notable Pieces, My Take.
- Do not create `wiki/people/` notes for people the user personally knows.

## Wiki Knowledge Base

The `wiki/` folder is maintained by the AI agent using the `/compile-kb` command.
- Follow `wiki/SCHEMA.md` exactly.
- Every article ends with a `## My Take` section — leave this blank for the user.
- Update `wiki/index.md` after every compilation.

## Agent Guidelines

### What agents should do:
- Create and edit notes using the frontmatter and H1 conventions above.
- Create new type definitions in `Getting Started/` as flat `.md` files. Add them to the types table in this file.
- For projects with 3+ related notes, create a project folder and place all notes inside it.
- Keep People notes at root for personal contacts; use `wiki/people/` only for intellectual references from reading.
- Add or modify relationships without breaking existing wikilinks.
- Create and edit saved views in `views/`.

### What agents should avoid:
- Do not place project-related notes (budgets, events, plans) inside `wiki/` — that folder is for reading synthesis only.
- Do not create `wiki/people/` notes for people the user personally knows.
- Do not fill in `## My Take` sections — those always belong to the user.
- Do not overwrite user-authored config or installation-specific app files unless explicitly asked.

---

# Specialized Agent Prompts

Use these prompt structures when performing specific tasks in this vault.

## For starting or replicating a project:

Read these files first:
1. `AGENTS.md` — vault conventions
2. `april-financial-plan/april-financial-plan.md` — project structure to learn from
3. `april-financial-plan/` — scan all files in this folder

Then: create a new project called [NAME] following the same structure. 
Area: [AREA]. People involved: [NAMES]. Sub-notes needed: [LIST].

## For continuing work on an existing project:

Read these files first:
1. `AGENTS.md` — vault conventions
2. [project-folder]/[project-name].md — main project note
3. All files inside [project-folder]/

Understand: what is this project, what's done, what's pending, who's involved. Then help with: [SPECIFIC TASK].

## For KB compilation work:

Read these files first:
1. `AGENTS.md` — vault conventions
2. `WORKFLOW.md` — compile process
3. `wiki/SCHEMA.md` — article structure rules
4. `wiki/index.md` — what already exists

Then compile: `raw/articles/[FILENAME]`

**The Principle**: Always provide `AGENTS.md` + the specific context file. This provides the minimum necessary context for accurate agent behavior.
