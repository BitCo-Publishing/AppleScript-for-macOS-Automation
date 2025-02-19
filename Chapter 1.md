
# Clear Temporary Files

```bash
#!/bin/bash
rm -rf ~/Library/Caches/*
echo "Cache cleared!"
```

---

# Automate Email Sending in Mail

```applescript
tell application "Mail"
    set newMessage to make new outgoing message with properties {subject:"Hello!", content:"This is an automated email.", visible:true}
    tell newMessage
        make new to recipient at end of to recipients with properties {address:"example@example.com"}
        send
    end tell
end tell
```

---

# Rename and Move a File

```applescript
tell application "Finder"
    set name of file "screenshot.png" of desktop to "Meeting_Notes.png"
    move file "Meeting_Notes.png" of desktop to folder "Screenshots" of documents folder
end tell
```

---

# Backup and Notification Workflow

1. **Step 1: Terminal script compresses files:**
    ```bash
    tar -czf ~/Documents/Backup.tar.gz ~/Documents/Work
    ```

2. **Step 2: AppleScript sends an email notification:**
    ```applescript
    tell application "Mail"
        set newMessage to make new outgoing message with properties {subject:"Backup Complete", content:"Your files have been backed up successfully."}
        send newMessage
    end tell
    ```
```
