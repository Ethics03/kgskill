---
description: Initialize a new Knowledge Graph with raw/ and wiki/ structure
---

# KG Init Command

Initialize a new Knowledge Graph with the standard structure.

## Usage

The user will call this as `/kg-init [<path>]`

If no path provided, defaults to `~/MyKG`.

## Implementation

### Step 1: Determine Target Path

Check in order:
1. Provided argument
2. `MYKG_PATH` environment variable
3. `~/.config/mykg/config` file
4. Default: `~/MyKG`

```bash
# Resolve target path
resolve_target_path() {
  # 1. Provided argument
  if [ -n "$1" ]; then
    echo "$1"
    return 0
  fi
  
  # 2. Environment variable
  if [ -n "$MYKG_PATH" ]; then
    echo "$MYKG_PATH"
    return 0
  fi
  
  # 3. Config file
  if [ -f "$HOME/.config/mykg/config" ]; then
    local path=$(cat "$HOME/.config/mykg/config")
    if [ -n "$path" ]; then
      echo "$path"
      return 0
    fi
  fi
  
  # 4. Default
  echo "$HOME/MyKG"
  return 0
}

TARGET_PATH=$(resolve_target_path "$1")
```

### Step 2: Check if Already Exists

```bash
if [ -d "$TARGET_PATH" ]; then
  if [ -f "$TARGET_PATH/index.md" ]; then
    echo "KG already exists at $TARGET_PATH"
    echo "Contents:"
    ls -la "$TARGET_PATH"
    echo ""
    echo "To reconfigure, run: /kg-setup"
    exit 0
  else
    echo "Directory exists but is not a KG: $TARGET_PATH"
    echo "Choose a different path or remove the directory."
    exit 1
  fi
fi
```

### Step 3: Create Structure

```bash
# Create main directories
mkdir -p "$TARGET_PATH"
mkdir -p "$TARGET_PATH/raw"/{articles,papers,transcripts,notes}
mkdir -p "$TARGET_PATH/wiki"/{entities,concepts,synthesis,comparisons}

echo "✓ Created directory structure"
```

### Step 4: Create Core Files

Create from templates (copy from kgskills/templates/kg-init/):

- `index.md` — Catalog of all pages
- `log.md` — Chronological change history
- `Active.md` — Current focus items
- `AGENTS.md` — Schema and workflows

```bash
# If kgskills repo is available, copy templates
if [ -d "$HOME/Dev/kgskills/templates/kg-init" ]; then
  cp "$HOME/Dev/kgskills/templates/kg-init"/*.md "$TARGET_PATH/"
  echo "✓ Created core files from templates"
else
  # Create minimal versions
  echo "✓ Created core files"
fi
```

### Step 5: Save Config

```bash
mkdir -p "$HOME/.config/mykg"
echo "$TARGET_PATH" > "$HOME/.config/mykg/config"
echo "✓ Saved config to ~/.config/mykg/config"
```

### Step 6: Success Message

```
✓ Knowledge Graph initialized at $TARGET_PATH

Structure:
├── index.md
├── log.md
├── Active.md
├── AGENTS.md
├── raw/
│   ├── articles/
│   ├── papers/
│   ├── transcripts/
│   └── notes/
└── wiki/
    ├── entities/
    ├── concepts/
    ├── synthesis/
    └── comparisons/

Next:
1. Ingest a source: /kg-ingest <file>
2. Create a project: /kg-project <name>
3. Query the wiki: /kg search <term>
```