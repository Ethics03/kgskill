# Examples

## Research Project

```
MyKG/
├── index.md
├── log.md
├── AGENTS.md
├── Active.md
├── raw/
│   ├── papers/
│   │   ├── attention-is-all-you-need.pdf
│   │   └── bert-pretraining.pdf
│   └── articles/
│       └── transformer-explainer.md
└── wiki/
    ├── entities/
    │   ├── google.md
    │   ├── openai.md
    │   └── vaswani.md
    ├── concepts/
    │   ├── attention.md
    │   ├── transformers.md
    │   └── pretraining.md
    ├── synthesis/
    │   └── nlp-progress.md
    └── comparisons/
        └── bert-vs-gpt.md
```

### After Ingesting a Paper

`/kg-ingest raw/papers/attention-is-all-you-need.pdf`

Creates:
- `wiki/attention-is-all-you-need.md` — Source summary
- `wiki/entities/vaswani.md` — Lead author
- `wiki/entities/google.md` — Organization (updated)
- `wiki/concepts/attention.md` — New concept
- `wiki/concepts/transformers.md` — Updated with new insights

Updates:
- `wiki/synthesis/nlp-progress.md` — Added transformer revolution
- `index.md` — New pages listed
- `log.md` — Entry added

### Query Example

`/kg search transformers`

Finds:
- `wiki/concepts/transformers.md` — Main concept page
- `wiki/attention-is-all-you-need.md` — Source that introduced it
- `wiki/comparisons/bert-vs-gpt.md` — Comparison using transformers

---

## Reading a Book

```
MyKG/
├── index.md
├── log.md
├── AGENTS.md
├── Active.md
├── raw/
│   └── chapters/
│       ├── lotr-fellowship-01.md
│       ├── lotr-fellowship-02.md
│       └── ...
└── wiki/
    ├── entities/
    │   ├── frodo.md
    │   ├── gandalf.md
    │   ├── sam.md
    │   └── rivendell.md
    ├── concepts/
    │   ├── the-ring.md
    │   ├── corruption.md
    │   └── friendship.md
    ├── synthesis/
    │   └── fellowship-theme.md
    └── comparisons/
        └── frodo-vs-bilbo.md
```

### Character Page Example

```markdown
# Frodo Baggins

## Overview
Protagonist of The Lord of the Rings. A hobbit of the Shire 
who inherits the One Ring and undertakes the quest to destroy it.

## Key Facts
- Born: September 22, TA 2968
- Home: Bag End, Hobbiton
- Quest: Destroy the One Ring in Mount Doom

## Relationships
- [[sam]]: Friend and companion
- [[gandalf]]: Guide and protector
- [[bilbo]]: Uncle and ring-bearer predecessor

## Appearances
- [[lotr-fellowship-01]]: Introduced, inherits the ring
- [[lotr-fellowship-02]]: Leaves the Shire
- ...

## Arc
Reluctant hero who grows from innocent hobbit to world-weary 
ring-bearer. The ring's corruption slowly affects him.

## Sources
- [[raw/chapters/lotr-fellowship-01]]
- [[raw/chapters/lotr-fellowship-02]]
```

---

## Personal Tracking

```
MyKG/
├── index.md
├── log.md
├── AGENTS.md
├── Active.md
├── raw/
│   ├── journal/
│   │   ├── 2024-01-15.md
│   │   └── 2024-01-16.md
│   └── articles/
│       └── sleep-science.md
└── wiki/
    ├── entities/
    │   └── me.md
    ├── concepts/
    │   ├── sleep-quality.md
    │   ├── exercise.md
    │   └── energy.md
    └── synthesis/
        └── patterns.md
```

### Pattern Detection

After ingesting multiple journal entries:

- `wiki/synthesis/patterns.md` — "Energy dips at 3pm consistently"
- `wiki/concepts/sleep-quality.md` — Correlation between 7+ hours and better focus
- Contradiction flagged: Sleep quality varies significantly

---

## Project with Custom SKILL.md

```markdown
---
name: quantum-computing-skill
description: Research project on quantum computing
---

# Quantum Computing Research

## Domain
Quantum computing algorithms and hardware

## Source Types
- Papers (arXiv)
- Conference proceedings
- Blog posts from researchers

## Key Entities
- Research labs (Google Quantum AI, IBM Quantum)
- Researchers (John Preskill, Peter Shor)
- Companies (IBM, Google, Microsoft)

## Key Concepts
- Qubits, superposition, entanglement
- Quantum error correction
- NISQ era

## Ingestion Workflow
1. Extract core contribution from paper
2. Identify cited works → create/update entity pages
3. Note which concepts the paper advances
4. Flag contradictions with earlier findings
```