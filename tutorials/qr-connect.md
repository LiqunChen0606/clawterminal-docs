# QR Connect — Tutorial

> Setting up a connection on a phone keyboard is the single most painful part of first-run setup: typing a Tailscale hostname, a port, and a username with no typos, on a touch screen. QR Connect collapses all of that into one scan. Set up a connection once on any device, show its QR code, and scan it on a second iPhone or iPad to import the host, port, and username instantly.

---

## Part 1: What QR Connect Does

A connection profile is just a few fields — hostname, port, username, and an auth method. QR Connect encodes those connection details into a QR code on one device so another device can read them in a single scan, instead of you retyping everything by hand.

Two actions, both in the connection list:

- **Share as QR** — generates a QR code for one profile.
- **Import via QR** — opens the camera to scan a QR code from another device.

The result on the new device is a pre-filled connection profile. You connect, and you're in.

---

## Part 2: Security — Your Key Stays Put by Default

This is the important part, so read it before you share anything.

**By default, your private key does NOT travel in the QR code.** Only the connection details (host, port, username) are encoded. On the new device you supply the auth yourself — enter the password, or pick/generate an SSH key on that device and authorize it on your Mac the usual way.

**Private-key sharing is strictly opt-in and off by default.** If you explicitly turn it on, ClawTerminal shows a loud warning before encoding the key, because a QR code carrying a private key is a credential anyone who photographs your screen can capture. Only enable it if:

- You're moving between two devices you both own and control, and
- You're somewhere private where nobody can photograph the screen, and
- You understand the QR image itself is now sensitive — don't save it, don't send it, don't leave it on screen.

For almost everyone, the default (connection details only, key entered on the new device) is the right choice.

---

## Part 3: Sharing a Profile (Device A)

1. On the device that already has a working connection, open the **Connections** tab
2. Find the profile you want to share in the connection list
3. Tap its **Share as QR** action (per-profile — each connection has its own code)
4. A QR code appears on screen
   - By default it encodes host, port, and username only
   - If you've opted into key sharing, you'll see the warning first and the QR will additionally carry the key
5. Leave the code on screen for the other device to scan

---

## Part 4: Importing a Profile (Device B)

1. On the second iPhone or iPad, open the **Connections** tab
2. Tap **Import via QR**
3. Grant camera permission if prompted (required to scan)
4. Point the camera at Device A's QR code
5. ClawTerminal reads the code and pre-fills a new connection profile with the host, port, and username
6. Supply the auth:
   - If the QR carried connection details only (the default), enter the password or select an SSH key on this device
   - If the QR carried the key (opt-in), the auth is already filled in
7. Tap the profile to connect

That's the whole flow — one scan replaces a screen full of careful typing.

---

## Part 5: When QR Connect Shines

- **Adding your iPad after your iPhone.** You set up the iPhone with Tailscale and a username; the iPad just scans and connects.
- **Onboarding a teammate to a shared dev box.** Share the connection details over QR; they enter their own credentials on their device.
- **Replacing a device.** New phone, same Mac — scan the old phone's code instead of redoing setup from memory.

---

## Tips

- **One code per profile.** "Share as QR" is per-connection, so you can share exactly the profile you mean — not your whole connection list.
- **Keep the key on the new device by default.** The safest pattern is: scan the connection details, then generate or pick an SSH key locally on the new device. The key never appears in any image.
- **Treat a key-bearing QR like a password.** If you opt into key sharing, the QR image is a live credential. Show it, scan it, dismiss it — never store or forward it.
- **Camera permission is required to import.** If the scanner won't open, check the camera permission for ClawTerminal in iOS Settings.
