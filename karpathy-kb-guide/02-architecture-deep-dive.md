# 02 — Architecture Deep Dive: All 5 Pipeline Steps + Support Layer

## Reading the Diagram

Karpathy's system has two layers:
- **Main Pipeline** (read left to right): Sources → raw/ → WIKI → Q&A Agent → Output
- **Support Layer** (below): Obsidian, Lint+Heal, CLI Tools

Everything flows *into* the wiki, and the wiki continuously improves via feedback loops.

---

## MAIN PIPELINE

### STEP 1: Sources (What You Feed In)

**What it is:** The raw material you want to learn from.

**Types of sources Karpathy uses:**
| Source Type | Examples |
|---|---|
| Articles | Blog posts, news, documentation |
| Papers | ArXiv PDFs, conference papers |
| Repos | GitHub code and READMEs |
| Datasets | CSV files, JSON datasets |
| Images + Diagrams | Architecture screenshots, charts |

**How they get into the system:**
- **Obsidian Web Clipper** (browser extension) — clips web articles directly to Markdown
- Manual drag-and-drop of PDFs into the `raw/` folder
- Scripts that pull GitHub READMEs
- Copy-paste of important content

**Key Rule:** Sources are **immutable**. The LLM reads but **never modifies** files in `raw/`.

---

### STEP 2: raw/ (The Sacred Inbox)

**What it is:** A folder where all unprocessed source files live, stored as-is.

**Directory structure:**
```
raw/
├── papers/
│   ├── attention-is-all-you-need.pdf
│   └── gpt4-technical-report.pdf
├── articles/
│   ├── karpathy-llm-os.md         ← clipped from web
│   └── andrej-neural-nets.md
├── repos/
│   └── llama.cpp-readme.md
├── datasets/
│   └── openwebtext-stats.csv
└── assets/                        ← images referenced in notes
    ├── transformer-arch.png
    └── karpathy-workflow.png
```

**Philosophy of `raw/`:**
- Think of it as your **inbox** — nothing is processed, nothing is organized
- Every new piece of knowledge starts here
- It's the "source of truth" — if the wiki ever gets corrupted, you can rebuild from `raw/`
- Images are stored locally (not as external URLs) so the LLM can reference them

---

### STEP 3: WIKI — The Heart of the System

**What it is:** A structured Markdown knowledge base written and maintained by the LLM.

