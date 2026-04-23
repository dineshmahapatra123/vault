# 03 — How This Differs From RAG (And Why It's Better For Research)

## What Is RAG?

RAG (Retrieval-Augmented Generation) is the most common way people use LLMs with their documents today:

```
User Query → Embed Query → Vector Search → Retrieve Top-K Chunks → LLM → Answer
```

**How RAG works:**
1. Your documents are chunked (split into ~500 token pieces)
2. Each chunk is embedded into a vector space
3. When you ask a question, your query is embedded too
4. The system finds the N most similar chunks via cosine similarity
5. Those chunks are stuffed into the LLM prompt
6. The LLM generates an answer from those chunks

**Popular RAG tools:** LlamaIndex, LangChain, Chroma, Pinecone, Weaviate

---

## The Problems With RAG

### Problem 1: Stateless Memory

Every RAG query starts from scratch. The system has no memory of what it previously answered. If you ask "What are transformers?" and later ask "How do they generalize?", the second query has no context from the first answer.

**Karpathy's solution:** Every answer gets filed back into the wiki. The system remembers everything.

### Problem 2: Chunking Destroys Context

RAG splits documents into chunks to fit in context windows. But research ideas often span entire papers. Chunking breaks the logical flow — you get fragments, not understanding.

**Karpathy's solution:** The LLM reads the entire document and writes a synthesized summary. The synthesis preserves meaning, not just text.

### Problem 3: Semantic Similarity ≠ Relevance

Vector search finds text that is *semantically similar* to your query — but similar text isn't always the most *relevant* text. A paper on "attention" might retrieve chunks about focus and concentration, not the attention mechanism.

**Karpathy's solution:** The LLM curates the wiki by meaning, not by vector proximity. The pre-indexed wiki articles are already correctly categorized.

### Problem 4: No Synthesis Across Sources

RAG retrieves chunks — it doesn't synthesize knowledge across multiple documents. If the answer requires combining insights from Paper A, Blog Post B, and Dataset C, RAG struggles.

**Karpathy's solution:** The wiki already contains synthesized articles that combine multiple sources. The Q&A agent reasons from synthesis, not raw chunks.

### Problem 5: Black Box Results

With RAG + vector embeddings, you can't easily inspect *why* a particular chunk was retrieved. The system is opaque.

**Karpathy's solution:** The wiki is plain Markdown. You can read every article, see every backlink, understand exactly how knowledge is organized. Fully auditable.

---

## Side-by-Side Comparison

| Feature | Traditional RAG | Karpathy's Wiki |
|---|---|---|
| **Storage** | Vector database | Plain Markdown files |
| **Memory** | Stateless (each query fresh) | Stateful (compounds over time) |
| **Knowledge representation** | Raw text chunks | Synthesized wiki articles |
| **Cross-source synthesis** | Poor (retrieves chunks) | Excellent (pre-synthesized) |
| **Auditability** | Opaque (vector black box) | Transparent (readable Markdown) |
| **Improvement over time** | None (requires re-embedding) | Yes (wiki grows automatically) |
| **Setup complexity** | Medium (embeddings, vector DB) | Low (files + LLM API) |
| **Human readability** | Low (chunks are fragments) | High (wiki articles are polished) |
| **Query type** | Short, factual questions | Complex, multi-hop research questions |
| **Context window use** | Top-K chunks | Full wiki (long context models) |

---

## When RAG Is Still Better

To be fair, RAG has its place:

| Use Case | Better Choice |
|---|---|
| Querying a single large document (100+ pages) | RAG |
| Real-time search over a changing corpus | RAG |
| Low-latency retrieval at scale | RAG |
| First-pass answer without curation time | RAG |
| Large enterprise document corpora (10K+ docs) | RAG |

**Karpathy's wiki is better when:**
- You are building a personal/team knowledge base over months/years
- You need synthesized understanding, not just retrieved text
- You want the system to improve with each interaction
- Your source material is medium-scale (hundreds of docs, not thousands)
- Auditability and human readability matter

---

## The "Compiler" Analogy

Karpathy uses the term "compile" intentionally. Here's why it's perfect:

| Programming | Karpathy's Wiki |
|---|---|
| Source code (.c files) | raw/ directory |
| Compiler (gcc) | LLM compilation pass |
| Executable binary | wiki/ (compiled knowledge) |
| Runtime queries | Q&A agent queries wiki |
| Debugger/linter | Lint + Heal pass |

The `raw/` files are like source code — human-readable but not optimized for querying.
The `wiki/` is like a compiled artifact — optimized for fast, intelligent access.

You don't query source code at runtime. You compile it first. Similarly, you don't query raw PDFs — you compile them into the wiki first.

---

## Why Long-Context Models Changed Everything

Traditional RAG was necessary when LLMs had 4K or 8K token context windows — you couldn't fit entire documents in the prompt.

**Modern long-context models:**
- Claude 3.5/3.7: **200K token** context window
- GPT-4o: **128K token** context window  
- Gemini 2.0: **1M token** context window

With a 200K context window, you can fit **~400 pages** of text. That's Karpathy's entire wiki in a single prompt.

This is *why the wiki approach works now but didn't work before*. The LLM can reason about your entire knowledge base, not just retrieved fragments.

---

## Next Step

You now understand the *what* and *why*. Let's build it.

→ Continue to `04-build-your-own-step-by-step.md`
