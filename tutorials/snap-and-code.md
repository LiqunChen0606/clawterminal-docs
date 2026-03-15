# Snap & Code: Camera to Code

> Take a photo of a UI mockup, error screen, whiteboard diagram, or anything else, and Claude generates code from it. Combine with the inline Run button to go from camera to running code in three taps.

---

## What Is Snap & Code?

Snap & Code is a workflow that combines three ClawTerminal features:

1. **Camera capture** --- take a photo directly from the chatroom
2. **Claude vision** --- Claude analyzes the image and generates code
3. **Inline Run** --- tap the Run button on the generated code block to execute it immediately

The result: you can photograph a UI sketch on a napkin, and moments later have running HTML/CSS on your Mac.

---

## How to Use Snap & Code

### From an Empty Chatroom

When you open a chatroom with no messages, you will see starter prompt chips at the bottom. One of them is a purple **"Snap & Code"** chip. Tap it to launch the camera.

### From Any Message

You can also attach a photo at any time:

1. Tap the **attachment button** (paperclip icon) in the chat input bar
2. Select **Take Photo** from the options
3. The camera opens --- take a photo of whatever you want Claude to analyze
4. The photo is attached to your message, and the input field is pre-filled with a prompt template

### The Prompt Template

When you capture a photo via Snap & Code, the input field is pre-filled with:

```text
Look at this image and generate code that recreates or addresses what you see.
```

You can edit this prompt before sending to be more specific:

```text
Look at this image. It's a wireframe of a login page.
Generate a SwiftUI view that matches this layout.
```

---

## Step-by-Step Example

### Photographing a UI Mockup

1. Draw a login form on paper with fields for username, password, and a submit button
2. Open a ClawTerminal chatroom connected to your Mac
3. Tap the **"Snap & Code"** chip (or attachment button followed by **Take Photo**)
4. Point your camera at the sketch and tap the shutter button
5. The image appears as an attachment in the input bar
6. Optionally refine the prompt, then tap **Send**
7. Claude analyzes the image and generates code --- for example, an HTML page with a login form
8. The code block in Claude's response has a green **Run** button in the header
9. Tap **Run** to execute the code on your Mac

### Photographing an Error Screen

1. You see an error dialog on another device or a monitor
2. Take a photo of it using Snap & Code
3. Claude reads the error message from the image and explains the cause and fix

### Photographing a Whiteboard Diagram

1. After a design meeting, photograph the architecture diagram on the whiteboard
2. Send it to Claude with: "Generate a Mermaid diagram from this whiteboard sketch"
3. Claude produces a text-based Mermaid diagram you can paste into documentation

---

## Camera Permissions

The first time you use Snap & Code, iOS will ask for camera permission. If you deny it, you can re-enable it later in **Settings (iOS) --> ClawTerminal --> Camera**.

ClawTerminal's `Info.plist` includes the `NSCameraUsageDescription` key explaining that the camera is used for capturing images to send to Claude for code generation.

---

## Tips

- **Lighting matters:** Take photos in good lighting for best results. Claude's vision model can handle imperfect images, but clearer photos produce better code
- **Be specific:** Edit the pre-filled prompt to tell Claude exactly what language or framework you want. "Generate this as a React component" yields better results than a generic prompt
- **Combine with Run:** The generated code often includes shell commands or scripts. Use the green **Run** button to execute them directly without copy-pasting
- **Photos from the library:** If you already have a photo saved, use the attachment button and select **Photo Library** instead of **Take Photo**
- **Works in API mode too:** Snap & Code works in both CLI and API mode, since image attachments are supported in both paths
