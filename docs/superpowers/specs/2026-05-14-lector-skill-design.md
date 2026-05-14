# Lector Skill — Design Spec

**Date:** 2026-05-14
**Status:** Approved

---

## Context

This project contains reference literature from Justin Zobel's *Writing for Computer Science* (3rd ed., Springer 2014), stored as high-quality markdown files under `literature/Writing-for-Computer-Science/`. The lector skill must aggregate concrete good/bad examples from these files directly into each subagent's prompt — this is a hard requirement.

---

## Purpose

A Claude Code skill that helps a CS author do a near-final self-review of their academic writing before submission. The skill checks writing quality and presentation only — it does not evaluate scientific content, methodology soundness, or academic integrity.

**Target user:** The author reviewing their own work  
**Stage:** Near-final (not early draft restructuring)  
**Discipline:** Computer science only  
**Language:** American English  

---

## Architecture

The skill uses a **coordinator + 4 parallel subagents** pattern:

```
lector (coordinator)
├── asks: document type? (paper / master's thesis / PhD thesis)
├── asks: document input? (file path or pasted text)
├── reads the document
├── dispatches 4 subagents in parallel:
│   ├── language-agent      → style, grammar, anti-patterns, American English
│   ├── structure-agent     → section-by-section checks (adapts to doc type)
│   ├── elements-agent      → figures, tables, algorithms, equations
│   └── references-agent    → citations, reference list, consistency
└── consolidates findings into a unified review report
```

The coordinator reads the document once and passes the full text + document type to each subagent. Subagents run in parallel. Each returns structured findings. The coordinator formats and combines them into a single report.

---

## Subagent 1: Language Agent

**Zobel source chapters:** Ch. 6 (Good Style), Ch. 7 (Style Specifics), Ch. 8 (Punctuation), Ch. 13 (Editing)

### Style & Economy
- Deadwood and padding: "it is important to note that", "in order to", "the fact that", "it should be noted that"
- Nominalizations: verb-as-noun constructions ("the performance of the evaluation" → "evaluating performance")
- Vague quantifiers: "many", "several", "a number of", "some" used without specifics
- Tautologies: "completely eliminate", "future plans", "end result", "past history"

### Register & Clarity
- Colloquialisms inappropriate in academic writing
- Ambiguous pronoun references ("this" or "it" with no clear antecedent)
- Overly long sentences that should be split
- Inconsistent tense: present tense for established facts, past tense for the author's own experiments

### Grammar & Punctuation
- Comma splices and run-on sentences
- Hyphen vs. em-dash misuse
- Missing comma before coordinating conjunctions in compound sentences
- Apostrophe errors
- CS-specific capitalization conventions (e.g., "Figure" vs. "figure" when used as a label, "Internet", algorithm names)

### American English
- British spellings flagged and corrected: "colour" → "color", "analyse" → "analyze", "centre" → "center", "behaviour" → "behavior", "recognise" → "recognize", "programme" → "program", etc.
- American punctuation: punctuation goes inside quotation marks
- American vocabulary: "math" (not "maths"), "program" (not "programme"), "defense" (not "defence"), "acknowledgments" (not "acknowledgements")

### CS Anti-Patterns (Zobel-specific)
- Passive voice overuse hiding agency ("it was found that" → "we found that")
- Excessive hedging where directness is more appropriate
- "In this paper we…" / "In this thesis we…" as unnecessary preamble in the abstract
- Overuse of "clearly", "obviously", "trivially" — claims that may not be obvious to the reader

**Output per finding:**
```
[LOCATION] [CATEGORY] — "[offending text]" → [corrected form or explanation] (Zobel Ch. X)
```

---

## Subagent 2: Structure Agent

**Zobel source chapters:** Ch. 2 (Getting Started), Ch. 3 (Reading and Reviewing), Ch. 4 (Hypotheses, Questions, Evidence), Ch. 5 (Writing a Paper), Ch. 12 (Other Professional Writing)

### Universal Checks (all document types)
- **Title:** Descriptive and specific; avoids "A study of…", "An investigation into…", "On the…" patterns
- **Abstract:** Self-contained (no undefined abbreviations, no citations); accurately summarizes the work; fits expected length
- **Introduction:** Motivates the problem clearly; states the contribution explicitly; outlines the document structure
- **Body paragraphs:** Each paragraph has one focus; paragraphs open with a topic sentence; transitions between sections are explicit and logical
- **Conclusion:** Summarizes contributions without introducing new claims; future work is specific and plausible; does not simply repeat the abstract verbatim
- **Heading style:** Consistent capitalization and depth; no orphaned single subsections

### Paper-Specific Checks
- Related work is positioned against the paper's contribution (not just a neutral survey)
- Evaluation section is present and addresses the stated research questions
- Abstract follows problem → approach → result → significance structure

### Master's Thesis-Specific Checks
- Each chapter opens with a short introduction and closes with a chapter summary
- Table of contents matches actual chapter and section structure
- List of figures and list of tables are present
- Acknowledgements section is present

