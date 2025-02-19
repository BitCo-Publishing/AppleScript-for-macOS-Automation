
# Automating Tasks on macOS: AppleScript, Shell Scripts, `launchd`, and `cron`

This guide provides examples of automating tasks using AppleScript, shell scripts, `launchd`, and `cron`. It includes workflows for file management, monitoring, email processing, and scheduling.

---

## **1. Automating File Cleanup with AppleScript and `launchd`**

### **Step 1: Create the AppleScript**
Move files older than 7 days from the Desktop to a Backup folder:
```applescript
set sourceFolder to POSIX file "/Users/YourUsername/Desktop"
set destinationFolder to POSIX file "/Users/YourUsername/Desktop/Backup"

tell application "Finder"
    set oldFiles to (every file of folder sourceFolder whose modification date < ((current date) - 7 * days))
    repeat with f in oldFiles
        move f to folder destinationFolder
    end repeat
end tell
```

### **Step 2: Create the `launchd` Property List (.plist)**
Schedule the script to run daily at 11 PM:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.yourname.cleanup</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/osascript</string>
        <string>/Users/YourUsername/Desktop/cleanup.scpt</string>
    </array>
    <key>StartCalendarInterval</key>
    <dict>
        <key>Hour</key>
        <integer>23</integer>
        <key>Minute</key>
        <integer>0</integer>
    </dict>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```

### **Step 3: Load and Manage `launchd` Jobs**
1. Move the `.plist` file to `~/Library/LaunchAgents/`:
   ```bash
   mv com.yourname.cleanup.plist ~/Library/LaunchAgents/
   ```
2. Load the job into `launchd`:
   ```bash
   launchctl load ~/Library/LaunchAgents/com.yourname.cleanup.plist
   ```
3. Check if the job is running:
   ```bash
   launchctl list | grep com.yourname.cleanup
   ```
4. Unload the job (if needed):
   ```bash
   launchctl unload ~/Library/LaunchAgents/com.yourname.cleanup.plist
   ```

---

## **2. Automating Log Cleanup with `cron`**

### **Step 1: Create the Shell Script**
Delete log files older than 7 days:
```zsh
#!/bin/zsh
find /var/log -name "*.log" -type f -mtime +7 -exec rm {} \;
echo "Logs cleaned up on $(date)" >> /Users/YourUsername/log_cleanup.log
```

### **Step 2: Schedule with `cron`**
1. Open the crontab editor:
   ```bash
   crontab -e
   ```
2. Add this line to schedule the script every Monday at 2 AM:
   ```bash
   0 2 * * 1 /Users/YourUsername/cleanup_logs.sh
   ```
3. Verify scheduled jobs:
   ```bash
   crontab -l
   ```

---

## **3. Backing Up Screenshots with `launchd`**

### **Step 1: Create the AppleScript**
Move screenshot files from the Desktop to a backup folder:
```applescript
set sourceFolder to POSIX file "/Users/YourUsername/Desktop"
set destinationFolder to POSIX file "/Users/YourUsername/ScreenshotsBackup"

tell application "Finder"
    set screenshotFiles to (every file of folder sourceFolder whose name starts with "Screenshot")
    repeat with f in screenshotFiles
        move f to folder destinationFolder
    end repeat
end tell
```

### **Step 2: Create the `launchd` Property List (.plist)**
Schedule the script to run daily at midnight:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.yourname.screenshotbackup</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/osascript</string>
        <string>/Users/YourUsername/screenshot_backup.scpt</string>
    </array>
    <key>StartCalendarInterval</key>
    <dict>
        <key>Hour</key>
        <integer>0</integer>
        <key>Minute</key>
        <integer>0</integer>
    </dict>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```

### **Step 3: Load and Manage `launchd` Jobs**
1. Move the `.plist` file to `~/Library/LaunchAgents/`:
   ```bash
   mv com.yourname.screenshotbackup.plist ~/Library/LaunchAgents/
   ```
2. Load the job into `launchd`:
   ```bash
   launchctl load ~/Library/LaunchAgents/com.yourname.screenshotbackup.plist
   ```
3. Check if the job is running:
   ```bash
   launchctl list | grep com.yourname.screenshotbackup
   ```

---

## **4. Monitoring USB Drives with a Continuous Loop**

### **Step 1: Create the Monitoring Script**
Back up files when a USB drive is detected:
```zsh
#!/bin/zsh
backup_folder="$HOME/USB_Backups"
mkdir -p "$backup_folder"

while true; do
    if diskutil list | grep -q "USB_DRIVE_NAME"; then
        echo "USB detected! Backing up files..."
        cp -R /Volumes/USB_DRIVE_NAME/Documents "$backup_folder"
        echo "Backup complete!"
        break
    fi
    sleep 10
done
```

### **Step 2: Run the Script in the Background**
```bash
nohup ./usb_backup.sh &
```

---

## **5. Sending Periodic Reminders with AppleScript**

Display a reminder notification every 5 minutes:
```applescript
repeat
    display notification "Time to take a break!" with title "Reminder"
    delay 300 -- Wait for 5 minutes
end repeat
```

Run the script:
```bash
osascript reminder.scpt
```

---

## **6. Extracting Email Attachments and Processing Reports**

### **Step 1: Extract Email Attachments with AppleScript**
Save attachments from emails with "Daily Report" in the subject:
```applescript
tell application "Mail"
    set inboxMessages to messages of inbox
    repeat with aMessage in inboxMessages
        if subject of aMessage contains "Daily Report" then
            set emailAttachments to mail attachments of aMessage
            repeat with anAttachment in emailAttachments
                set filePath to (POSIX path of (path to desktop)) & name of anAttachment
                save anAttachment in filePath
            end repeat
        end if
    end repeat
end tell
```

