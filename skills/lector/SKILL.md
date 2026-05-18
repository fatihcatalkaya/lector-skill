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
6. **Assign global IDs**: concatenate the four subagent tables in fixed order — Language → Structure → Elements → References — and number rows sequentially starting at `1`. IDs are unique across the whole report; do not reset per agent.
7. Produce the consolidated report below.

## Consolidated Report Format

Each agent gets its own titled section with a finding count and a single markdown table. If an agent reported no findings, keep the section header with `(0 findings)` and write `No findings.` instead of a table.

```
# Lector Review — [Document Title or Filename]
Document type: [Paper / Master's Thesis / PhD Thesis]
Reviewed: [date]

## Language & Style (N findings)
| ID | Location | Category | Finding | Zobel |
|----|----------|----------|---------|-------|
| 1  | §2.1 | Economy | "in order to" → "to" | Ch. 3 |
| 2  | §3   | Register | "we feel that the results are good" → "the results improve X by Y%" | Ch. 2 |

## Structure (N findings)
| ID | Location | Category | Finding | Zobel |
|----|----------|----------|---------|-------|
| 3  | Introduction | Ordering | motivation after method → move motivation before method | Ch. 7 |

## Elements (N findings)
| ID | Location | Category | Finding | Zobel |
|----|----------|----------|---------|-------|
| 4  | Fig. 2 | Caption | caption "Results" → state what is shown and the takeaway | Ch. 11 |

## References (N findings)
| ID | Location | Category | Finding | Zobel |
|----|----------|----------|---------|-------|
| 5  | [Smith24] | Missing entry | cited in §3 but absent from reference list → add bibliography entry | Ch. 6 |

## Summary
Total findings: N (Language: a | Structure: b | Elements: c | References: d)
Priority items: #2, #5, … — one-line reason per ID
```

## Finding Format

Subagents emit rows of `| Location | Category | Finding | Zobel |`. The coordinator prepends an `ID` column with sequential global integers.

## Out of Scope

- Scientific content quality (methodology, evaluation soundness, contribution novelty)  
- Academic integrity / plagiarism detection  
- LaTeX/tool-specific formatting issues (overfull hboxes, etc.)  
- Non-CS academic disciplines  
- Early-draft structural rethinking  
