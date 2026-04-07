---
description: Query KG - use 'active', 'recent', 'search <term>', 'decisions <project>', or 'plan <project>'
---

# KG Query Command

Query the Knowledge Graph for architectural decisions, plans, analysis, and project context.

## Usage

The user will call this as `/kg <subcommand> [<args>]`

### Subcommands

1. **`/kg active`** - Show current priorities from Active.md
2. **`/kg recent`** - Show last 10 entries from log.md
3. **`/kg search <term>`** - Search allpages for the term
4. **`/kg decisions <project>`** - List all ADRs for a project
5. **`/kg plan <project>`** - Show plans for a project

## Implementation

### Step 1: Resolve KG Path

Check in order:
1. `MYKG_PATH` environment variable
2. `~/.config/mykg/config` file
3. `~/.mykgrc` file
4. Common locations: `~/MyKG`, `~/Dev/Obsidian/MyKG`

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
  echo "KG path not found. Run /kg-setup to configure."
  return 1
}

KG_PATH=$(resolve_kg_path)
if [ $? -ne 0 ]; then
  exit 1
fi
```

### Step 2: Execute Subcommand

Based on the user's subcommand:

#### `/kg active`
```bash
cat "$KG_PATH/Active.md"
```
Show current priorities and focus items.

#### `/kg recent`
```bash
tail -50 "$KG_PATH/log.md"
```
Show recent changes.

#### `/kg search <term>`
```bash
grep -ri "<term>" "$KG_PATH" --include="*.md" | head -20
```
Search for the term across all pages.

#### `/kg decisions <project>`
```bash
ls -la "$KG_PATH/<project>/Decisions/"
cat "$KG_PATH/<project>/Decisions/README.md"
```
List all ADRs and show the index.

#### `/kg plan <project>`
```bash
ls -la "$KG_PATH/<project>/Plans/"
```
List all plans for the project.

### Step 3: Format Output

Present the results in a readable format:
- For lists: use bullet points or tables
- For files: show title and summary, offer to read full content
- For searches: show file path and matching line

## Error Handling

- If KG path not found: Run `/kg-setup` to configure
- If project not found: List available projects from index.md
- If file not found: Suggest similar files or check index.md

## After Querying

After showing results, offer to:
- Read full content of a specific file
- Navigate to related pages via wikilinks
- Add new content (decision, plan, analysis)