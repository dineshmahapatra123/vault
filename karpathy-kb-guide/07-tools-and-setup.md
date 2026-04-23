# 07 — Tools, Plugins, and Setup

## Overview: What You Need

| Layer | Tool | Cost | Purpose |
|---|---|---|---|
| Frontend | Obsidian | Free | View and navigate wiki |
| Clipping | Obsidian Web Clipper | Free | Send web content to raw/ |
| LLM | Claude API (Anthropic) | Pay-per-use | Compile, Q&A, Lint |
| Slides | Marp | Free | Markdown → Presentation |
| Search | Omnisearch (Obsidian plugin) | Free | Full-text search across vault |
| Scripting | Python 3.10+ | Free | Automation scripts |
| Visualization | Matplotlib/Seaborn | Free | Charts from data |

---

## Obsidian Setup (You Already Have This)

### Essential Plugins for This Workflow

**Install these via Settings → Community Plugins:**

1. **Obsidian Web Clipper**
   - Install the browser extension: chrome.google.com/webstore → "Obsidian Web Clipper"
   - Configure: Vault = your vault, Default folder = `raw/articles`
   - Keyboard shortcut: Cmd+Shift+C (customize to taste)

2. **Omnisearch**
   - Replaces default search with full-text, fuzzy search
   - Works across both `raw/` and `wiki/` simultaneously
   - Shortcut: Cmd+Shift+O

3. **Dataview**
   - Lets you query your wiki like a database
   - Example: `TABLE file.ctime FROM "wiki/papers" SORT file.ctime DESC` → shows recently added papers
   - Essential for building dynamic tables in your index.md

4. **Templater**
   - More powerful than core templates
   - Create templates for each wiki article type (concept, paper, qa, etc.)
   - Triggers on file creation to auto-fill frontmatter

5. **Marp Slides** (optional, for presentations)
   - Renders Marp slide decks from Markdown directly in Obsidian
   - Install: Community Plugins → "Marp Slides"

6. **Graph Analysis** (optional)
   - Advanced graph view showing link density, isolated notes
   - Helps identify articles that need more connections

---

## Obsidian Configuration

### Configure Attachments (Critical!)
*Settings → Files & Links → Default location for new attachments*

Set to: `raw/assets`

This means any image you paste or drag into Obsidian goes directly into `raw/assets/` — making it accessible to the LLM.

### Configure New Note Location
*Settings → Files & Links → Default location for new notes*

Set to: `1 - Rough/` (your existing inbox)
This way new notes don't end up scattered.

### File Naming (Keep Your Existing Convention)
Your existing sentence-case naming convention is fine. For wiki files, use kebab-case:
- `wiki/concepts/fair-compensation.md` ✓
- `wiki/papers/why-nations-fail-acemoglu.md` ✓

---

## Obsidian Web Clipper Template

Create this as your default clipping template:

```
---
source: "{{url}}"
clipped: "{{date:YYYY-MM-DD}}"
title: "{{title}}"
author: "{{author}}"
status: unprocessed
---

# {{title}}

> Clipped from: {{url}}
> Author: {{author}}
> Date clipped: {{date:YYYY-MM-DD}}

---

{{content}}
```

When you clip an article, it instantly becomes a raw/ file ready for compilation.

---

## LLM Setup: Claude API

### Why Claude?
- **200K token context window** — can process your entire wiki at once
- **Excellent at synthesis** — produces coherent, well-structured wiki articles
- **Citation-accurate** — less prone to hallucination in structured document tasks
- **Cost-effective** — Claude Haiku (~$0.25/1M tokens) for routine tasks; Sonnet for complex Q&A

### Setup
```bash
# Install the Anthropic Python library
pip install anthropic

# Set your API key (add to your .zshrc or .bashrc)
export ANTHROPIC_API_KEY="sk-ant-..."

# Test it works
python -c "import anthropic; print(anthropic.Anthropic().models.list())"
```

