# 06 — Adapting Karpathy's System for Your PhD Research

## Your Existing System + Karpathy's = The Ultimate Research Setup

You already have a sophisticated research workflow. This isn't a replacement — it's an **upgrade layer** that sits on top.

### Mapping Your Current System

| Your Current System | Karpathy's Layer | Combined Result |
|---|---|---|
| `7 - Papers/` (PDFs) | `raw/papers/` | Same folder — just add wiki compilation |
| `2 - Notes/Papers/` (scaffolded notes) | `wiki/papers/` | Merge — wiki becomes smarter notes |
| `3 - Tags/` (tag notes) | `wiki/concepts/` | Upgrade tags to full concept articles |
| `@[/prime]` command (AI synthesis) | Compilation prompt | Same idea, more structured |
| `@[/concept-snowballer]` | Lint + connection suggestions | Same idea, more proactive |
| Clippings/ | `raw/articles/` | Same folder |
| Tweets/ | `raw/tweets/` | Add to raw/ pipeline |
| `6 - Writings/` | Output step | Wiki → papers/chapters |

---

## The PhD-Specific SCHEMA

Create `wiki/SCHEMA.md` with this PhD-focused version:

```markdown
# PhD Research Wiki Schema

## Purpose
This is a curated, LLM-maintained knowledge base for PhD research in [YOUR FIELD].
Domain focus: [e.g., Urban Planning, Land Policy, Eminent Domain]

## Article Types

### concept/
Core theoretical and empirical concepts in the domain.
Required:
- **Definition**: What this concept is
- **Theoretical Grounding**: Which theories explain it
- **Empirical Evidence**: What the data shows
- **Debates**: Contested aspects of this concept
- **My Position**: Where my research stands
- **Relevant Papers**: [[wiki links to paper summaries]]
- **Chapter Relevance**: Which thesis chapter(s) this feeds

### paper/
Summaries of academic papers.
Required:
- **Citation**: APA format (pull from Zotero)
- **Research Question**: What they asked
- **Method**: How they studied it
- **Key Findings**: Numbers + qualitative results
- **Limitations**: What they couldn't answer
- **Relevance to My Research**: How this informs my thesis
- **Quotes Worth Saving**: Exact quotes for potential citations
- **Connections**: [[links]] to related concepts and papers

### debate/
Ongoing academic debates in the field.
Required:
- **The Debate**: What's contested and why
- **Side A**: Position + key proponents + best papers
- **Side B**: Position + key proponents + best papers
- **My View**: Where my research positions itself
- **Evidence Gap**: What empirical evidence is missing

### evidence/
Empirical data points, case studies, statistics.
Required:
- **Finding**: The specific data/observation
- **Source**: [[paper]] or dataset
- **Context**: When/where this applies
- **My Interpretation**: How this informs my argument
- **Cautions**: Limitations of this evidence

### chapter/
Draft outlines and planning for thesis chapters.
Required:
- **Chapter Goal**: What this chapter argues
- **Outline**: H2/H3 structure
- **Key Concepts**: [[concept links]] that anchor the chapter
- **Key Papers**: [[paper links]] most central to argument
- **Status**: Draft/In Progress/Complete
- **Open Questions**: What I still need to figure out

### qa/
Research questions answered by the wiki.
Required:
- **Question**: The exact research question
- **Answer**: Comprehensive response
- **Sources**: [[wiki links]] to evidence
- **Confidence**: High/Medium/Low
- **Filed**: Date
- **Implications**: What this means for my thesis

## Backlinking Rules
- Every paper/ must link to at least 2 concept/ articles
- Every concept/ must link to at least 3 paper/ references  
- Every chapter/ must link to all relevant concept/ and evidence/ notes
- Debates link to the papers arguing each side

## Index Structure
Group by: Concepts | Papers by Chapter | Debates | Evidence | Chapters | Q&A
```

---

## Upgrade Your Paper Ingestion Workflow

### Before (Your Current Workflow)
1. Drop PDF → `7 - Papers/`
2. Run `@[/rename-paper]` to rename
3. Run `@[/scaffold]` to create note template
4. Read paper and annotate
5. Run `@[/prime]` to generate synthesis
6. Manually link to concept notes
7. Run `@[/concept-snowballer]` (occasionally)

