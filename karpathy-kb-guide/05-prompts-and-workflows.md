# 05 — Prompts and Workflows: Ready-to-Use Templates

## How to Use This File

These prompts are designed for direct use with Claude, GPT-4, or Gemini.
Each prompt is a complete, self-contained template — fill in the `[BRACKETED]` sections and paste.

---

## PROMPT 1: The Compilation Prompt (Step 3)

**When to use:** When you add a new file to `raw/` and want to compile it into the wiki.

```
You are an expert research librarian maintaining a personal knowledge base wiki.

Your task: Compile the following raw source into structured wiki articles.

## Rules
- Follow the SCHEMA exactly
- Use [[WikiLink]] format for all cross-references to other wiki articles
- Be comprehensive but concise — aim for synthesis, not copy-pasting
- Every claim should be traceable to the source
- Create ALL relevant articles (e.g., a paper might need: a paper/ article + new concept/ articles)

## SCHEMA
[PASTE YOUR wiki/SCHEMA.md HERE]

## Existing Wiki Index (so you know what already exists)
[PASTE YOUR wiki/index.md HERE]

## Source File
Filename: [e.g., raw/papers/attention-is-all-you-need.pdf]
Content:
[PASTE THE SOURCE CONTENT HERE]

## Output Format
For each file to create, use:
=== FILE: wiki/[type]/[filename].md ===
[file content]
=== END FILE ===

Also output the updated index.md.
```

---

## PROMPT 2: The Q&A Agent Prompt (Step 4)

**When to use:** When you have a complex research question to answer using your wiki.

```
You are a Q&A agent for a personal knowledge base. Your job is to answer research
questions by reasoning over the wiki, then file the answer back as a new wiki article.

## Important Rules
- Base your answer primarily on the wiki articles provided
- If the wiki lacks sufficient information, say so explicitly
- After answering, write the response as a wiki/qa/ article
- List any follow-up questions your answer raises

## My Question
[YOUR RESEARCH QUESTION HERE]

## Available Wiki Articles
[PASTE RELEVANT wiki/ FILES HERE — or paste all of them if using long-context model]

## Output Format

### Part 1: The Answer
[Your comprehensive answer here]

### Part 2: Wiki Article to File
=== FILE: wiki/qa/[question-slug].md ===
# Q: [Question Title]

**Question:** [exact question]
**Filed:** [today's date]
**Sources Consulted:** [[Article 1]], [[Article 2]], ...

## Answer
[the answer]

## Sources (from raw/)
- [raw file references]

## Follow-up Questions
- [question 1]
- [question 2]
=== END FILE ===
```

---

## PROMPT 3: The Lint Prompt (Support Layer)

**When to use:** Weekly quality checks on your wiki.

```
You are a knowledge base quality auditor.

Analyze the following wiki articles and produce a Lint Report identifying:

1. **Critical Issues** — Things that must be fixed
   - Contradictions between articles
   - Broken [[links]] to non-existent articles
   - Missing required sections (per SCHEMA)
   
2. **Quality Improvements** — Would significantly improve the wiki
   - Articles with no backlinks (isolated notes)
   - Articles that are too thin (< 200 words)
   - Missing examples where claimed
   
3. **Structural Gaps** — Missing articles that should exist
   - Concepts referenced via [[link]] but no article exists
   - Major topics in the domain not yet covered
   - Papers mentioned but not summarized

4. **Suggested Connections** — New backlinks to add
   - Articles that should link to each other but don't

## Wiki Articles
[PASTE ALL wiki/*.md FILES HERE]

## SCHEMA (for reference)
[PASTE wiki/SCHEMA.md HERE]

Output as a structured Lint Report with severity levels:
- 🔴 Critical (fix now)
- 🟡 Important (fix this week)
- 🟢 Nice to Have (fix eventually)
```

---

## PROMPT 4: The Heal Prompt (After Lint)

**When to use:** After running the lint — to fix the issues found.

```
You are fixing issues identified in a knowledge base wiki audit.

Here are the lint issues to address:
[PASTE LINT REPORT HERE]

Here are the current wiki articles:
[PASTE AFFECTED wiki/ ARTICLES]

Please:
1. Fix all 🔴 Critical issues
2. Fix as many 🟡 Important issues as possible
3. Create any missing articles identified as structural gaps

Output each fixed/created file using:
=== FILE: wiki/[type]/[filename].md ===
[updated content]
=== END FILE ===
```

---

## PROMPT 5: The Index Build/Refresh Prompt

**When to use:** When your index.md is out of date or you want a comprehensive refresh.

```
You are rebuilding the master index for a knowledge base wiki.

Scan all the wiki articles below and create a comprehensive, well-organized index.md.

The index should:
- Group articles by type (Concepts, Papers, People, Q&A, Topics)
- Provide a one-line description for each article
- Sort within each group alphabetically
- Include statistics (total articles, total words estimate)
- Include a "Recently Added" section (flag articles with no "Updated:" frontmatter)

## Wiki Articles
[PASTE ALL wiki/ FILES OR LIST THEM]

Output:
=== FILE: wiki/index.md ===
[comprehensive new index]
=== END FILE ===
```

---

## PROMPT 6: The Slide Generator Prompt (Output Step)

**When to use:** You want to turn wiki content into a Marp presentation.

```
Convert the following wiki articles into a Marp presentation slide deck.

Topic: [YOUR TOPIC]
Audience: [e.g., PhD seminar, conference talk, team meeting]
Length: [e.g., 15 minutes, ~20 slides]

Marp Rules:
- Use --- between slides
- Use # for slide titles
- Keep text minimal (3-5 bullet points per slide max)
- Include a title slide and conclusion slide
- Reference sources where appropriate

## Wiki Articles to Draw From
[PASTE RELEVANT wiki/ ARTICLES]

Output a complete .md file compatible with Marp:
=== FILE: output/[topic]-slides.md ===
---
marp: true
theme: default
paginate: true
---

# [Title]
## [Subtitle]

[Your Name] | [Date]

---
[slides...]
=== END FILE ===
```

---

## PROMPT 7: New Source Batch Processing

**When to use:** You've accumulated 5-10 new raw files and want to process them all at once.

```
You are batch-compiling multiple new sources into a personal knowledge base wiki.

For each source:
1. Identify the appropriate article type (concept/, paper/, person/, etc.)
2. Create the wiki article following the SCHEMA
3. Identify connections to existing wiki articles (backlinks)
4. List any new concepts that need their own articles

After processing all sources, provide:
- Updated index.md
- A list of all created files
- Recommended next steps (gaps to fill, articles to refine)

## SCHEMA
[PASTE SCHEMA.md]

## Existing Wiki Index
[PASTE index.md]

## New Sources to Process

### Source 1: [filename]
[content]

### Source 2: [filename]
[content]

[... add more ...]
```

---

## Daily Workflow Shortcuts

### Morning Clip (< 1 minute)
When you find an interesting article online:
1. Click Obsidian Web Clipper
2. Set folder to `raw/articles/`
3. Done — it's in the queue

### Evening Compile (5-10 minutes)
1. Open terminal: `python scripts/compile.py raw/articles/[today's-clips]`
2. Save outputs to `wiki/`
3. Open Obsidian — browse new wiki articles

### Weekly Ritual (30 minutes)
```bash
# 1. Lint the wiki
python scripts/lint.py

# 2. Review the report
# 3. Fix critical issues
# 4. Check index.md growth — this should feel satisfying!
```

---

## Next Step

Learn how to adapt this for your PhD research specifically.

→ Continue to `06-adapt-for-phd-research.md`
