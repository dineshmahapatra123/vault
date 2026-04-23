---
description: Shatter a PhD research paper into atomic Wiki articles (Concepts, People, Methods).
---

# /compile-phd Workflow

Use this command to process a research paper from `7 - Papers/` into your `9 - Knowledge_base/`.

### 1. Preparations
- Identify the paper file name.
- Confirm it exists in `7 - Papers/`.

### 2. Full-Text Conversion (Neural Bridge)
- **Check**: Does a matching `.md` file exist in `9 - Knowledge_base/Full-Text/`?
- **Action**: If not, run **`@[/pdf2md]`** on the PDF and save output to `/9 - Knowledge_base/Full-Text/`.

### 3. Deconstruction (The Shattering)
- Read the FULL TEXT of the paper from the MD file.
- Analyze against your `9 - Knowledge_base/PHD_SCHEMA.md`.
- **Create/Update**:
    - **Methods**: Extract the research design and design logic.
    - **Concepts**: Extract theoretical definitions and debates.
    - **People**: Map the author's lens and scholarly network.

### 4. Backlinking & Indexing
- Link all new wiki articles to each other.
- Link them back to the original `2 - Notes/Papers/[Name].md` note.
- Update `9 - Knowledge_base/index.md` with the new entries.

---

> [!NOTE]
> This workflow is rigorous. It prioritizes academic finding and methodological "how" over general summary.