### After (Karpathy's Layer Added)
1. Drop PDF → `7 - Papers/` (same)
2. Run `@[/rename-paper]` (same)
3. Run `@[/scaffold]` → creates `2 - Notes/Papers/paper-name.md` (same)
4. **NEW: Run compile prompt → creates `wiki/papers/paper-name.md`** (Karpathy step)
5. Read paper and annotate (same — human judgment still critical)
6. Run `@[/extract]` to pull highlights (same)
7. **NEW: Run wiki update prompt to enrich the paper summary with your annotations**
8. **NEW: `wiki/concepts/` get auto-updated with new evidence from this paper**

### The Key Addition
The wiki layer means that when you add Paper #47, it **automatically** gets connected to all the concepts in Papers #1-46 that exist in the wiki. No manual snowballing needed.

---

## The Evidence Aggregator: Replacing Concept Snowballer

Your `@[/concept-snowballer]` manually propagates evidence from papers to concept notes. Karpathy's system automates this via the wiki.

**New approach: Evidence → Concept auto-aggregation**

After each paper compilation, run this prompt:

```
I've just added a new wiki article for [PAPER NAME]. 
It mentions these concepts: [[concept1]], [[concept2]], [[concept3]]

For each concept, please:
1. Open the existing concept article
2. Add the new evidence/finding from this paper
3. Update the "Empirical Evidence" section
4. Add a [[link]] to the new paper

Existing concept articles:
[PASTE wiki/concepts/concept1.md etc.]

New paper article:
[PASTE wiki/papers/new-paper.md]

Output: Updated versions of each concept article.
```

---

## Answering Your Research Questions

One of the most powerful uses: asking complex research questions against your entire literature base.

**Example workflow:**

You're writing Chapter 2 on Eminent Domain. You want to know:
*"What does the literature say about fair compensation standards across different jurisdictions?"*

**Old approach:** Search Zotero, re-read 5 papers, manually synthesize.

**New approach:**
```
Q&A Agent Prompt:
"Based on my wiki, summarize what the literature says about 
fair compensation standards across jurisdictions in eminent domain cases.
Include: key debates, empirical findings, jurisdictional variations, and gaps."

[Paste wiki/concepts/fair-compensation.md,
     wiki/papers/[relevant papers],
     wiki/debates/compensation-standards.md]
```

The agent synthesizes across your entire literature instantly. File the answer as `wiki/qa/compensation-standards-comparison.md`. When you write Chapter 2, this Q&A article becomes a draft section.

---

## Building Towards Your Thesis Chapters

The most exciting use: using the wiki to drive chapter writing.

### Step 1: Create chapter plan articles

For each thesis chapter, create `wiki/chapter/chapter-2-eminent-domain.md`:
```markdown
# Chapter 2: Eminent Domain and Fair Compensation

**Goal:** Argue that current compensation standards systematically undervalue...
**Status:** Outline

## Outline
- 2.1 Historical evolution of taking doctrine
- 2.2 Current compensation frameworks
- 2.3 Empirical evidence on valuation gaps
- 2.4 Normative framework for reform

## Key Concepts
- [[fair-compensation]]
- [[just-compensation-doctrine]]
- [[eminent-domain-power]]

## Anchor Papers
- [[kelo-v-city-of-new-london-analysis]]
- [[economic-analysis-takings]]

## Open Questions
- Does [[fair market value]] capture subjective losses?
- How do [[procedural protections]] vary by jurisdiction?
```

### Step 2: Let the wiki answer your open questions

Run Q&A sessions for each open question in the chapter plan.
File answers back into `wiki/qa/`.
The Q&A articles become first drafts of chapter subSections.

### Step 3: Generate draft prose

```
Based on the following wiki articles covering Chapter 2 of my PhD thesis
on [TOPIC], draft a 2,000-word section on [SUBSECTION].

Use academic prose suitable for a PhD thesis.
Integrate the evidence and arguments from the wiki into a coherent narrative.
Every claim must be traceable to a [[source]] in the wiki.

[PASTE RELEVANT WIKI ARTICLES]
```

---

## Your Integrated System Diagram

```
INPUTS:
Papers (PDFs) → 7-Papers/  ─────────┐
Web Articles → Clippings/  ─────────┼──► raw/ (source of truth)
Tweets/threads → Tweets/   ─────────┘       │
                                             │ [Compile]
                                             ▼
                                        wiki/ ◄──── Lint/Heal (weekly)
                                             │
                            ┌────────────────┴──────────────┐
                            ▼                               ▼
                     Q&A Agent                      Obsidian Graph
                       │                               (navigate)
                       │ [file back]
                       ▼
                    wiki/qa/ ──────────────────── Chapter drafts
                                                   6-Writings/
```

---

## Next Step

Tools, plugins, and configuration needed to set this up.

→ Continue to `07-tools-and-setup.md`