**Scale (Karpathy's wiki):**
- ~100 articles
- ~400K words
- Auto-maintained index

**Structure of the wiki:**
```
wiki/
├── index.md                      ← master index (auto-maintained)
├── SCHEMA.md                     ← rules the LLM must follow
├── concepts/
│   ├── transformer-architecture.md
│   ├── attention-mechanism.md
│   └── tokenization.md
├── people/
│   ├── andrej-karpathy.md
│   └── yoshua-bengio.md
├── papers/
│   ├── attention-is-all-you-need-summary.md
│   └── gpt4-review.md
└── qa/
    └── why-does-llm-hallucinate.md ← answers filed back from Q&A
```

**What the LLM generates for each article:**
1. **Summary** — concise synthesis of the source
2. **Key Concepts** — extracted as wiki links (`[[concept]]`)
3. **Backlinks** — references to other wiki articles
4. **Categories** — topic tags for navigation
5. **Questions Raised** — open questions for future exploration

**The SCHEMA.md file:**
This is a crucial config file. It tells the LLM:
- What structure to use for each article type
- How to create backlinks
- What categories exist
- How to handle conflicts or duplicates

Example SCHEMA.md excerpt:
```markdown
## Article Types
- `concept/`: Core ideas in the domain. Must include: Definition, Examples, Related Concepts.
- `paper/`: Paper summaries. Must include: Authors, Year, Key Contribution, Limitations.
- `person/`: Notable figures. Must include: Role, Key Works, Connections to domain.
- `qa/`: Answers to questions. Must include: Question, Answer, Sources, Date.

## Backlink Rules
- Always use [[WikiLink]] format for cross-references
- Every article must link to at least 2 others
- New concepts must be added to index.md
```

---

### STEP 4: Q&A Agent

**What it is:** An LLM that answers complex questions by querying the FULL wiki.

**Why this is different from standard chatbots:**
- Standard chatbot: queries based on your current conversation context
- Q&A Agent: queries against your *entire curated knowledge base* — all 400K words

**The key distinction — no RAG needed:**
Because the wiki is already structured and synthesized (by the LLM in Step 3), the Q&A agent doesn't need to do chunking and vector search. It can reason about the entire wiki at once (using long-context LLMs like Claude with 200K token windows).

**Example Q&A interactions:**
```
You: "What does my research say about why transformers generalize better than RNNs?"

Agent: [reads wiki/concepts/transformer-architecture.md,
        wiki/concepts/recurrent-networks.md,
        wiki/papers/attention-is-all-you-need-summary.md]
       → Synthesizes a comprehensive answer
       → Files the answer as wiki/qa/transformer-vs-rnn-generalization.md
```

**The compound effect:** That answer is now part of the wiki. Next related question builds on it.

---

### STEP 5: Output (What You Get)

**Types of outputs the system can produce:**

| Output Type | Format | Use Case |
|---|---|---|
| Summary articles | Markdown (.md) | Knowledge retention |
| Presentation slides | Marp format | Conferences, teaching |
| Data visualizations | Matplotlib plots | Research papers |
| Reports | Long-form Markdown | Comprehensive analysis |

**Marp for slides:**
Marp is a Markdown-to-slides converter. Because the wiki is already in Markdown, the LLM can produce presentation-ready content with zero reformatting work.

---

## SUPPORT LAYER

### Obsidian (IDE Frontend)

**Role:** The human-facing interface to view, navigate, and understand the wiki.

**Why Obsidian specifically:**
- Local-first: all files are on your machine, not in a cloud database
- Standard Markdown: no vendor lock-in
- **Graph view**: visualizes backlink connections across your entire wiki
- **Backlinks panel**: shows what other notes reference any given note
- **Marp plugin**: renders slides directly from Markdown
- **Web Clipper**: sends web content straight to `raw/`

**The "IDE" analogy:**
- Obsidian = the code editor (where humans read)
- LLM = the programmer (where writing happens)
- Wiki = the codebase (the artifact being built)

---

### Lint + Heal

**Role:** Automated quality control for the wiki.

**What linting finds:**
- ❌ Inconsistent data (contradictions between articles)
- ❌ Missing information (empty sections, no backlinks)
- ❌ Outdated content (articles that haven't been updated after new sources)
- ❌ Isolated notes (articles with no connections to others)
- ✅ Suggestions for new articles to write
- ✅ Web searches to fill gaps in knowledge

**How it works:**
The LLM scans the wiki directory and produces a "health report" — a list of issues and suggested fixes. You can then run a "heal" pass where the LLM fixes the issues automatically.

**Analogy:** Think of it like `eslint` for code, but for your knowledge base.

---

### CLI Tools

**Role:** Programmatic interface to the wiki and LLM system.

**What CLI tools do:**
- 🔍 Search engine over the wiki (full-text + semantic)
- 🌐 Web UI for direct queries without needing Obsidian open
- 🤖 CLI interface to run LLM operations (compile, lint, Q&A)
- 📊 Stats about the wiki (article count, word count, coverage)

**Example CLI commands (vibe-coded by Karpathy):**
```bash
# Add a new source and compile it into the wiki
kb add paper.pdf

# Ask a question
kb ask "What are the main failure modes of transformers?"

# Lint the wiki
kb lint

# Generate slides on a topic
kb slides "Attention Mechanisms" --output talk.md
```

---

## The Feedback Loop (Why It Compounds)

```
Sources → raw/ → [LLM Compiles] → wiki/
                                     ↓
                              Q&A Agent queries wiki
                                     ↓
                              Answer filed back → wiki/
                                     ↑
                              Lint scans wiki
                                     ↓
                              Inconsistencies fixed
                                     ↓
                             Wiki improves → wiki/ (↑ quality)
```

Every cycle: more sources in, more synthesis out, more answers filed back. The wiki is a **living organism** that grows smarter every day you use it.

---

## Next Step

Now you understand the architecture. Let's understand why this beats RAG.

→ Continue to `03-how-it-differs-from-rag.md`
