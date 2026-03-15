# Conversation Export (PDF and Markdown)

> Save your Claude conversations as Markdown files or formatted PDFs for documentation, sharing, or archival.

---

## Why Export Conversations?

Long conversations with Claude often contain valuable information: architecture decisions, code reviews, debugging sessions, step-by-step guides. Exporting lets you:

- **Save for reference** --- keep a record of important decisions and Claude's reasoning
- **Share with teammates** --- send a PDF of Claude's code review to your team
- **Document your work** --- archive what Claude built during a session
- **Move to other tools** --- import Markdown into Notion, Obsidian, or a wiki

---

## How to Export

There are two ways to export a conversation:

### Method 1: Chat Toolbar Button

1. Open the chatroom you want to export
2. In the scrollable toolbar at the top of the chat (alongside Memory, Skills, Jobs, Snippets), tap **Export**
3. A dialog appears with two options:
   - **Export as Markdown**
   - **Export as PDF**
4. Choose your format
5. The iOS **share sheet** opens --- save to Files, AirDrop to another device, or share via any app

### Method 2: Room Chip Context Menu

1. In the chatroom, **long-press** the room name chip (the tab at the top that shows the room name)
2. Select **Export Conversation** from the context menu
3. Choose **Export as Markdown** or **Export as PDF**
4. The share sheet opens

---

## Export Formats

### Markdown

The Markdown export serializes all messages in the conversation into a clean, readable text file:

- **User messages** are prefixed with `## User`
- **Claude responses** are preserved with full Markdown formatting (code blocks, headings, lists, bold, italic)
- **Tool calls** are summarized (tool name, input, output)
- **Timestamps** are included for each message

The resulting `.md` file is compatible with any Markdown editor or viewer.

### PDF

The PDF export renders the conversation into a formatted multi-page A4 document:

- Uses **CoreText** (`CTFramesetter`) for proper text layout and pagination
- Each message is clearly labeled with the sender (User or Claude)
- Code blocks are rendered in a monospaced font
- Pages break cleanly between messages when possible

---

## What Gets Exported

| Included | Not Included |
|----------|-------------|
| All user messages | Room context document (system prompt) |
| All Claude responses | Skills configuration |
| Tool call summaries | Active model setting |
| Message timestamps | Thinking/reasoning content |
| Code blocks with formatting | Image attachments |

---

## Tips

- **Export before clearing:** If you plan to use `/clear` to reset a conversation, export it first to preserve the history
- **Markdown for editing:** Choose Markdown if you want to edit, search, or annotate the conversation later
- **PDF for sharing:** Choose PDF if you are sending the conversation to someone who may not have a Markdown viewer
- **Regular exports:** For important projects, export periodically --- ClawTerminal stores conversations in memory and on the device, but an exported file is an independent backup
- **File naming:** Exported files include the room name and a timestamp, making them easy to organize
