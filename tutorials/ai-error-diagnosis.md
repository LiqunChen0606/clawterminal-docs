# AI Error Diagnosis

> When a command fails in the terminal, ClawTerminal detects the error and offers a floating "Ask Claude" button that sends the error context to Claude for instant diagnosis.

---

## How It Works

ClawTerminal continuously monitors terminal output for error patterns. When it detects a failure, a floating **"Ask Claude"** pill appears in the bottom-right corner of the terminal view.

### Detection

The terminal session maintains a 200-line ring buffer of recent output. ClawTerminal watches this buffer for approximately 30 common error patterns, including:

| Pattern | Example |
|---------|---------|
| `command not found` | `zsh: command not found: npx` |
| `permission denied` | `bash: /usr/local/bin/app: Permission denied` |
| `No such file or directory` | `ls: /foo/bar: No such file or directory` |
| `segmentation fault` | `Segmentation fault: 11` |
| Stack traces | Python tracebacks, Node.js stack traces |
| Build errors | `error: cannot find module`, `fatal error:` |
| Git errors | `fatal: not a git repository` |
| Network errors | `Connection refused`, `Could not resolve host` |

### The "Ask Claude" Pill

When an error is detected:

1. A floating purple pill labeled **"Ask Claude"** animates into view at the bottom-right of the terminal
2. The pill auto-dismisses after **10 seconds** if you do not tap it
3. The pill appears on both the **My Mac** raw terminal view and the standalone **Terminal** tab

### Tapping the Pill

When you tap the pill:

1. ClawTerminal captures the **last 30 lines** of terminal output, with ANSI escape sequences stripped for readability
2. The app switches to **Claude mode** (the chatroom)
3. A diagnosis prompt is automatically composed and sent to Claude:

```text
I got an error in my terminal. Here's the recent output:

```
[last 30 lines of terminal output]
```

Please explain what went wrong and suggest how to fix it.
```

4. Claude analyzes the error and responds with an explanation and fix

---

## Step-by-Step Example

1. You are in the terminal and run a command that fails:

   ```bash
   npm run build
   ```

   Output:

   ```text
   > myapp@1.0.0 build
   > tsc && vite build

   error TS2307: Cannot find module 'react-router-dom'
   ```

2. The **"Ask Claude"** pill appears in the bottom-right corner

3. Tap the pill --- the app switches to the Claude chatroom

4. Claude responds with something like:

   > The TypeScript compiler cannot find the `react-router-dom` module. This usually means the package is not installed. Run:
   >
   > ```bash
   > npm install react-router-dom
   > npm install --save-dev @types/react-router-dom
   > ```
   >
   > Then try `npm run build` again.

---

## Where It Appears

The "Ask Claude" pill overlay appears in two places:

| Location | When |
|----------|------|
| **My Mac** tab (raw terminal mode) | When connected to your Mac and using the terminal view |
| **Terminal** tab (standalone terminal) | When connected to any SSH server via a connection profile |

In both cases, tapping the pill switches to the **My Mac** Claude chatroom. If you are not connected to your Mac, the pill will not appear on standalone terminal tabs (since there is no Claude session to send the diagnosis to).

---

## Tips

- **Act quickly:** The pill disappears after 10 seconds. If you miss it, you can always copy-paste the error into the chatroom manually
- **Context is key:** The 30-line capture window usually includes enough context for Claude to diagnose the issue. If the error output is very long, Claude may ask for additional context
- **Works with any language:** Error detection covers Python, Node.js, Go, Rust, C/C++, Java, Ruby, shell scripts, Docker, git, and general system errors
- **CLI mode recommended:** For the best diagnosis, make sure your chatroom is in CLI mode (not API mode) so Claude can also inspect files on your Mac to understand the full context
