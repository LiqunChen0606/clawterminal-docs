# Connect Claude Without Port Setup (Beta) — Tutorial

> Getting a Claude chatroom working over the network usually means an SSH port forward — and for some setups, configuring inbound network access on your Mac. Channels is an opt-in **Beta** transport that drives a Claude chatroom on your Mac through a documented local bridge instead of an SSH port forward, removing that inbound-network setup. Honest framing up front: it removes the *network* setup, not the *Mac*. `claude` still runs on your machine. SSH stays the default and keeps powering everything else.

---

## Part 1: What Channels Is — and Isn't

**What it is:** an alternative transport for the Claude chatroom. Instead of reaching `claude` through an SSH port forward, ClawTerminal talks to it through a documented local bridge on your Mac. That removes the inbound-network configuration some users have to do for port forwarding.

**What it is NOT:**

- It is **not** "Claude without a Mac." `claude` still runs on your machine — Channels just changes how ClawTerminal reaches it.
- It is **not** a replacement for SSH. SSH remains the default and continues to power the terminal, the SFTP file browser, and the other AI tools (Codex, Gemini, Aider). Channels only affects the Claude chatroom transport.
- It is **not** finished. This is a **Beta / research-preview** feature, off by default. Treat it as experimental.

If you're happy with SSH, you don't need Channels at all.

---

## Part 2: Requirements

- A **recent version of Claude Code** on your Mac — Channels relies on a research-preview capability that older versions don't have.
- The Mac-side bridge, installed via `/channels install`.
- Because it's a research-preview feature, running it requires launching `claude` with a development flag. The installer prints the exact flag and command for you — read it before you run anything.

---

## Part 3: Install the Mac Side

From any chatroom that's connected to your Mac over SSH:

```
/channels install
```

ClawTerminal opens a confirmation card showing exactly what it will run on your Mac, including the development flag needed to launch `claude` in research-preview mode. Read the card. It tells you:

- What gets installed on the Mac side
- The exact `claude` launch command + development flag the bridge needs
- That this is a Beta capability requiring a recent Claude Code

If it looks right, confirm and let it run. The work dispatches over your existing SSH connection, so SSH is still doing the install — you're just setting up an alternate path for the chatroom afterward.

---

## Part 4: Enable It in Settings

Once the Mac side is installed:

1. Open **Settings → AI Intelligence**
2. Find the Channels transport row
3. Turn it on

Channels is **off by default** — you opt in here. With it enabled, the Claude chatroom uses the local bridge instead of an SSH port forward. Everything else in the app — terminal, files, Codex/Gemini/Aider — continues to use SSH exactly as before.

To go back, turn the toggle off. SSH resumes immediately as the chatroom transport.

---

## Part 5: When to Use Channels vs SSH

| Situation | Use |
|-----------|-----|
| You want the most stable, fully-supported path | **SSH** (the default) |
| You need the terminal, file browser, or Codex/Gemini/Aider | **SSH** (Channels doesn't cover these) |
| SSH port forwarding is the only setup hurdle you're hitting for the Claude chatroom | **Channels** (Beta) — removes that inbound-network step |
| You're comfortable running a research-preview feature with a development flag | **Channels** is worth a try |
| You want guaranteed behavior with no surprises | **SSH** until Channels leaves Beta |

The honest summary: Channels removes a *network-setup* hurdle for the Claude chatroom. It doesn't remove your Mac, and it doesn't replace SSH for everything else. SSH stays the dependable backbone; Channels is an experimental convenience for one specific transport.

---

## Tips

- **Keep SSH set up even if you enable Channels.** The terminal, file browser, and other AI tools all need it, and you'll want it as a fallback.
- **Read the install card carefully.** Because Channels needs `claude` launched with a development flag, the card is the place to understand exactly what runs on your Mac.
- **It's Beta — expect rough edges.** If the chatroom misbehaves with Channels on, flip the Settings toggle off to fall back to SSH instantly.
- **Recent Claude Code only.** If `/channels install` warns about your Claude Code version, update Claude Code on your Mac first.
