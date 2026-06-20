# Connect Claude Without Port Setup (Beta) — Examples

> Channels is an opt-in **Beta** transport that drives a Claude chatroom on your Mac through a documented local bridge instead of an SSH port forward — removing inbound-network setup. Honest framing: it removes the *network* setup, not the *Mac*. `claude` still runs on your machine, and SSH stays the default for the terminal, file browser, and other AI tools.

## Quick Start

1. From an SSH-connected chatroom: `/channels install` (requires a recent Claude Code)
2. Read the confirmation card — it prints the `claude` launch command + development flag
3. Confirm and let it install over SSH
4. Enable it in **Settings → AI Intelligence** (off by default)

---

## What Channels does and doesn't do

```
Channels affects:   the Claude chatroom transport (bridge instead of SSH port forward)

Channels does NOT:  remove your Mac — claude still runs locally
                    replace SSH — terminal, SFTP files, Codex/Gemini/Aider stay on SSH
                    leave Beta — it's a research-preview feature, off by default
```

---

## 1. Install the Mac side

```
/channels install
```

A confirmation card opens showing exactly what runs on your Mac, including the development flag needed to launch `claude` in research-preview mode. Read it, then confirm. The install dispatches over your existing SSH connection.

**Requires:** a recent version of Claude Code on your Mac. If the card warns about your version, update Claude Code first.

---

## 2. Enable the transport

```
Settings → AI Intelligence → Channels transport → ON
```

Channels is **off by default**. With it on, the Claude chatroom uses the local bridge. Everything else keeps using SSH.

To revert:

```
Settings → AI Intelligence → Channels transport → OFF
```

SSH resumes immediately as the chatroom transport.

---

## 3. Choosing Channels vs SSH

```
Want the most stable, fully-supported path        → SSH (default)
Need terminal / file browser / Codex/Gemini/Aider → SSH (Channels doesn't cover these)
SSH port forwarding is your only chatroom hurdle  → Channels (Beta) removes the network step
Comfortable with a research-preview dev flag      → Channels is worth a try
Want guaranteed, no-surprise behavior             → SSH until Channels leaves Beta
```

---

## Notes

- **Keep SSH configured even with Channels on.** The terminal, file browser, and other AI tools all need it — and it's your instant fallback.
- **It's Beta.** Expect rough edges. If the chatroom misbehaves, flip the Settings toggle off to fall back to SSH.
- **The card is where the honesty lives.** Because Channels needs `claude` launched with a development flag, the install card spells out exactly what runs on your Mac. Read it before confirming.
- **Removes network setup, not the Mac.** `claude` still runs on your machine; Channels only changes how ClawTerminal reaches it.
