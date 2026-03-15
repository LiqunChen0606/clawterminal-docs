# Using ClawTerminal on iPad

> ClawTerminal adapts its layout for iPad with a sidebar-based interface, supports multiple windows via Stage Manager, and shares SSH sessions across windows so you never duplicate connections.

---

## iPad Layout: NavigationSplitView

On iPad (and any device where the horizontal size class is `.regular`), ClawTerminal replaces the iPhone bottom tab bar with a **NavigationSplitView** sidebar:

| iPhone Layout | iPad Layout |
|---------------|-------------|
| Bottom tab bar with 5 tabs | Left sidebar with 5 navigation items |
| Full-screen content area | Split view: sidebar + detail content |

### Sidebar Items

The iPad sidebar mirrors the five tabs from the iPhone layout:

| Sidebar Item | Icon | Description |
|-------------|------|-------------|
| **My Mac** | Desktop computer | Connect to your Mac, Claude chatroom, terminal, files, tunnels |
| **My Linux** | Server rack | Connect to a Linux server with the same features |
| **Terminal** | Terminal | Standalone multi-tab SSH terminal |
| **Connections** | Network | Manage SSH connection profiles |
| **Settings** | Gear | App configuration, SSH keys, MCP servers, themes |

Tap any sidebar item to switch the detail panel on the right.

---

## Opening Multiple Windows (Stage Manager)

iPadOS Stage Manager lets you open multiple instances of ClawTerminal side by side. This is useful for workflows like:

- **Claude chatroom** in one window + **raw terminal** in another
- **File browser** in one window + **code discussion** in another
- Two chatrooms open simultaneously for different projects

### How to Open a Second Window

1. **From the Dock:** Long-press the ClawTerminal icon in the Dock and drag it to the side of the screen to create a split view, or drop it onto the desktop to create a new window
2. **From the App Switcher:** Tap the three-dot menu (**...**) at the top of the ClawTerminal window, then select **Add Window**
3. **From Stage Manager:** Drag the ClawTerminal icon from the shelf on the left to open a new instance

Each window is independent --- you can navigate to different sidebar items, open different chatrooms, or view different files in each window.

---

## Shared SSH Sessions Across Windows

When you open multiple iPad windows, ClawTerminal ensures that they **share the same SSH connection** to your Mac rather than opening duplicate connections. This is handled by the **SharedSessionRegistry**, a process-wide singleton that manages session instances:

### How It Works

1. When a window connects to a Mac profile, the `MyMacSessionManager` for that connection is registered in the `SharedSessionRegistry` using the profile's UUID
2. When a second window opens, it looks up the registry for an existing manager with the same profile UUID
3. If a shared manager exists, the second window reuses it --- same SSH connection, same chatroom state, same job store
4. If no manager exists (e.g., the second window connects to a different profile), a new independent session is created

### What Is Shared

| Shared across windows | Independent per window |
|-----------------------|-----------------------|
| SSH connection to the Mac | Currently selected sidebar item |
| Chatroom list and messages | Window size and position |
| Background jobs and their status | Scroll position in lists |
| SFTP file browser state | |
| MCP server connections | |

### What This Means in Practice

- If Claude is streaming a response in one window, switching to the same chatroom in another window shows the same live stream
- A background job submitted in one window appears in the Jobs panel of the other window
- Disconnecting from SSH in one window disconnects all windows using that profile

---

## Split-Screen and Slide Over

ClawTerminal supports all standard iPadOS multitasking modes:

| Mode | How to activate | Best for |
|------|----------------|----------|
| **Full screen** | Default | Focused single-task work |
| **Split view** | Drag from Dock or use **...** menu | Side-by-side with another app (e.g., Safari for docs) |
| **Slide Over** | Drag app to the right edge | Quick reference while working in another app |
| **Stage Manager** | Requires iPadOS 16+ and supported hardware | Multiple resizable floating windows |

The sidebar automatically collapses in narrow split-view configurations and expands when the window is wide enough.

---

## Keyboard and Trackpad Support

On iPad with an attached keyboard (Magic Keyboard, Smart Keyboard Folio, or any Bluetooth keyboard), ClawTerminal provides:

- **Full physical keyboard input** in the terminal --- all key combinations work as expected, including Ctrl+C, Ctrl+D, and function keys
- **Trackpad/mouse support** --- click to position the cursor in the terminal, scroll through output, and interact with all UI elements
- The **extended keyboard bar** (Shell / Tmux / Git / Vim shortcuts) remains available above the software keyboard when the software keyboard is shown

---

## Tips for iPad Users

- **Use the sidebar for quick navigation:** Switching between My Mac, Terminal, and Settings is faster with the sidebar than the tab bar
- **Stage Manager + Claude:** Keep a Claude chatroom in one window and a terminal in another for a desktop-like development workflow
- **Landscape orientation:** The sidebar layout works best in landscape. In portrait, the sidebar may overlay the content --- swipe to dismiss it
- **External display:** ClawTerminal supports external display output via Stage Manager. Connect your iPad to a monitor for even more screen space
- **Split view with Safari:** Open ClawTerminal in split view with Safari to reference documentation while coding through Claude
