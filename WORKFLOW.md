# KB Workflow — How to Compile New Sources

This file tells the LLM exactly what to do when new content is added to `raw/`.
Paste this file + `wiki/SCHEMA.md` + `wiki/index.md` at the start of any session.

---

## When to Use This

Use this file whenever you say:
- "Compile this new article"
- "I added a paper to raw/"
- "Do the needful with this file"

---

## The Compilation Instruction (for the LLM)

You are maintaining a personal reading knowledge base. The human has added a new file to `raw/`. Your job is to compile it into the `wiki/` following the SCHEMA.

### Step 1: Identify what to create
Based on the source, decide which wiki article types are needed:
- A news article or op-ed → 1 `topics/` article + any new `concepts/` + any new `people/`
- An essay (like Orwell) → `people/` for the author + multiple `concepts/` 
- An academic paper → `papers/` summary + new `concepts/` it introduces
- A repo/README → `topics/` covering what the project does + relevant `concepts/`

### Step 2: Write the wiki articles
Follow `wiki/SCHEMA.md` exactly.
- Use `[[WikiLink]]` format for all cross-references
- Each article must link to at least 2 others
- Check `wiki/index.md` — link to articles that already exist

### Step 3: Update `wiki/index.md`
Add every new article to the correct section in the index.
Update the Statistics block (article count, date, source count).

### Step 4: Confirm
List every file created and where it was saved.

---

## How to Start a New Session

Paste these three things to the LLM at the start:
1. This file (`WORKFLOW.md`)
2. `wiki/SCHEMA.md`
3. `wiki/index.md`

Then say: **"Compile raw/[type]/[filename]"**

That's all. The LLM will do the rest.

---

## How to Ask a Q&A Question

Paste the relevant `wiki/` articles to the LLM and say:
"Answer this question using my wiki: [your question]"

The LLM will answer and create a `wiki/qa/[question].md` file.
You save that file to `wiki/qa/`.

---

## Folder Reference

| Folder | Purpose |
|---|---|
| `raw/articles/` | Clipped articles, essays, op-eds |
| `raw/papers/` | Academic PDFs |
| `raw/repos/` | GitHub READMEs |
| `raw/assets/` | Images, diagrams |
| `raw/datasets/` | Data files |
| `wiki/concepts/` | Core ideas |
| `wiki/papers/` | Paper summaries |
| `wiki/people/` | Notable thinkers |
| `wiki/topics/` | Running threads |
| `wiki/qa/` | Answered questions |
