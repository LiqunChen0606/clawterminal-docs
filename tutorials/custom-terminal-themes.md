# Custom Terminal Themes

> Design your own terminal color scheme with a full ANSI color palette editor, or customize the built-in themes to match your preferences.

---

## Built-In Themes

ClawTerminal ships with five built-in terminal themes:

| Theme | Description |
|-------|-------------|
| **Default Dark** | Dark background with standard terminal colors |
| **Solarized Dark** | Ethan Schoonover's precision-engineered dark colorscheme |
| **One Dark** | Atom editor's popular dark theme |
| **Monokai** | Sublime Text's iconic dark theme with vivid highlights |
| **Gruvbox Dark** | Retro-warm dark theme with earthy tones |

To switch between built-in themes:

1. Go to **Settings --> Terminal Theme**
2. Tap any theme name to apply it immediately
3. The terminal updates in real time

---

## Creating a Custom Theme

### Step 1 --- Open the Theme Editor

1. Go to **Settings --> Terminal Theme**
2. Tap **Create Custom Theme**

The theme editor opens with color pickers for every configurable element.

### Step 2 --- Set Base Colors

The top section of the editor has four main color pickers:

| Color | What it controls |
|-------|-----------------|
| **Background** | Terminal background color |
| **Foreground** | Default text color |
| **Cursor** | Cursor block or line color |
| **Selection** | Text selection highlight color |

Tap any color swatch to open the iOS `ColorPicker`, which supports:

- A spectrum/grid color picker
- RGB sliders
- Hex color input (e.g., `#1E1E2E`)
- Opacity slider

### Step 3 --- Set ANSI Colors

Below the base colors, you will find 16 ANSI color swatches arranged in two rows:

**Normal colors (0--7):**

| Index | Name |
|-------|------|
| 0 | Black |
| 1 | Red |
| 2 | Green |
| 3 | Yellow |
| 4 | Blue |
| 5 | Magenta |
| 6 | Cyan |
| 7 | White |

**Bright colors (8--15):**

| Index | Name |
|-------|------|
| 8 | Bright Black (Gray) |
| 9 | Bright Red |
| 10 | Bright Green |
| 11 | Bright Yellow |
| 12 | Bright Blue |
| 13 | Bright Magenta |
| 14 | Bright Cyan |
| 15 | Bright White |

Tap any swatch to open the color picker and set your preferred color for that ANSI slot. These colors affect how terminal programs render colored output (e.g., `ls` directory listings, `git diff` output, syntax-highlighted code).

### Step 4 --- Name and Save

1. Enter a name for your theme at the top of the editor
2. Tap **Save**
3. Your custom theme is now available in the theme list under Settings

---

## Editing a Custom Theme

1. Go to **Settings --> Terminal Theme**
2. Custom themes show a **pencil icon** next to their name
3. Tap the pencil icon to reopen the theme editor
4. Make your changes and tap **Save**

---

## Deleting a Custom Theme

1. Go to **Settings --> Terminal Theme**
2. If a custom theme is currently selected, a **Delete Custom Theme** option appears (in red)
3. Tap it to delete the theme
4. The terminal reverts to the Default Dark theme

---

## Tips

- **Start from a base:** If you like a built-in theme but want to tweak a few colors, note down the colors you like, create a new custom theme, and set those as your starting point
- **Hex input is fastest:** In the color picker, switch to the hex slider tab and paste hex codes from your favorite colorscheme (e.g., from Catppuccin, Dracula, or Nord documentation)
- **Test with real output:** After saving your theme, run some colorful terminal commands to verify the palette: `ls --color`, `git log --oneline --graph`, `htop`, or `cat` a syntax-highlighted file
- **Themes persist:** Custom themes are saved to UserDefaults and persist across app restarts. They are specific to your device and are not synced via iCloud
