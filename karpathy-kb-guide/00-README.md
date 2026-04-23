# Karpathy's LLM Knowledge Base: A Complete Learning Guide

> **This folder is temporary** — delete it once you've internalized the concepts and built your system.

## What Is This?

This is a step-by-step learning guide to understand, replicate, and extend **Andrej Karpathy's LLM-powered Personal Knowledge Base** system — as shared on X (formerly Twitter) in April 2026.

The core idea: **You never write the wiki. The LLM writes everything. You just steer — every answer compounds.**

---

## Files In This Guide

| File | What You'll Learn |
|------|-------------------|
| `01-karpathy-concept.md` | The idea, the philosophy, and why it matters |
| `02-architecture-deep-dive.md` | Full breakdown of all 5 pipeline steps + support layer |
| `03-how-it-differs-from-rag.md` | Why this beats traditional RAG systems |
| `04-build-your-own-step-by-step.md` | Concrete implementation in YOUR Obsidian vault |
| `05-prompts-and-workflows.md` | Ready-to-use prompts for each pipeline step |
| `06-adapt-for-phd-research.md` | How to integrate this with your existing PhD cyborg workflow |
| `07-tools-and-setup.md` | Tools, plugins, and configuration needed |
| `08-future-vision.md` | Where this goes next: fine-tuning on wiki data |

---

## How to Read This Guide

**Sequential reading** (best for first-timers):
→ 01 → 02 → 03 → 04 → 05 → 06 → 07 → 08

**Build-first readers:**
→ Start at `04-build-your-own-step-by-step.md`, refer back as needed.

---

## The Core Mental Model

```
You (Curator) → raw/ folder → LLM (Compiler) → wiki/ (Knowledge Base) → You (Querier)
                                    ↑                      |
                                    └──── filed back ───────┘
                                         (knowledge compounds)
```

Every time you ask the system a question, the answer gets filed back into the wiki. The wiki grows smarter over time — automatically.

---

*Generated: April 2026 | Based on Karpathy's tweet (https://x.com/karpathy/status/2039805659525644595)*