### PhD Thesis-Specific Checks
All master's thesis checks, plus:
- Research contributions are stated explicitly (in a dedicated section or prominently in the introduction)
- Each chapter has its own introduction that establishes context without assuming the reader has read prior chapters
- Cross-chapter notation consistency is flagged (particularly important for multi-paper theses)

**Output per finding:**
```
[SECTION NAME] [CATEGORY] — [description of issue] → [what is expected] (Zobel Ch. X)
```

---

## Subagent 3: Elements Agent

**Zobel source chapters:** Ch. 9 (Mathematics), Ch. 10 (Algorithms), Ch. 11 (Graphs, Figures, and Tables)

### Figures
- Every figure has a caption placed below the figure (CS convention)
- Every figure is referenced in the text before it appears ("Figure 3 shows…")
- Captions are self-contained — readable without the surrounding text
- Axes are labeled with units where applicable
- Legends present when multiple series are shown
- No figure is unreferenced or undiscussed in the text

### Tables
- Every table has a caption placed above the table (CS convention)
- Every table is referenced in the text before it appears
- Column headers are present and descriptive
- Units are in headers or caption, not repeated in every cell
- Captions are self-contained

### Algorithms
- Every algorithm has a caption and a number
- Every algorithm is referenced in the text
- Input and output are specified
- Notation is consistent with the mathematical notation in surrounding text

### Mathematical Notation
- All variables are defined before first use
- Equations referenced in text are numbered; standalone display equations not referenced need not be numbered
- Notation is consistent throughout (no symbol reuse for different concepts; no concept with multiple symbols)
- Equations are typeset as math markup, not plain text ("x = y * z" in prose is flagged)
- Short expressions use inline math; standalone equations use display math

**Output per finding:**
```
[ELEMENT ID or LOCATION] [TYPE] — [description of issue] → [what is expected] (Zobel Ch. X)
```

---

## Subagent 4: References Agent

**Zobel source chapters:** Ch. 5 (Writing a Paper), Ch. 7 (Style Specifics)

### Citation ↔ Reference List Integrity
- Every in-text citation has a corresponding entry in the reference list
- Every entry in the reference list is cited at least once in the text
- No broken citation markers (`[?]`, `??`, `[Author, YEAR]` placeholders)

### Reference Entry Completeness
- Each entry contains required fields: authors, title, venue (conference or journal name), year
- Conference papers include proceedings name and page numbers where available
- Journal papers include volume, issue, and page numbers where available

### Consistency
- Citation style is uniform throughout: all numeric `[1]` or all author-year `[Zobel, 2014]`, not mixed
- Reference list formatting is consistent: same capitalization style for titles, same abbreviation style for venue names
- Author name format is consistent: all "First Last" or all "Last, F.", not mixed within the list

### CS-Specific Conventions
- Conference and journal abbreviations are standard and consistent within the list (SIGMOD, ICML, NeurIPS — not invented or mixed-form abbreviations)

**Output per finding:**
```
[CITATION KEY or REF #] [CATEGORY] — [description of issue] → [what is expected] (Zobel Ch. X)
```

---

## Coordinator & Report Format

### Entry Flow
1. Ask: document type — paper / master's thesis / PhD thesis
2. Ask: document input — file path or pasted text
3. Read document content
4. Dispatch all 4 subagents in parallel with document content + document type
5. Collect findings from all 4 agents
6. Produce consolidated report

### Report Structure
```
# Lector Review — [Document Title or Filename]
Document type: [Paper / Master's Thesis / PhD Thesis]
Reviewed: [date]

## Pass 1: Language & Style
### Economy & Style
### Register & Clarity
### Grammar & Punctuation
### American English

## Pass 2: Structure
### [section-by-section findings]

## Pass 3: Elements
### Figures
### Tables
### Algorithms
### Mathematical Notation

## Pass 4: References
### Citation ↔ Reference List Integrity
### Completeness
### Consistency

## Summary
Total findings: [N] (Language: N | Structure: N | Elements: N | References: N)
Priority items: [top 3–5 most impactful issues flagged explicitly]
```

### Finding Format (all agents)
```
- [LOCATION] [CATEGORY] — "[offending text or element]" → [correction or expectation] (Zobel Ch. X)
```

---

## Zobel Integration Requirement

Each subagent prompt must embed concrete examples extracted directly from the Zobel markdown files in `literature/Writing-for-Computer-Science/`. This is a hard requirement — not optional. Examples should include both the bad pattern and the corrected form as Zobel presents them. The implementation phase must read the relevant chapters and pull specific illustrative pairs into each agent prompt.

---

## Out of Scope

- Scientific content quality (methodology, evaluation soundness, contribution novelty)
- Academic integrity / plagiarism detection
- LaTeX / formatting tool-specific issues (e.g., overfull hboxes)
- Non-CS academic disciplines
- Early-draft structural rethinking
- ArXiv-only citation flagging
