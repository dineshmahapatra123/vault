# 01 — The Karpathy Concept: How LLMs Turn Raw Research Into a Living Knowledge Base

## The Problem He Was Solving

Most knowledge workers face a version of this:
- You save 50 articles, 20 papers, 10 YouTube transcripts.
- They sit in folders, never getting connected.
- You re-read the same source twice because you forgot you had it.
- You can't "query" your own notes intelligently.
- RAG (Retrieval-Augmented Generation) helps, but it re-derives meaning every time — it doesn't *learn*.

Traditional note-taking systems (Zettelkasten, PARA, etc.) solve this through **manual curation**. Karpathy's insight: **make the LLM do the curating**.

---

## The Core Insight

> *"You never write the wiki. The LLM writes everything. You just steer — every answer compounds."*
> — Andrej Karpathy

This one sentence encodes a profound shift:

| Old Mindset | Karpathy's Mindset |
|---|---|
| I read → I take notes → I organize | I collect sources → LLM organizes → I query |
| The wiki is written by me | The wiki is written by the LLM |
| Knowledge stays static | Every query grows the wiki |
| You are the librarian | You are the library director |

---

## Why Karpathy Built This

Karpathy is a researcher (ex-OpenAI, Tesla AI). He reads enormous amounts of:
- ML papers
- Code repositories
- Blog posts
- Datasets and dataset documentation

He needed a system that could:
1. **Absorb raw material at scale** (hundreds of sources)
2. **Keep knowledge connected** (backlinks, categories, cross-references)
3. **Answer complex questions** against his entire reading history
4. **Grow smarter** — not just store information

His solution: use an LLM as an **active research librarian**, not a passive search engine.

---

## The Philosophical Shift: From Retrieval to Compilation

Traditional RAG asks: *"What relevant chunks can I find for this query?"*

Karpathy's system asks: *"What does my curated, synthesized knowledge base say about this?"*

The difference:
- RAG works on **raw chunks of text** — every query re-derives meaning from scratch
- The wiki works on **pre-synthesized knowledge** — the LLM has already done the thinking

Think of it like this:
- **RAG** = searching a library full of unread books
- **Karpathy's Wiki** = consulting an expert who has read all those books and synthesized them

---

## The "Compound Knowledge" Effect

The most powerful insight: **every interaction improves the system**.

When you ask a question → the LLM:
1. Consults the wiki
2. Synthesizes an answer
3. **Files the answer back into the wiki** as a new article

Next time you ask a related question, the wiki already has a synthesized answer to build on.

This is *compounding knowledge*. Just like compounding interest — the longer you use the system, the smarter it gets.

---

## Who This Is For

This system is ideal if you:
- Research intensively (academics, PhD students, journalists, analysts)
- Consume large amounts of information and need to synthesize it
- Have an existing Obsidian setup (or are willing to set one up)
- Want a "second brain" that actually thinks, not just stores
- Are comfortable using LLM APIs (Claude, GPT-4, Gemini)

---

## What This Is NOT

- ❌ This is NOT a simple note-taking app
- ❌ This is NOT a chatbot that answers questions from PDFs
- ❌ This is NOT another RAG pipeline
- ✅ This IS an active, self-improving, LLM-curated knowledge graph stored as Markdown files

---

## Next Step

Now that you understand the *why*, let's go deep on the *how*.

→ Continue to `02-architecture-deep-dive.md`
