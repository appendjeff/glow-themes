---
name: cmux-doc-glowup
description: Use when there is a markdown file or plan to review - opens it in a new named cmux tab using glow pager mode
---

# View Plan in New cmux Tab

## Overview

Opens a markdown plan in a new named cmux tab using `glow -p` for pager mode. The tab title is derived from the plan's first H1 heading.

## Steps

1. **Resolve the absolute path** to the markdown file.

2. **Extract the title** from the first H1 heading:
   ```bash
   grep -m1 '^# ' "$FILE" | sed 's/^# //'
   ```
   Fall back to the filename (without extension) if no H1 is found:
   ```bash
   basename "$FILE" .md
   ```

3. **Open a new terminal tab** and capture its surface ID:
   ```bash
   SURFACE=$(cmux new-surface | grep -o 'surface:[0-9]*')
   ```
   > `cmux new-surface` returns a full status line like `OK surface:10 pane:1 workspace:1`; the grep extracts just the `surface:N` ref that subsequent commands expect.

4. **Send the glow command** to the new tab:
   ```bash
   cmux send --surface "$SURFACE" "glow -p $FILE"
   cmux send-key --surface "$SURFACE" "Enter"
   ```

5. **Rename the tab** to the plan title:
   ```bash
   cmux rename-tab --surface "$SURFACE" "$TITLE"
   ```

## Full Example

```bash
FILE="$(realpath plan.md)"
TITLE=$(grep -m1 '^# ' "$FILE" 2>/dev/null | sed 's/^# //')
[ -z "$TITLE" ] && TITLE=$(basename "$FILE" .md)
SURFACE=$(cmux new-surface | grep -o 'surface:[0-9]*')
cmux send --surface "$SURFACE" "glow -p $FILE"
cmux send-key --surface "$SURFACE" "Enter"
cmux rename-tab --surface "$SURFACE" "$TITLE"
```

## Quick Reference

| Situation | Action |
|-----------|--------|
| File has `# My Plan` heading | Tab named `My Plan` |
| File has no H1 | Tab named after filename (no extension) |
| Relative path given | Resolve to absolute with `realpath` before passing to cmux |
