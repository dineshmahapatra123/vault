# 04 — Build Your Own: Step-by-Step Implementation

## Prerequisites

Before starting, make sure you have:
- [ ] Obsidian installed (https://obsidian.md) — you already have this ✓
- [ ] An LLM API key: Claude (Anthropic), GPT-4 (OpenAI), or Gemini (Google)
- [ ] Obsidian Web Clipper browser extension installed
- [ ] Python 3.10+ (for scripts) — you likely have this ✓

---

## Step 1: Set Up the Folder Structure

Inside your Obsidian vault, create this structure:

```bash
# Run from your vault root
mkdir -p raw/papers raw/articles raw/repos raw/assets raw/datasets
mkdir -p wiki/concepts wiki/papers wiki/people wiki/qa wiki/topics
touch wiki/index.md wiki/SCHEMA.md
```

**In your existing vault** (`/Users/dineshmahapatra/Downloads/Dinesh/Vault`):

Since you already have a PKM system, map Karpathy's folders to your existing ones:

| Karpathy's Folder | Your Equivalent | Notes |
|---|---|---|
| `raw/papers/` | `7 - Papers/` | PDFs already live here |
| `raw/articles/` | `Clippings/` | Web clips already here |
| `raw/assets/` | `1 - Rough/` images | Diagrams and screenshots |
| `wiki/` | New `wiki/` folder | Create this fresh |
| `wiki/concepts/` | `3 - Tags/` (repurpose) | Concept-level thinking |
| `wiki/papers/` | `2 - Notes/Papers/` | Your paper notes |
| `wiki/qa/` | New `wiki/qa/` folder | Answers filed back |

**Recommendation:** Keep your existing structure and add just the `wiki/` directory as the output destination.

---

## Step 2: Create the SCHEMA.md

This is the most important file. It tells the LLM exactly how to behave in your wiki.

Create `wiki/SCHEMA.md` with this content:

```markdown
# Wiki Schema — Knowledge Base Rules

## Purpose
This is a curated, LLM-maintained knowledge base on [YOUR DOMAIN].
The LLM writes and maintains all content. The human directs and curates sources.

## Article Types

### concept/
Core ideas in the domain.
Required sections:
- **Definition**: 2-3 sentence explanation
- **Why It Matters**: Why this concept is important
- **Key Mechanisms**: How it works
- **Examples**: 2-3 concrete examples
- **Related Concepts**: [[backlinks]] to 3+ other concepts
- **Open Questions**: Unresolved mysteries about this concept
- **Sources**: Which raw/ files informed this

### paper/
Summaries of academic papers.
Required sections:
- **One-line Summary**: The core contribution in one sentence
- **Authors + Year**: 
- **Key Contribution**: What's new/important
- **Method**: How they did it
- **Key Results**: Main findings with numbers
- **Limitations**: What the paper doesn't address
- **My Hot Take**: Why this matters for [YOUR DOMAIN]
- **Connections**: [[links]] to related papers/concepts

### person/
Notable researchers and thinkers.
Required sections:
- **Role**: Current position
- **Key Contributions**: What they're known for
- **Notable Work**: [[links]] to their papers/ideas
- **Influence**: How they've shaped the field

### qa/
Answers to research questions.
Required sections:
- **Question**: The exact question asked
- **Answer**: Comprehensive response
- **Sources Consulted**: Which wiki articles informed this
- **Filed**: Date this was created
- **Follow-up Questions**: New questions raised by this answer

## Backlinking Rules
- Always use [[WikiLink]] format for cross-references
- Every article must link to at least 3 other articles
- New concepts must be added to index.md immediately
- Use #tags for category classification

## Index Rules
- index.md must always be updated when a new article is created
- Group articles by type: Concepts, Papers, People, Q&A
- Each entry: `- [[Article Name]] — one-line description`

## Style Rules
- Write in clear, direct prose (no bullet-list overload)
- Assume a smart reader who doesn't know this specific topic
- Prefer concrete examples over abstract explanations
- Every claim should be traceable to a source in raw/
```

---

## Step 3: Create the Index

Create `wiki/index.md`:

```markdown
# Knowledge Base Index

*Auto-maintained by LLM. Last updated: {{date}}.*

## Concepts
<!-- LLM adds entries here -->

## Papers
<!-- LLM adds entries here -->

## People
<!-- LLM adds entries here -->

## Q&A Articles
<!-- LLM adds entries here -->

## Statistics
- Total articles: 0
- Total words: 0
- Last compilation: —
```

---

## Step 4: Add Your First Source to raw/

Let's do a test run. Pick ONE source to start with. Good first sources:
- A paper you've read recently (PDF)
- A web article you clipped via Obsidian Web Clipper
- A GitHub README you've been meaning to synthesize

For example, clip this Karpathy tweet thread to `raw/articles/karpathy-llm-kb-workflow.md` using the Obsidian Web Clipper.

---

## Step 5: Run the First Compilation (The Core Loop)

This is the key step. You'll prompt the LLM to compile `raw/` → `wiki/`.

**Open Claude (or your preferred LLM) and paste this prompt:**

```
You are maintaining a personal knowledge base wiki. 

Your task: Read the following raw source file and create appropriate wiki articles.

Follow these rules strictly:
1. Read wiki/SCHEMA.md first (I'll paste it below)
2. Create wiki articles in the correct subdirectory
3. Update wiki/index.md with the new articles
4. Use [[WikiLink]] format for all cross-references

SCHEMA:
[paste your SCHEMA.md content]

RAW SOURCE:
[paste the content of your raw/ file]

EXISTING WIKI INDEX:
[paste wiki/index.md]

Please create:
1. The appropriate wiki article(s) for this source
2. Updated index.md entries
3. Any new concept articles that should be created based on this source
```

Save the LLM's output as the appropriate files in `wiki/`.

---

## Step 6: Set Up Obsidian Web Clipper

The Web Clipper lets you send web articles directly into `raw/articles/` with one click.

1. Install: Chrome Web Store → "Obsidian Web Clipper"
2. Configure:
   - Vault: your vault name
   - Default folder: `raw/articles`
   - Template: include title, URL, date, and full content

**Recommended Clipper Template:**
```
---
source: {{url}}
clipped: {{date}}
title: {{title}}
---

{{content}}
```

---

## Step 7: Automate Compilation with a Script (Optional)

Once you've tested the manual flow, you can automate it with a Python script.

Create `scripts/compile.py` in your vault:

```python
#!/usr/bin/env python3
"""
kb compile — Compiles new raw/ files into wiki/ articles.
Usage: python compile.py [path_to_raw_file]
"""

import os
import sys
from pathlib import Path
import anthropic  # pip install anthropic

VAULT_ROOT = Path("/Users/dineshmahapatra/Downloads/Dinesh/Vault")
RAW_DIR = VAULT_ROOT / "raw"
WIKI_DIR = VAULT_ROOT / "wiki"
SCHEMA_FILE = WIKI_DIR / "SCHEMA.md"
INDEX_FILE = WIKI_DIR / "index.md"

def read_file(path: Path) -> str:
    return path.read_text(encoding="utf-8")

def compile_source(raw_file_path: str):
    raw_content = read_file(Path(raw_file_path))
    schema = read_file(SCHEMA_FILE)
    index = read_file(INDEX_FILE)
    
    client = anthropic.Anthropic(api_key=os.environ["ANTHROPIC_API_KEY"])
    
    prompt = f"""You are maintaining a personal knowledge base wiki.

SCHEMA (follow this strictly):
{schema}

CURRENT INDEX:
{index}

NEW RAW SOURCE TO COMPILE:
File: {raw_file_path}
Content:
{raw_content}

Tasks:
1. Create wiki article(s) for this source following the SCHEMA
2. Output an updated index.md with new entries added
3. List what files to create where

Format your response as:
## FILE: wiki/[type]/[name].md
[full file content]
---
## FILE: wiki/index.md  
[full updated index content]
---
"""
    
    message = client.messages.create(
        model="claude-opus-4-5",
        max_tokens=8192,
        messages=[{"role": "user", "content": prompt}]
    )
    
    print(message.content[0].text)

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage: python compile.py <path_to_raw_file>")
        sys.exit(1)
    compile_source(sys.argv[1])
```

**Run it:**
```bash
export ANTHROPIC_API_KEY="your-key-here"
python scripts/compile.py raw/articles/karpathy-llm-kb-workflow.md
```

---

## Step 8: Run Your First Q&A Session

Now query the wiki you've built:

**Manual Q&A (paste into Claude):**
```
You are a research assistant querying my personal knowledge base wiki.

Task: Answer the following research question by reasoning over the wiki articles I provide.
After answering, write the answer as a new wiki article in wiki/qa/ format.

WIKI ARTICLES:
[paste relevant wiki articles]

QUESTION:
[your research question]

Please:
1. Answer the question comprehensively
2. Cite which wiki articles you used
3. Output the answer as a new wiki/qa/[question-slug].md file
4. List any follow-up questions this raises
```

Save the Q&A article to `wiki/qa/[your-question].md` and update `index.md`.

---

## Step 9: Run a Lint Pass (Quality Check)

After you've built up a few wiki articles (5+), run a lint check:

```
You are a wiki quality checker.

I'll give you all my wiki articles. Your job:
1. Find inconsistencies (contradictions between articles)
2. Find isolated notes (no backlinks)  
3. Find missing information (incomplete sections)
4. Suggest new articles that are clearly missing
5. Suggest connections between articles that don't link to each other

WIKI ARTICLES:
[paste all wiki/*.md content]

Output a "Lint Report" with:
- Critical Issues (must fix)
- Suggestions (would improve wiki)
- Missing Articles (should create)
- New Connections (backlinks to add)
```

---

## Step 10: The Daily Workflow (The Habit Loop)

Once set up, your daily workflow becomes:

```
MORNING:
1. Open Obsidian
2. Clip any interesting articles → raw/articles/ (30 sec per clip)
3. Drop any new PDFs → raw/papers/ (15 sec)

EVENING (optional, 15 min):
4. Run compile on your new raw/ files
5. Browse the new wiki articles Obsidian generated
6. Ask 1-2 research questions → file answers as qa/ articles

WEEKLY (30 min):
7. Run lint pass
8. Fix critical issues
9. Review wiki/index.md — see how your knowledge has grown
```

**The compounding magic:** After 1 month, you'll have a wiki that contains the synthesis of everything you've read. After 6 months, it becomes a serious research asset.

---

## Next Step

Ready-to-use prompts for every step of the workflow.

→ Continue to `05-prompts-and-workflows.md`
