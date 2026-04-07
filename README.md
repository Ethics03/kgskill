# MyKG Skills

An LLM-maintained personal knowledge graph. Incrementally build a structured wiki of markdown files — reading sources, extracting knowledge, updating pages, and keeping everything connected.

Based on the [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) pattern by Andrej Karpathy.

## What is this?

MyKG provides tools for AI agents to maintain a persistent knowledge base:

1. **Universal Skill** - General KG operations, ingestion, querying, linting
2. **Project Skills** - Domain-specific instructions for research, reading, personal tracking

Three-layer architecture:
- **Raw sources** — immutable source documents (papers, articles, transcripts)
- **Wiki** — LLM-generated pages (entities, concepts, synthesis)
- **Schema** — configuration telling the LLM how to maintain the wiki

## Installation

### OpenCode

```bash
# 1. Install the skill
mkdir -p ~/.opencode/skills/mykg
cp SKILL.md ~/.opencode/skills/mykg/SKILL.md

# 2. Install commands
mkdir -p ~/.config/opencode/command
cp commands/*.md ~/.config/opencode/command/
```

### Marketplace

```
/plugin marketplace add https://github.com/Ethics03/kgskill
/plugin install mykg@kgskill
```

### npx skills

```
npx skills add https://github.com/Ethics03/kgskill.git
```

### Manual

```bash
# Copy skills into your .claude folder
cp -r skills/ ~/.claude/skills/

# Copy commands
mkdir -p ~/.claude/commands
cp commands/*.md ~/.claude/commands/
```

### Other AI Agents

The skill and commands are platform-agnostic markdown files. Adapt for your agent:

1. **Skill file** (`SKILL.md`) — Load as system instructions or context
2. **Commands** (`commands/*.md`) — Register as slash commands

The commands are shell scripts wrapped in markdown — extract the bash sections and adapt to your platform.

### Initialize or Configure

```
/kg-init [path]      # Create new KG
/kg-setup            # Configure existing KG
```

## Quick Start

### Initialize New KG

```
/kg-init ~/MyKG
```

Creates:
```
MyKG/
├── index.md
├── log.md
├── Active.md
├── AGENTS.md
├── raw/
│   ├── articles/
│   ├── papers/
│   └── transcripts/
└── wiki/
    ├── entities/
    ├── concepts/
    ├── synthesis/
    └── comparisons/
```

### Ingest a Source

```
/kg-ingest raw/articles/my-article.md
```

The LLM will:
1. Read the source
2. Extract entities and concepts
3. Create/update wiki pages
4. Link related pages
5. Update index and log

### Query the Wiki

```
/kg search <term>    # Search wiki pages
/kg recent           # Recent changes
/kg entities         # List entities
/kg concepts         # List concepts
```

### Create a Project

```
/kg-project research-paper-x "Research on quantum computing"
```

Creates project-specific workspace with custom `SKILL.md`.

## Commands

| Command | Description |
|---------|-------------|
| `/kg-setup` | Configure KG path |
| `/kg-init [path]` | Initialize new KG |
| `/kg-project <name>` | Create project workspace |
| `/kg-ingest <source>` | Ingest source into wiki |
| `/kg-lint` | Health check wiki |
| `/kg recent` | Recent changes |
| `/kg search <term>` | Search wiki |
| `/kg entities` | List entity pages |
| `/kg concepts` | List concept pages |
| `/kg active` | Current focus items |

## Configuration

KG path resolved in order:
1. `MYKG_PATH` environment variable
2. `~/.config/mykg/config` file
3. Common locations: `~/MyKG`, `~/Dev/Obsidian/MyKG`

## Operations

### Ingest

Read source → extract knowledge → update wiki:
- Create summary page
- Extract entities (people, orgs, tools)
- Extract concepts (ideas, topics, themes)
- Note contradictions with existing pages
- Update index and log

### Query

Search wiki → synthesize answer → cite sources:
- Read index to find relevant pages
- Drill into specific pages
- Synthesize with `[[citations]]`
- Optionally save answer as new page

### Lint

Health check the wiki:
- Orphan pages (no inbound links)
- Contradictions between pages
- Missing cross-references
- Stale claims
- Gaps to investigate

## Structure

```
kgskills/
├── .claude-plugin/plugin.json    # Claude Code plugin manifest
├── SKILL.md                      # Universal skill
├── commands/                     # Slash commands
├── skills/mykg/                  # Plugin skill entry (symlink to SKILL.md)
├── templates/kg-init/            # New KG scaffold
├── project-templates/init/       # New project scaffold
└── docs/
    ├── concepts.md
    └── examples.md
```

## Two-Tier Architecture

### Universal Skill

Loaded once. Teaches the agent:
- KG structure and conventions
- Ingest, query, lint operations
- Standard page types
- Wikilink syntax

### Project Skill

Loaded per-project. Contains:
- Domain-specific page types
- Custom ingestion workflows
- Domain terminology
- Project-specific formatting

Example:
```
MyKG/
├── index.md
├── raw/
├── wiki/
├── ProjectA/
│   ├── SKILL.md              # "Research paper on X"
│   ├── raw/papers/
│   └── wiki/
└── ProjectB/
    ├── SKILL.md              # "Reading The Lord of the Rings"
    ├── raw/chapters/
    └── wiki/
```

## Examples

### Research Project
- Raw: papers, articles, transcripts
- Wiki: paper summaries, researcher entities, concept synthesis
- Use: track evolving thesis across sources

### Reading a Book
- Raw: chapter notes
- Wiki: characters, themes, plot threads
- Use: build companion wiki as you read

### Personal Tracking
- Raw: journal entries, health data
- Wiki: patterns, insights over time
- Use: self-knowledge accumulation

## Tips

- **Obsidian Web Clipper** — save web articles as markdown
- **Graph view** — see wiki structure, find orphans
- **Git repo** — version history for free
- **Good answers → wiki pages** — queries compound knowledge too

## Companion Skills

Install skills from [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) for enhanced mykg workflows:

| Skill | Benefit |
|-------|---------|
| `obsidian-markdown` | Correct wikilink, frontmatter, and callout syntax when writing wiki pages |
| `obsidian-cli` | Open and search notes directly when the KG lives in an Obsidian vault |
| `defuddle` | Clean markdown extraction from URLs when ingesting web sources |

## Why I Built This

This pattern is inspired by **Andrej Karpathy's approach** to knowledge management. His insight was that knowledge bases should be living artifacts that grow with you — not static repositories that rot.

The insight: LLMs are great at the tedious work — touching 15 files in one pass, maintaining consistency, updating cross-references. They don't get bored.

I extended this with **project-specific customization**. A research project needs different entity types than reading a novel. The two-tier skill system lets each project have its own domain model while sharing the universal infrastructure.

This is for people who want a knowledge base that grows with them — that compounds knowledge rather than just storing it.

**Credit:** The core structure and philosophy come from Karpathy's work. I adapted it with the two-tier skill system for project-specific customization.

## License

MIT
