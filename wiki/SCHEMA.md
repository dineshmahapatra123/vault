# Wiki Schema — Reading Knowledge Base

## Purpose
This is a curated, LLM-maintained knowledge base for general reading:
news, op-eds, articles, and papers across any domain of interest.

The LLM compiles and synthesizes all wiki content from sources in raw/.
Dinesh adds sources to raw/, writes in raw/notes/, and fills in "My Take" sections.

**Voice Rule:** The `## My Take` section in every article is written by Dinesh, not the LLM.
The LLM must leave this section blank with a placeholder. Never fill it in.

---

## Article Types

### concepts/
Core ideas drawn from reading — political, economic, scientific, philosophical.

Required sections:
- **Definition**: What this concept is in 2-3 sentences
- **Why It Matters**: Why this is worth knowing
- **Key Arguments**: Main positions or tensions around it
- **Examples**: 2-3 concrete real-world examples
- **Related Concepts**: [[WikiLinks]] to 3+ related concepts
- **Sources**: Which raw/ files informed this
- **My Take**: *(Dinesh writes this — leave blank with placeholder)*

### papers/
Summaries of academic papers and long-form research.

Required sections:
- **One-line Summary**: The core finding in one sentence
- **Authors + Year**:
- **Key Contribution**: What's new or important
- **Method**: How they studied it
- **Key Findings**: Main results
- **Limitations**: What it doesn't answer
- **Why I Saved This**: *(Dinesh writes this — leave blank with placeholder)*
- **My Take**: *(Dinesh writes this — leave blank with placeholder)*
- **Connections**: [[links]] to related concepts and topics

### people/
Notable thinkers, writers, journalists, policymakers worth tracking.

Required sections:
- **Role/Background**: Who they are
- **Known For**: Their main ideas or work
- **Notable Pieces**: [[links]] to their articles/papers in wiki
- **Their Lens**: How they see the world (their framework)
- **My Take**: *(Dinesh writes this — leave blank with placeholder)*

### topics/
Running threads on a subject — a topic that keeps appearing across multiple sources.

Required sections:
- **What's the Story**: The core narrative or debate
- **Key Arguments**: Main positions
- **Timeline**: Major developments (if relevant)
- **Key Players**: [[people]] involved
- **Related Concepts**: [[concepts]] at play
- **Articles on This**: [[links]] to wiki/papers, wiki/concepts, wiki/people, or wiki/topics that cover it
- **My Take**: *(Dinesh writes this — leave blank with placeholder)*

### qa/
Answers to questions asked against the wiki.

Required sections:
- **Question**: Exact question asked
- **Answer**: Comprehensive response drawn from wiki
- **Sources Consulted**: [[wiki links]] used
- **Filed**: Date
- **Follow-up Questions**: New questions raised

---

## Backlinking Rules
- Always use [[WikiLink]] format for cross-references
- Every article must link to at least 2 other wiki articles
- New articles must be added to wiki/index.md immediately
- Use the format: `[[article-name]]` (lowercase, hyphenated)

## raw/notes/ Rule
- `raw/notes/` contains Dinesh's own writing: reactions, half-formed essays, opinions
- Treat notes as a source, exactly like articles or papers
- When compiling a note, Dinesh is the author — attribute ideas to him, not external sources
- Notes can be compiled into `wiki/qa/` or `wiki/topics/` articles

## Frontmatter Rule
Every wiki article must start with:
```
---
type: Wiki Note
---
```
This is what distinguishes wiki articles from regular notes in Tolaria's sidebar. Reaction notes in `raw/notes/` use `type: Note` — they are Dinesh's voice, not compiled KB articles.

## My Take Placeholder
When the LLM creates any article, add this at the end:
```
## My Take
> *[Your reaction, opinion, or question here — Dinesh fills this in]*
```
Never write content in this section. It belongs to Dinesh.

## Style Rules
- Write for a smart generalist reader, not a specialist
- Concrete examples always beat abstract explanations
- Prefer synthesis over summary — what does this *mean*?
- Every factual claim traceable to a raw/ source

## Index Rules
- index.md is the master map — always kept up to date
- Group by type: Concepts | Papers | People | Topics | Q&A
- Each entry: `- [[article-name]] — one-line description`