### **Step 2: Rename the File with Shell Script**
Rename the most recent file on the Desktop:
```zsh
#!/bin/zsh
cd ~/Desktop
latest_file=$(ls -t | head -1)  # Get the most recent file
new_name="report_$(date +%Y-%m-%d).pdf"
mv "$latest_file" ~/Documents/Reports/"$new_name"
echo "Renamed $latest_file to $new_name"
```

### **Step 3: Extract Data from the Report with Python**
Extract text from the PDF report:
```python
import PyPDF2

pdf_path = "/Users/YourUsername/Documents/Reports/report_$(date +%Y-%m-%d).pdf"
with open(pdf_path, "rb") as file:
    reader = PyPDF2.PdfReader(file)
    text = ""
    for page in reader.pages:
        text += page.extract_text() + "\n"

# Save extracted data
with open("/Users/YourUsername/Documents/Reports/extracted_data.txt", "w") as output:
    output.write(text)

print("Data extraction completed.")
```

### **Step 4: Automate the Workflow with `cron`**
Combine all steps into a single workflow script:
```zsh
#!/bin/zsh
osascript extract_attachment.scpt   # Run AppleScript to get email attachments
./rename_file.sh                    # Rename the file
python3 extract_data.py             # Extract report data
```

Schedule the workflow to run every morning at 8 AM:
```bash
crontab -e
```
Add this line:
```bash
0 8 * * * /Users/YourUsername/workflow.sh
```


# Automating System Maintenance on macOS

This guide provides a step-by-step approach to automating system maintenance tasks such as clearing cache files, deleting old logs, compressing logs, and emptying the Trash. The workflow is implemented using shell scripts and scheduled with `launchd`.

---

## **Step 1: Deleting Cache Files**

### **Shell Script: `clear_cache.sh`**
Delete cache files older than 7 days:
```zsh
#!/bin/zsh
find ~/Library/Caches -type f -mtime +7 -delete
echo "Cache files older than 7 days deleted."
```

---

## **Step 2: Deleting Old Log Files**

### **Shell Script: `clear_logs.sh`**
Delete log files older than 7 days:
```zsh
#!/bin/zsh
find /var/log -type f -mtime +7 -delete
echo "Old logs removed."
```

---

## **Step 3: Compressing Remaining Logs**

### **Shell Script: `compress_logs.sh`**
Compress remaining log files into a `.tar.gz` archive:
```zsh
#!/bin/zsh
tar -czf ~/logs_backup_$(date +%Y-%m-%d).tar.gz /var/log
echo "Logs compressed."
```

---

## **Step 4: Combining into One Workflow**

### **Shell Script: `maintenance.sh`**
Combine all tasks into a single script:
```zsh
#!/bin/zsh
./clear_cache.sh
./clear_logs.sh
./compress_logs.sh
```

---

## **Enhanced System Maintenance Script**

### **Shell Script: `system_maintenance.sh`**
A comprehensive script that performs multiple maintenance tasks:
```zsh
#!/bin/zsh
echo "Starting Daily System Maintenance..."

# 1. Clear cache files older than 7 days
echo "Cleaning cache files..."
find ~/Library/Caches -type f -mtime +7 -delete

# 2. Remove old log files
echo "Deleting old log files..."
find /var/log -type f -mtime +7 -delete
find ~/Library/Logs -type f -mtime +7 -delete

# 3. Empty the Trash
echo "Emptying Trash..."
rm -rf ~/.Trash/*

# 4. Verify disk space
echo "Checking available disk space..."
df -h

echo "System Maintenance Completed!"
```

---

## **Adding Notifications**

### **AppleScript: `notify.scpt`**
Display a notification when maintenance is complete:
```applescript
display notification "System maintenance completed successfully!" with title "Maintenance Done"
```

### **Updated `system_maintenance.sh`**
Integrate the notification into the script:
```zsh
#!/bin/zsh
echo "Starting Daily System Maintenance..."

# 1. Clear cache files older than 7 days
echo "Cleaning cache files..."
find ~/Library/Caches -type f -mtime +7 -delete

# 2. Remove old log files
echo "Deleting old log files..."
find /var/log -type f -mtime +7 -delete
find ~/Library/Logs -type f -mtime +7 -delete

# 3. Empty the Trash
echo "Emptying Trash..."
rm -rf ~/.Trash/*

# 4. Notify the user
osascript -e 'display notification "System maintenance completed!" with title "Maintenance Done"'

echo "Maintenance Completed!"
```

---

## **Scheduling with `launchd`**

### **Step 1: Create a Launch Agent**
Create a `.plist` file for scheduling:
```bash
nano ~/Library/LaunchAgents/com.user.systemmaintenance.plist
```

Add the following configuration:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>Label</key>
        <string>com.user.systemmaintenance</string>
        <key>ProgramArguments</key>
        <array>
            <string>/Users/YourUsername/system_maintenance.sh</string>
        </array>
        <key>StartInterval</key>
        <integer>86400</integer> <!-- Runs every 24 hours -->
        <key>RunAtLoad</key>
        <true/>
    </dict>
</plist>
```

### **Step 2: Load the Agent**
Load the agent into `launchd`:
```bash
launchctl load ~/Library/LaunchAgents/com.user.systemmaintenance.plist
```

---

## **Testing the Automation**

### **Manually Run the Script**
Test the script by running it manually:
```bash
sh system_maintenance.sh
```

### **Check `launchd` Status**
Verify that the job is running:
```bash
launchctl list | grep systemmaintenance
```


