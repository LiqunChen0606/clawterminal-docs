# ClawTerminal Privacy Policy

**Effective Date:** February 25, 2026
**App:** ClawTerminal — SSH Terminal + Claude AI
**Developer:** Liqun Chen
**Contact:** [Open an issue](https://github.com/LiqunChen0606/clawterminal-docs/issues)

---

## Overview

ClawTerminal is designed with privacy as a core principle. Almost all data stays on your device. We do not operate any servers that receive your personal data, and we do not use analytics SDKs, advertising networks, or third-party tracking of any kind.

The only data that ever leaves your device is the content you explicitly send to Claude AI (Anthropic's API) when using the Claude chatroom feature.

---

## 1. Data We Collect and Store

All data listed below is stored **locally on your device only** and is never transmitted to us.

| Data | Where Stored | Purpose |
| --- | --- | --- |
| SSH credentials (passwords, private keys) | iOS Keychain | Authenticate SSH connections |
| Anthropic API key | iOS Keychain | Authenticate Claude API requests |
| Connection profiles (hostname, port, username) | SwiftData (local) | Save and recall SSH connections |
| App settings and preferences | UserDefaults | Persist your configuration |
| Chat history | In-memory only | Display conversation in session |

**We never see, store, or transmit any of this data.**

---

## 2. Data Sent to Third Parties

### Anthropic (Claude AI)

When you use the Claude AI chatroom in API mode, ClawTerminal sends your messages directly from your device to Anthropic's API (`api.anthropic.com`). This may include:

- Your chat messages and conversation history
- File attachments you add to messages (images, PDFs, text files)
- System context you configure (context documents, skills)
- Tool results when MCP servers are in use

This data is sent directly to Anthropic — it does not pass through any ClawTerminal server. Anthropic's data handling is governed by the [Anthropic Privacy Policy](https://www.anthropic.com/privacy) and [Usage Policy](https://www.anthropic.com/usage-policy).

### MCP Servers (User-Configured)

If you configure MCP (Model Context Protocol) servers, ClawTerminal will connect to the URLs you specify. We have no control over or visibility into those servers. You are responsible for reviewing the privacy practices of any MCP server you configure.

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
| **Microphone** | When you tap the voice input button | Transcribe your voice into text for Claude messages |
| **Speech Recognition** | When you tap the voice input button | Convert speech to text using Apple's on-device recognition |
| **Local Network** | On first launch or first connection attempt | Discover your Mac via Bonjour for automatic SSH setup |
| **Face ID / Touch ID** | When you enable app lock in Settings | Lock the app and protect your SSH sessions and API key |
| **Notifications** | When you send your first chat message | Alert you when Claude's response arrives while the app is in the background |

All permissions are optional. The app functions without any of them (with the corresponding features disabled).

---

## 4. Children's Privacy

ClawTerminal is not directed at children under the age of 13 and is not intended for use by children. We do not knowingly collect personal information from anyone under 13.

---

## 5. Data Retention and Deletion

- **SSH credentials and API key:** Stored in the iOS Keychain. Deleted when you remove them in Settings or uninstall the app.
- **Connection profiles:** Stored locally in SwiftData. Deleted when you remove profiles in the app or uninstall the app.
- **Chat history:** Stored in memory only. Not persisted to disk. Cleared when the app is terminated.
- **App settings:** Stored in UserDefaults. Reset when you uninstall the app.

To delete all data, uninstall ClawTerminal from your device.

---

## 6. iCloud

iCloud sync for connection profiles is not available in version 1.0. Connection profiles are stored locally only. A future update may add optional iCloud sync; this policy will be updated at that time.

---

## 7. Security

- SSH credentials and your Anthropic API key are stored exclusively in the iOS Keychain with `kSecAttrAccessibleWhenUnlockedThisDeviceOnly` protection.
- All SSH connections use standard encrypted SSH protocol (via the Citadel / SwiftNIO SSH library).
- Claude API calls use HTTPS (TLS) exclusively.
- No credentials are logged or written to disk outside the Keychain.

---

## 8. Changes to This Policy

We may update this Privacy Policy from time to time. When we do, we will update the Effective Date above and post the revised policy at this URL. Continued use of ClawTerminal after changes are posted constitutes your acceptance of the updated policy.

---

## 9. Contact

If you have questions about this Privacy Policy, please [open an issue](https://github.com/LiqunChen0606/clawterminal-docs/issues) in the ClawTerminal docs repository.

---

*ClawTerminal — SSH + Claude AI for iOS*
