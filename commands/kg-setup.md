---
description: Configure Knowledge Graph path and create config file
---

# KG Setup Command

Configure where your Knowledge Graph is located.

## Usage

The user will call this as `/kg-setup [<path>]`

If no path provided, will prompt for it or use default.

## Implementation

### Step 1: Check Existing Config

Check if path is already configured:

```bash
# Check in order:
# 1. Environment variable
# 2. Config file ~/.config/mykg/config
# 3. Config file ~/.mykgrc

if [ -n "$MYKG_PATH" ]; then
  echo "✓ MYKG_PATH is set: $MYKG_PATH"
  CONFIGURED="env"
elif [ -f "$HOME/.config/mykg/config" ]; then
  MYKG_PATH=$(cat "$HOME/.config/mykg/config")
  echo "✓ Config found: $MYKG_PATH"
  CONFIGURED="config"
elif [ -f "$HOME/.mykgrc" ]; then
  MYKG_PATH=$(cat "$HOME/.mykgrc")
  echo "✓ Config found: $MYKG_PATH"
  CONFIGURED="config"
fi
```

### Step 2: Validate Path

If path is set, verify it exists:

```bash
if [ -n "$MYKG_PATH" ]; then
  if [ -d "$MYKG_PATH" ]; then
    echo "✓ KG exists at $MYKG_PATH"
    echo ""
    echo "Contents:"
    ls "$MYKG_PATH"
  else
    echo "✗ Path exists but KG not found at $MYKG_PATH"
    echo "Run /kg-init to create it"
  fi
  exit 0
fi
```

### Step 3: Prompt for Path

If no path configured, ask the user:

```
No KG path configured.

Options:
1. Provide a path: /kg-setup /path/to/your/kg
2. Use default: ~/MyKG
3. Let me search for existing KGs

Enter path or press Enter to use ~/MyKG:
```

### Step 4: Search for Existing KGs

If user wants to search, look for `index.md` + `log.md` + `Active.md` pattern:

```bash
# Search common locations for KG
SEARCH_PATHS=(
  "$HOME/MyKG"
  "$HOME/Dev/Obsidian/MyKG"
  "$HOME/Documents/MyKG"
  "$HOME/Notes/MyKG"
  "$HOME/vault/MyKG"
)

for path in "${SEARCH_PATHS[@]}"; do
  if [ -f "$path/index.md" ] && [ -f "$path/log.md" ]; then
    echo "Found KG at: $path"
    FOUND+=("$path")
  fi
done

if [ ${#FOUND[@]} -gt 0 ]; then
  echo ""
  echo "Select which KG to use (1-${#FOUND[@]}):"
  for i in "${!FOUND[@]}"; do
    echo "$((i+1))) ${FOUND[$i]}"
  done
fi
```

### Step 5: Save Config

Save the chosen path to config file:

```bash
# Create config directory
mkdir -p "$HOME/.config/mykg"

# Save path
echo "$MYKG_PATH" > "$HOME/.config/mykg/config"

echo "✓ Config saved to ~/.config/mykg/config"
```

### Step 6: Shell Config (Optional)

If user wants to set environment variable permanently:

```bash
echo ""
echo "To set MYKG_PATH permanently, add to your shell config:"
echo ""
echo "  echo 'export MYKG_PATH=\"$MYKG_PATH\"' >> ~/.bashrc"
echo "  # or"
echo "  echo 'export MYKG_PATH=\"$MYKG_PATH\"' >> ~/.zshrc"
echo ""
echo "Then reload: source ~/.bashrc (or ~/.zshrc)"
```

### Step 7: Verify Setup

```bash
echo ""
echo "Setup complete!"
echo ""
echo "KG Path: $MYKG_PATH"
echo ""
echo "Test with:"
echo "  /kg active    - Show current priorities"
echo "  /kg recent    - Show recent changes"
echo "  /kg search X   - Search for term"
```

## Config File Format

The config file is a single line containing the absolute path:

```
/home/user/Dev/Obsidian/MyKG
```

## Config Locations (checked in order)

1. `MYKG_PATH` environment variable
2. `~/.config/mykg/config`
3. `~/.mykgrc`

## Example Session

```
User: /kg-setup
Agent: No KG path configured.

Checking for existing KGs...
Found KG at: /home/user/Dev/Obsidian/MyKG

Use this KG? (Y/n): Y

✓ Config saved to ~/.config/mykg/config

KG Path: /home/user/Dev/Obsidian/MyKG

Test with:
  /kg active    - Show current priorities
  /kg recent    - Show recent changes
```