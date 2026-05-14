# Lector — CS Academic Writing Review Skill

A Claude Code plugin that reviews near-final CS academic writing for **writing quality and presentation** — not scientific content. Based on Justin Zobel's *Writing for Computer Science*.

## What it does

When you invoke the `lector` skill, it runs 4 subagents in parallel, each covering a distinct dimension:

| Agent | Checks |
|---|---|
| **Language** | Economy & style, register & clarity, grammar & punctuation, American English, CS anti-patterns |
| **Structure** | Title, abstract, introduction, body, related work, conclusion, heading style (adapted to document type) |
| **Elements** | Figures, tables, algorithms, mathematical notation |
| **References** | Citation ↔ reference list integrity, completeness, consistency |

Findings follow a uniform format with chapter references back to Zobel:

```
- [LOCATION] [CATEGORY] — "offending text" → correction (Zobel Ch. X)
```

## Scope

- **In scope:** CS papers, master's theses, PhD theses at near-final stage
- **Language standard:** American English
- **Out of scope:** Scientific content quality, methodology soundness, contribution novelty, LaTeX formatting issues, academic integrity, non-CS disciplines, early-draft restructuring

## Installation

```bash
claude plugin install https://github.com/fatihcatalkaya/lector-skill
```

Or from a local clone:

```bash
claude plugin install /path/to/lector-skill
```

## Usage

```
/lector
```

Claude will ask for the document type (paper / master's thesis / PhD thesis) and the document (file path or pasted text), then produce a consolidated report.

## Reference

Zobel, J. (2014). *Writing for Computer Science* (3rd ed.). Springer. (https://doi.org/10.1007/978-1-4471-6639-9)
