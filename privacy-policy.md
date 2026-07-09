# ClawTerminal Privacy Policy

**Effective Date:** July 8, 2026
**App:** CatClaw Terminal (ClawTerminal) — SSH Terminal + AI Chatroom
**Developer:** Liqun Chen
**Contact:** [Open an issue](https://github.com/LiqunChen0606/clawterminal-docs/issues)

---

## Overview

ClawTerminal is designed with privacy as a core principle. All data stays on your device. We do not operate any servers that receive your personal data, and we do not use analytics SDKs, advertising networks, or third-party tracking of any kind.

The only data that leaves your device is content you explicitly send to AI providers (Anthropic, OpenAI, Google) when using the AI chatroom, or to servers you configure yourself (SSH hosts, relay server).

Before any data is sent to a third-party AI provider for the first time, ClawTerminal shows an in-app consent screen that discloses what is sent and who it is sent to, and asks for your permission. AI features are not enabled until you agree, and you can review or withdraw this consent at any time in Settings. On-device processing via Apple Intelligence (iOS 26) is the exception — that content stays on your device and is never sent to a third party.

---

## 1. Data We Collect and Store

All data listed below is stored **locally on your device only** and is never transmitted to us.

| Data | Where Stored | Purpose |
| --- | --- | --- |
| SSH credentials (passwords, private keys) | iOS Keychain | Authenticate SSH connections |
| AI API keys (Anthropic, OpenAI, Google) | iOS Keychain | Authenticate AI API requests |
| Connection profiles (hostname, port, username) | SwiftData (local) | Save and recall SSH connections |
| Chat history and messages | App sandbox (`chatrooms.json`) | Persist conversations across sessions |
| Cross-session AI memory | App sandbox (`memory_store.json`) | Remember user preferences and project context |
| Custom skills and commands | App sandbox (`skills.json`, `commands.json`) | User-defined automation |
| Code snippets | App sandbox (`snippets.json`) | Saved code blocks |
| App settings and preferences | UserDefaults | Persist your configuration |

**We never see, store, or transmit any of this data.**

---

## 2. Data Sent to Third Parties

### AI Providers (Anthropic, OpenAI, Google)

When you use the AI chatroom in API mode, ClawTerminal sends your messages directly from your device to the provider's API. This may include:

- Your chat messages and conversation history
- File attachments you add to messages (images, PDFs, text files)
- System context you configure (skills, memory, project context)
- Tool results when MCP servers are in use

This data is sent directly to the provider — it does not pass through any ClawTerminal server. Each provider's data handling is governed by their own privacy policy:

- [Anthropic Privacy Policy](https://www.anthropic.com/privacy)
- [OpenAI Privacy Policy](https://openai.com/privacy)
- [Google Privacy Policy](https://policies.google.com/privacy)

**CLI mode (via your Mac).** When you use the AI chatroom in CLI mode, ClawTerminal sends your message over SSH to the Mac or Linux host you configure, where an AI command-line tool you installed (Claude Code, Codex, Gemini, or Aider) processes it. That tool sends the same categories of data listed above to the corresponding provider (Anthropic, OpenAI, or Google) from your machine. ClawTerminal has no server in this path.

**On-device (Apple Intelligence).** On supported devices running iOS 26 with Apple Intelligence, certain features (Save as Skill, /standup, /whatif) can run entirely on-device using Apple's Foundation Models framework. In that case your content stays on the device and is not sent to any third party.

**Consent.** ClawTerminal requests your permission in-app, with a disclosure of what is sent and to whom, before sending your data to a third-party AI provider for the first time. You can withdraw consent at any time in Settings, which disables the AI features that require it.

### SSH Hosts (User-Configured)

When you connect to a Mac or Linux server, ClawTerminal communicates with the host you specify via SSH. Terminal commands, AI CLI interactions, and file operations run on that host. We have no visibility into these connections.

### Relay Server (Optional, User-Configured)

If you configure the optional relay server for shared chatrooms, messages are routed through the relay URL you specify. We do not operate a relay server — you host your own.

### MCP Servers (Optional, User-Configured)

If you configure MCP (Model Context Protocol) servers, ClawTerminal will connect to the URLs you specify. You are responsible for reviewing the privacy practices of any MCP server you configure.

### No Other Third Parties

ClawTerminal does not include:

- Analytics or crash reporting SDKs
- Advertising networks
- Social login or third-party authentication
- Any other data-sharing integrations

---

## 3. Device Permissions

ClawTerminal requests the following iOS permissions:

| Permission | When Requested | Why |
| --- | --- | --- |
| **Microphone** | When you tap the voice input button | Transcribe your voice into text for chat messages |
| **Speech Recognition** | When you tap the voice input button | Convert speech to text using Apple's on-device recognition |
| **Camera** | When you use Screenshot → Code → Run | Take a photo to generate code from |
| **Local Network** | On first launch or first connection attempt | Discover your Mac via Bonjour for automatic SSH setup |
| **Face ID / Touch ID** | When you enable app lock in Settings | Lock the app and protect your credentials |
| **Notifications** | When you send your first chat message | Alert you when tasks complete or responses arrive in the background |
| **Background App Refresh** | Automatic | Check for running background jobs and scheduled tasks while the app is minimized |

All permissions are optional. The app functions without any of them (with the corresponding features disabled).

---

## 4. Children's Privacy

ClawTerminal is not directed at children under the age of 13 and is not intended for use by children. We do not knowingly collect personal information from anyone under 13.

---

## 5. Data Retention and Deletion

- **SSH credentials and API keys:** Stored in the iOS Keychain. Deleted when you remove them in Settings or uninstall the app.
- **Connection profiles:** Stored locally in SwiftData. Deleted when you remove profiles in the app or uninstall the app.
- **Chat history and memory:** Stored in app sandbox files. Deleted when you clear them in the app or uninstall the app.
- **App settings:** Stored in UserDefaults. Reset when you uninstall the app.

To delete all data, uninstall ClawTerminal from your device.

---

## 6. iCloud

Connection profiles may sync via iCloud if enabled on your device. No other app data syncs to iCloud. You can disable iCloud sync for ClawTerminal in iOS Settings.

---

## 7. Security

- SSH credentials and API keys are stored exclusively in the iOS Keychain with `kSecAttrAccessibleWhenUnlockedThisDeviceOnly` protection.
- All SSH connections use standard encrypted SSH protocol (via the Citadel / SwiftNIO SSH library).
- AI API calls use HTTPS (TLS) exclusively.
- No credentials are logged or written to disk outside the Keychain.

---

## 8. Changes to This Policy

We may update this Privacy Policy from time to time. When we do, we will update the Effective Date above and post the revised policy at this URL. Continued use of ClawTerminal after changes are posted constitutes your acceptance of the updated policy.

---

## 9. Contact

If you have questions about this Privacy Policy, please [open an issue](https://github.com/LiqunChen0606/clawterminal-docs/issues) in the ClawTerminal docs repository.

---

*CatClaw Terminal — SSH + AI for iOS & iPad*