### Cost Estimates
A typical compilation of one paper (~15 pages, ~6,000 tokens):
- Input: ~8,000 tokens (paper + schema + index) = ~$0.024 with Claude Sonnet
- Output: ~2,000 tokens (wiki articles) = ~$0.012
- **Total per paper: ~$0.036 (~4 cents)**

For 100 papers: ~$3.60 total. Extremely cheap.

### Alternative: Local LLMs (Free, Private)
If you want to keep your research private:

```bash
# Install Ollama
brew install ollama

# Pull a capable model
ollama pull llama3.3:70b  # best open-source model for this task

# Run it
ollama serve
```

Then modify the compile script to use Ollama:
```python
import ollama
response = ollama.chat(model='llama3.3:70b', messages=[{'role': 'user', 'content': prompt}])
```

**Trade-off:** Local models are slower and slightly less capable, but 100% free and private.

---

## Python Environment Setup

```bash
# Navigate to your vault
cd "/Users/dineshmahapatra/Downloads/Dinesh/Vault"

# Your .venv already exists — activate it
source .venv/bin/activate

# Install required packages
pip install anthropic python-dotenv pathlib watchdog

# Create a .env file for secrets (NEVER commit this to git)
echo "ANTHROPIC_API_KEY=your-key-here" > .env
echo ".env" >> .gitignore
```

---

## The Scripts Directory

Create these scripts in your vault:

```
scripts/
├── compile.py       ← compile raw/ file → wiki/ articles
├── lint.py          ← scan wiki for quality issues
├── qa.py            ← interactive Q&A against wiki
├── stats.py         ← wiki statistics and growth tracking
└── batch_compile.py ← process multiple new files at once
```

### `scripts/stats.py` — Track Your Wiki Growth

```python
#!/usr/bin/env python3
"""Show statistics about your knowledge base."""
from pathlib import Path
import json
from datetime import datetime

VAULT = Path("/Users/dineshmahapatra/Downloads/Dinesh/Vault")
WIKI = VAULT / "wiki"

def count_stats():
    articles = list(WIKI.rglob("*.md"))
    total_words = sum(len(f.read_text().split()) for f in articles)
    by_type = {}
    for a in articles:
        t = a.parent.name
        by_type[t] = by_type.get(t, 0) + 1
    
    print(f"\n📚 Knowledge Base Statistics — {datetime.now().strftime('%Y-%m-%d')}")
    print(f"{'─' * 40}")
    print(f"Total articles: {len(articles)}")
    print(f"Total words: {total_words:,}")
    print(f"\nBreakdown by type:")
    for t, count in sorted(by_type.items(), key=lambda x: -x[1]):
        print(f"  {t:15} {count} articles")

if __name__ == "__main__":
    count_stats()
```

---

## Marp: Markdown → Professional Slides

### Install Marp CLI
```bash
npm install -g @marp-team/marp-cli
```

### Generate a Presentation
```bash
# Convert a wiki article to slides
marp wiki/topics/my-topic-slides.md --output output/my-talk.html

# Generate PDF
marp wiki/topics/my-topic-slides.md --pdf --output output/my-talk.pdf

# Preview in browser (live reload)
marp --watch wiki/topics/my-topic-slides.md --preview
```

### Marp Slide Template
```markdown
---
marp: true
theme: default
paginate: true
style: |
  section {
    font-family: 'Georgia', serif;
    font-size: 28px;
  }
  h1 { color: #2c3e50; }
---

# Your Title Here
## Subtitle

Your Name | Conference | Date

---

# Introduction

- Point 1
- Point 2
- Point 3

> "Key quote from your research"

---
<!-- and so on -->
```

---

## Keyboard Shortcuts to Set Up in Obsidian

| Action | Shortcut |
|---|---|
| Open Web Clipper | Cmd+Shift+W |
| Omnisearch | Cmd+Shift+F |
| Open Graph View | Cmd+Shift+G |
| Open wiki/index.md | Cmd+Shift+I (set via Templater) |
| New note in raw/articles/ | Cmd+Shift+N |

---

## Next Step

Where this all goes in the future — fine-tuning on wiki data.

→ Continue to `08-future-vision.md`
