---
name: lector
description: Use when reviewing a near-final CS academic paper, master's thesis, or PhD thesis for writing quality and presentation before submission. Triggers on requests to "review", "proofread", "lector", or "check" a CS academic document.
---

# Lector — Academic Writing Review

Near-final self-review of CS academic writing. Checks **writing and presentation only** — not scientific content, not academic integrity.

**Language:** American English  
**Discipline:** Computer science only  
**Stage:** Near-final (not early-draft restructuring)

## Entry Flow

1. Ask: **Document type** — paper / master's thesis / PhD thesis
2. Ask: **Document input** — file path (you read it) or pasted text
3. Read the full document
4. Dispatch **4 subagents in parallel** using the Agent tool:
   - Language agent → `skills/lector/language-agent.md`
   - Structure agent → `skills/lector/structure-agent.md`
   - Elements agent → `skills/lector/elements-agent.md`
   - References agent → `skills/lector/references-agent.md`
5. Pass to each agent: full document text + document type
6. Collect all findings and produce a consolidated report

## Consolidated Report Format

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
### [section-by-section findings, adapted to document type]

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
Total findings: N (Language: N | Structure: N | Elements: N | References: N)
Priority items: [top 3–5 most impactful issues]
```

## Finding Format (all agents)

```
- [LOCATION] [CATEGORY] — "[offending text or element]" → [correction or expectation] (Zobel Ch. X)
```

## Out of Scope

- Scientific content quality (methodology, evaluation soundness, contribution novelty)  
- Academic integrity / plagiarism detection  
- LaTeX/tool-specific formatting issues (overfull hboxes, etc.)  
- Non-CS academic disciplines  
- Early-draft structural rethinking  
