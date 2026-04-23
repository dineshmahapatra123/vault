---
description: Compile a new raw/ file into wiki/ articles following the Karpathy KB workflow
---

## What This Does
Reads a raw source file and compiles it into structured wiki articles,
exactly like Karpathy's LLM Knowledge Base system.
One raw file → multiple wiki articles (people, concepts, topics, papers).

## Steps

1. Read these files first to understand the knowledge base:
   - `/Users/dineshmahapatra/Downloads/Dinesh/Vault/wiki/SCHEMA.md` — article structure rules
   - `/Users/dineshmahapatra/Downloads/Dinesh/Vault/wiki/index.md` — what already exists
   - All files in `/Users/dineshmahapatra/Downloads/Dinesh/Vault/wiki/concepts/`
   - All files in `/Users/dineshmahapatra/Downloads/Dinesh/Vault/wiki/people/`
   - All files in `/Users/dineshmahapatra/Downloads/Dinesh/Vault/wiki/topics/`

2. Read the raw source file specified by the user.

3. Decide what wiki articles to create. Rules:
   - Essay by a named author → always create `wiki/people/[author-name].md` (or update if exists)
   - Any strong idea or framework → create `wiki/concepts/[concept-name].md`
   - A running debate or news thread → create `wiki/topics/[topic-name].md`
   - Academic paper → create `wiki/papers/[paper-name].md`
   - One raw file typically produces 2–5 wiki articles
   - If a concept already exists in the wiki, UPDATE it with new evidence — don't duplicate

4. Write each wiki article following SCHEMA.md exactly:
   - Use `[[WikiLink]]` format for ALL cross-references
   - Link to at least 2 other wiki articles per new article
   - Check index.md — link to articles that already exist wherever relevant
   - End every article with: `## My Take` followed by `> *[Your reaction, opinion, or question here — Dinesh fills this in]*`
   - Never write content in the My Take section

5. Update `wiki/index.md`:
   - Add every new article under the correct heading (Concepts / Papers / People / Topics / Q&A)
   - Update the Statistics block: increment article count, update date, update source count

6. Create a blank note template in `raw/notes/`:
   - Filename: `on-[source-filename-slug].md`
   - Content:
     ```
     ---
     type: my-note
     source: raw/[folder]/[filename]
     author-of-source: [author name]
     date: [today's date]
     ---

     # My Reaction: [Source Title]

     *Write freely below. No structure needed. This is your voice.*

     ---

     ```

7. Report to the user:
   - List every file created (with full path)
   - List every existing file updated
   - Note any new [[WikiLinks]] that don't have articles yet (backlog)
   - Remind user: open `raw/notes/on-[source].md` in Obsidian to write their take

## Example
User says: `/compile-kb raw/articles/Write Till You Drop.md`
Expected output:
- wiki/people/annie-dillard.md (NEW)
- wiki/concepts/writing-as-compulsion.md (NEW)
- wiki/concepts/spend-it-all.md (NEW)
- wiki/people/george-orwell.md (UPDATED — new backlink added)
- wiki/index.md (UPDATED)
- raw/notes/on-write-till-you-drop.md (BLANK TEMPLATE created)
