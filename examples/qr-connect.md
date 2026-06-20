# QR Connect — Examples

> Set up a connection once, show its QR code, scan it on another iPhone or iPad to import host, port, and username in one shot. Private-key sharing is opt-in and off by default — the safe default sends connection details only.

## Quick Start

1. **Device A:** Connections tab → find the profile → **Share as QR**
2. **Device B:** Connections tab → **Import via QR** → grant camera permission → scan
3. **Device B:** supply auth (password or local SSH key), then tap to connect

---

## Two actions, both in the connection list

```
Share as QR   — per-profile; generates a QR code for one connection
Import via QR — opens the camera to scan a code from another device (camera permission required)
```

---

## 1. Move a connection from iPhone to iPad

On the iPhone (Device A):

```
Connections tab → tap the profile → Share as QR
```

A QR code appears, encoding host + port + username (the default — no key).

On the iPad (Device B):

```
Connections tab → Import via QR → scan the iPhone's screen
```

The iPad pre-fills a new profile with the host, port, and username. Enter the password or pick a local SSH key, then connect.

---

## 2. The safe default — connection details only

By default the QR carries **only** the connection details:

```
Encoded:  hostname, port, username
NOT encoded:  your private key
```

On the new device you supply the auth yourself — enter the password, or generate/select an SSH key on that device and authorize it on your Mac as usual. The key never appears in any image. This is the recommended path for almost everyone.

---

## 3. Opt-in key sharing (use with care)

Private-key sharing is **off by default**. If you explicitly enable it before generating the QR, ClawTerminal shows a loud warning first, because the QR image then carries a live credential:

```
⚠  This QR code will contain your private key.
    Anyone who photographs the screen can capture it.
    Only do this on devices you control, somewhere private.
```

Only opt in when:

- Both devices are yours, and
- You're somewhere nobody can photograph the screen, and
- You'll dismiss the code immediately and never save or forward the image

For everyone else, leave key sharing off and use Scenario 2.

---

## 4. Onboard a teammate to a shared dev box

```
Device A (you):    Share as QR   → connection details only
Device B (them):   Import via QR → scan → enter their own credentials
```

They get the host/port/username instantly without you reading it out loud, and they still bring their own auth — your key never leaves your device.

---

## Notes

- **Per-profile codes.** "Share as QR" shares exactly one connection, not your whole list.
- **Camera permission required to import.** If the scanner won't open, enable camera access for ClawTerminal in iOS Settings.
- **Treat a key-bearing QR like a password.** If you opt into key sharing, show it, scan it, dismiss it — never store or send the image.
- **Default = key stays on the new device.** Scan the details, then add an SSH key locally on the importing device for the most secure setup.
