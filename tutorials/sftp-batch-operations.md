# SFTP Batch Operations

> Select multiple files at once to download or delete them in bulk, instead of handling files one at a time.

---

## Enabling Multi-Select Mode

1. Connect to your Mac or server via SSH
2. Navigate to the **Files** tab (My Mac --> Files sub-tab)
3. Tap the **Select** toggle button in the breadcrumb bar at the top of the file browser

The file browser switches to multi-select mode: each file and folder row shows a **checkmark circle** on the left side.

---

## Selecting Files

In multi-select mode:

| Action | How |
|--------|-----|
| Select a single item | Tap its row --- the circle fills with a checkmark |
| Deselect an item | Tap it again --- the checkmark clears |
| Select all items | Tap **Select All** in the batch action bar at the bottom |
| Deselect all items | Tap **Deselect All** in the batch action bar |

> **Note:** When you navigate to a different directory, the selection is automatically cleared. Multi-select applies to the current directory only.

---

## Batch Actions

When you have one or more items selected, the **batch action bar** at the bottom of the screen provides two actions:

### Batch Download

1. Select the files you want to download (folders are excluded from batch download)
2. Tap **Download** in the action bar
3. A progress indicator appears as the files are transferred via SFTP
4. Files are saved to your device and available in the iOS Files app

> **Note:** Batch download only applies to files, not folders. If you select a mix of files and folders, only the files will be downloaded.

### Batch Delete

1. Select the files and/or folders you want to delete
2. Tap **Delete** in the action bar
3. A **confirmation dialog** appears showing how many items will be deleted
4. Tap **Delete** to confirm, or **Cancel** to abort

> **Warning:** Batch delete is permanent. Deleted files cannot be recovered from the SFTP server unless you have a backup.

---

## Exiting Multi-Select Mode

Tap the **Select** toggle button again in the breadcrumb bar to exit multi-select mode and return to normal file browsing.

---

## Tips

- **Preview before deleting:** If you are unsure about a file, tap it in normal mode first to preview its contents before switching to multi-select for deletion
- **Navigating clears selection:** If you accidentally navigate into a subfolder, your selection in the parent directory is lost. Select and act on files before navigating away
- **Large batch downloads:** For many files, the download runs sequentially. Large transfers may take a moment --- the progress indicator shows the current status
- **Use with SFTP upload:** Batch operations complement the existing single-file upload feature. To upload multiple files, use the Upload button (top right) and select multiple files from the iOS Files picker
