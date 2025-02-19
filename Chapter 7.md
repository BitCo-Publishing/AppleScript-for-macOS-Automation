
# macOS Backup Solutions: AppleScript, Shell Scripts, and Automation

This guide provides a comprehensive set of tools for automating backups on macOS. It includes AppleScripts, shell scripts, and instructions for scheduling backups using `launchd`.

---

## **AppleScript for File Backup**

This script backs up important folders (e.g., `Documents` and `Desktop`) to an external drive.

```applescript
set backupFolder to "/Volumes/BackupDrive/Personal_Backup"
set sourceFolders to {"/Users/YOUR_USERNAME/Documents", "/Users/YOUR_USERNAME/Desktop"}

repeat with folderPath in sourceFolders
    set shellCommand to "rsync -av --exclude='.DS_Store' " & quoted form of folderPath & " " & quoted form of backupFolder
    do shell script shellCommand
end repeat
```

### **Key Features**
- Uses `rsync` for efficient file synchronization.
- Excludes `.DS_Store` files to avoid clutter.
- Loops through multiple source folders for backup.

---

## **Shell Script for Automating Time Machine Backups**

This script checks if Time Machine is running and starts a backup if it isn't.

```bash
#!/bin/bash
# Define the backup drive name
BACKUP_DRIVE="Time Machine"

# Check if Time Machine is enabled
tmutil status | grep "Running = 1" > /dev/null
if [ $? -eq 0 ]; then
    echo "Time Machine is already running."
else
    echo "Starting Time Machine backup..."
    tmutil startbackup
fi
```

### **Key Features**
- Checks the status of Time Machine using `tmutil`.
- Starts a backup only if one isn't already running.

---

## **Shell Script for Cloud Backup (Google Drive Example)**

This script syncs local files to a Google Drive folder using `rsync`.

```bash
# Define local and cloud paths
LOCAL_FOLDER="$HOME/Documents/Important"
CLOUD_FOLDER="$HOME/Google Drive/My Backups"

# Sync files
rsync -av --delete "$LOCAL_FOLDER/" "$CLOUD_FOLDER/"
echo "Cloud backup completed successfully!"
```

### **Key Features**
- Uses `rsync` to synchronize files between local and cloud folders.
- The `--delete` flag ensures that deleted files in the source are also removed from the destination.

---

## **Shell Script for macOS Configuration Backup**

This script backs up system configurations, including Terminal profiles, Zsh settings, and VS Code preferences.

```bash
#!/bin/bash

# Backup zsh configurations
cp ~/.zshrc ~/Backup/zshrc_backup

# Backup Terminal profiles
defaults export com.apple.Terminal ~/Backup/terminal_backup.plist

# Backup VS Code settings
cp -r "$HOME/Library/Application Support/Code/User" ~/Backup/VSCodeBackup

echo "System preferences backup completed!"
```

### **Key Features**
- Backs up critical configuration files like `.zshrc` and Terminal profiles.
- Includes VS Code settings for developers.

---

## **AppleScript for Offsite Backup to External Drive**

This script performs an offsite backup of your home folder to an external SSD.

```applescript
set backupDisk to "/Volumes/ExternalSSD"
set homeFolder to "/Users/YOUR_USERNAME"

do shell script "rsync -a --progress " & quoted form of homeFolder & " " & quoted form of backupDisk
display dialog "Backup to external SSD completed!"
```

### **Key Features**
- Uses `rsync` with the `--progress` flag to show progress during the backup.
- Displays a confirmation dialog when the backup is complete.

---

## **Scheduling Backups Using `launchd`**

### **Step 1: Create a Plist File**
Open Terminal and create a new plist file:
```bash
nano ~/Library/LaunchAgents/com.user.backup.plist
```

### **Step 2: Add the XML Configuration**
Add the following XML configuration to the file:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>Label</key>
        <string>com.user.backup</string>
        <key>ProgramArguments</key>
        <array>
            <string>/Users/YOUR_USERNAME/backup.sh</string>
        </array>
        <key>StartInterval</key>
        <integer>86400</integer> <!-- Runs every 24 hours -->
    </dict>
</plist>
```

### **Step 3: Load the Plist File**
Save the file and load it into `launchd`:
```bash
launchctl load ~/Library/LaunchAgents/com.user.backup.plist
```
