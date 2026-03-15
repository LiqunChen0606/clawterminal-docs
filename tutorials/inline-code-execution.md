# Inline Code Execution

> Run code blocks from Claude's responses directly on your Mac with a single tap. See the output inline without switching to the terminal.

---

## How It Works

When Claude responds with a fenced code block in a supported language, a green **Run** button appears in the code block header (next to the existing **Copy** button). Tapping it sends the code to your Mac for execution and displays the result inline in the chat.

### Supported Languages

The Run button appears for code blocks tagged with these languages:

| Language tag | Execution method |
|-------------|-----------------|
| `bash`, `sh`, `zsh`, `shell` | Runs directly as a shell command |
| `python`, `python3` | Runs via `python3 -c "<code>"` |
| `ruby` | Runs via `ruby -e "<code>"` |
| `node`, `js` | Runs via `node -e "<code>"` |
| (no tag) | Runs as a shell command |

Code blocks tagged with other languages (e.g., `swift`, `go`, `java`, `html`, `css`) do not show the Run button because they cannot be directly executed as a one-liner.

---

## Step-by-Step Example

1. Ask Claude a question that results in runnable code:

   ```text
   How do I list all Python files in the current directory?
   ```

2. Claude responds with a code block:

   ````text
   ```bash
   find . -name "*.py" -type f
   ```
   ````

3. The code block header shows:
   - The language label (`bash`)
   - A **Copy** button
   - A green **Run** button

4. Tap **Run**

5. The code block briefly shows a **"Running..."** placeholder

6. After execution (30-second timeout), the result appears inline as a new output block:

   ```text
   ./src/main.py
   ./src/utils.py
   ./tests/test_main.py
   ```

---

## Context Menu Actions

In addition to the Run button, you can **long-press** (or right-click on iPad with a trackpad) any code block to access a context menu with three options:

| Action | Description |
|--------|-------------|
| **Run** | Execute the code on your Mac (same as the Run button) |
| **Copy** | Copy the code to the clipboard |
| **Save to Snippets** | Save the code block to your Code Snippet Library for later reuse |

---

## Execution Details

- **Timeout:** Code execution has a 30-second timeout. If the command does not complete within 30 seconds, it is terminated and a timeout message is shown
- **Working directory:** Code runs in the working directory set for the current chatroom (visible in Info --> Project directory)
- **SSH connection required:** The Run button only works when you have an active SSH connection to your Mac. In API mode without a Mac connection, the button does not appear
- **Output format:** The execution result replaces the "Running..." placeholder with a fenced code block containing stdout and stderr output

---

## Tips

- **Quick testing:** Use inline execution to quickly test Claude's suggestions before applying them to your project
- **Chain with Snap & Code:** Photograph a UI mockup, let Claude generate HTML, then tap Run to render it (if Claude generates a shell command to write and open the file)
- **Shell one-liners:** The Run button is especially useful for shell one-liners like `du -sh *`, `ps aux | grep node`, or `curl` commands
- **Python snippets:** Claude often suggests Python one-liners for data processing. The Run button makes it trivial to test them
- **Save useful snippets:** If Claude generates a command you will reuse, long-press the code block and tap **Save to Snippets** to keep it in your library
