---
description: Configure Knowledge Graph path and create config file
---

Configure the Knowledge Graph path. If a path argument was provided, use it directly. Otherwise check for existing config and report it, or search for a KG and prompt the user.

## Steps

**1. Check for existing config**

Check in order:
1. `$MYKG_PATH` environment variable
2. `~/.config/mykg/config` file
3. `~/.mykgrc` file

If found, verify the path exists on disk:
- If it exists and contains `index.md`: report `✓ KG found at <path>` and stop.
- If the path doesn't exist: tell the user the configured path is missing and suggest `/kg-init` to create it.

**2. If no config found, search common locations**

Check each of these for `index.md` + `log.md`:
- `~/MyKG`
- `~/Dev/Obsidian/MyKG`
- `~/Documents/MyKG`
- `~/Notes/MyKG`
- `~/vault/MyKG`

If any are found, show them to the user and ask which to use.

**3. If nothing found, ask the user**

Ask: "No KG found. Enter a path to configure, or I can create one with `/kg-init`."

**4. Save config**

Once a path is confirmed:
```bash
mkdir -p ~/.config/mykg
echo "<path>" > ~/.config/mykg/config
```

Report `✓ Config saved. KG path: <path>`.
