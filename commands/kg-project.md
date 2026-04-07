---
description: Create a new project in the Knowledge Graph with ADR tracking
---

# KG Project Command

Create a new project folder in the Knowledge Graph.

## Usage

The user will call this as `/kg-project <name> [<description>]`

## Implementation

### Step 1: Resolve KG Path

Check in order:
1. `MYKG_PATH` environment variable
2. `~/.config/mykg/config` file
3. `~/.mykgrc` file
4. Common locations

```bash
# Resolve KG path
resolve_kg_path() {
  # 1. Environment variable
  if [ -n "$MYKG_PATH" ] && [ -d "$MYKG_PATH" ]; then
    echo "$MYKG_PATH"
    return 0
  fi
  
  # 2. Config file
  if [ -f "$HOME/.config/mykg/config" ]; then
    local path=$(cat "$HOME/.config/mykg/config")
    if [ -d "$path" ]; then
      echo "$path"
      return 0
    fi
  fi
  
  # 3. Legacy config
  if [ -f "$HOME/.mykgrc" ]; then
    local path=$(cat "$HOME/.mykgrc")
    if [ -d "$path" ]; then
      echo "$path"
      return 0
    fi
  fi
  
  # 4. Common locations
  for loc in "$HOME/MyKG" "$HOME/Dev/Obsidian/MyKG" "$HOME/Documents/MyKG"; do
    if [ -d "$loc" ]; then
      echo "$loc"
      return 0
    fi
  done
  
  # Not found
  echo "KG path not found. Run /kg-setup or /kg-init first."
  return 1
}

MYKG_PATH=$(resolve_kg_path)
if [ $? -ne 0 ]; then
  exit 1
fi
```

### Step 2: Get Project Name

```bash
PROJECT_NAME="$1"
PROJECT_DESC="${2:-$PROJECT_NAME}"

if [ -z "$PROJECT_NAME" ]; then
  echo "Usage: /kg-project <name> [description]"
  exit 1
fi

# Normalize name (alphanumeric, hyphens)
PROJECT_NAME=$(echo "$PROJECT_NAME" | tr ' ' '-' | tr -cd 'a-zA-Z0-9-')
```

### Step 3: Check if Project Exists

```bash
if [ -d "$MYKG_PATH/$PROJECT_NAME" ]; then
  echo "Project '$PROJECT_NAME' already exists"
  ls -la "$MYKG_PATH/$PROJECT_NAME"
  exit 0
fi
```

### Step 4: Create Project Structure

```bash
mkdir -p "$MYKG_PATH/$PROJECT_NAME"/{Architecture,Decisions,Plans,Analysis}
```

### Step 5: Create Decisions README

Create `$MYKG_PATH/$PROJECT_NAME/Decisions/README.md`:

```markdown
# Decisions

Architecture Decision Records (ADRs) for $PROJECT_NAME.

## Index

| ADR | Title | Status | Date |
|-----|-------|--------|------|

## Next ADR Number

**Next: 001**

## Creating a New ADR

1. Update the number above (e.g.,001 → 002)
2. Copy `../../Decision Template.md`
3. Create `ADR-XXX - Title.md`
4. Fill in: Context, Decision, Consequences, Alternatives
5. Update this index
6. Update `../../index.md`
7. Add entry to `../../log.md`
```

### Step 6: Create Project SKILL.md (optional)

Create `$MYKG_PATH/$PROJECT_NAME/SKILL.md`:

```markdown
---
name: ${PROJECT_NAME}-skill
description: Project-specific instructions for ${PROJECT_NAME}. Use when working on codebase or making decisions.
---

# ${PROJECT_NAME} Skill

## Project Overview

[Description: ${PROJECT_DESC}]

## Tech Stack

| Component | Technology |
|-----------|------------|
| | |

## Key Architecture Decisions

Refer to ADRs in [[Decisions/]].

## Current Priorities

See [[Active]] for current focus items.

## Project Conventions

### Code Style
- [Add conventions]

### Naming
- Files: [Convention]
- Components: [Convention]

## Key Files

| File | Purpose |
|------|---------|
| `Architecture/Overview.md` | System design |
| `Decisions/README.md` | ADR index |
```

### Step 7: Update Root index.md

Add the new project to `$MYKG_PATH/index.md`:

```markdown
## Projects

| Project | Description |
|---------|-------------|
| [[${PROJECT_NAME}/]] | ${PROJECT_DESC} |
```

### Step 8: Update Root log.md

```markdown
## $(date +%Y-%m-%d)

- Created project **${PROJECT_NAME}** - [[${PROJECT_NAME}/]]
```

### Step 9: Success Message

```
✓ Project '${PROJECT_NAME}' created at $MYKG_PATH/$PROJECT_NAME

Structure:
├── SKILL.md
├── Architecture/
├── Decisions/
│   └── README.md
├── Plans/
└── Analysis/

Next:
1. Edit SKILL.md with project details
2. Create Architecture/Overview.md
3. Add your first ADR: /kg-decision ${PROJECT_NAME} "Decision Title"
```