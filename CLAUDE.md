# glow-themes

Custom themes for [glow](https://github.com/charmbracelet/glow), a terminal markdown renderer.

## Structure

- `style.json` — active theme, based on Glamour's dark style with custom heading and code block rendering

## Installation

1. Clone this repo to `~/Code/glow-themes`

### macOS (Homebrew)

2. Set the style path in `~/Library/Preferences/glow/glow.yml`:
   ```yaml
   style: "/Users/jeff/Code/glow-themes/style.json"
   ```

### Linux (snap)

2. Symlink the style into the glow snap config:
   ```
   ln -s ~/Code/glow-themes/style.json ~/snap/glow/common/style.json
   ```
3. Point glow at it in `~/snap/glow/current/.config/glow/glow.yml`:
   ```yaml
   style: "/home/jeff/snap/glow/common/style.json"
   ```

## Theme design

- **H1** — thick `━━━` rules above/below, uppercase, blue
- **H2** — thin `────` rule below, purple
- **H3** — `▸ ` prefix, pink
- **Code blocks** — `╭─ code ──╮` border, green on dark background
- **Inline code** — green on dark background
