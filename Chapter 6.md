
# File Organizer Script

## Define File Categories
```applescript
set fileCategories to {¬
    {"Documents", {"pdf", "doc", "docx", "txt"}}, ¬
    {"Pictures", {"jpg", "jpeg", "png", "gif"}}, ¬
    {"Archives", {"zip", "rar", "tar", "gz"}}, ¬
    {"Videos", {"mp4", "mov", "avi"}}, ¬
    {"Others", {}} ¬
}
```

## Extract File Extensions
```applescript
on getFileExtension(filePath)
    set AppleScript's text item delimiters to "."
    set fileParts to text items of filePath
    set AppleScript's text item delimiters to ""
    return last item of fileParts
end getFileExtension
```

### Example Usage:
```applescript
set myFile to "/Users/Max/Desktop/example.pdf"
set fileType to getFileExtension(myFile)
log fileType -- Outputs: "pdf"
```

## Find the Correct Destination Folder
```applescript
on getDestinationFolder(fileExtension, baseFolder)
    repeat with category in fileCategories
        set categoryName to item 1 of category
        set extensionsList to item 2 of category
        
        if fileExtension is in extensionsList then
            return baseFolder & "/" & categoryName
        end if
    end repeat
    return baseFolder & "/Others"
end getDestinationFolder
```

### Example Usage:
```applescript
set targetFolder to getDestinationFolder("pdf", "/Users/Max/Documents/File Organizer")
log targetFolder -- Outputs: "/Users/Max/Documents/File Organizer/Documents"
```

## Move Files to the Right Folder
```applescript
on moveFileToFolder(filePath, destinationFolder)
    tell application "Finder"
        -- Ensure the destination folder exists
        if not (exists folder destinationFolder) then
            make new folder at POSIX file destinationFolder
        end if
        -- Move the file
        move file filePath to folder destinationFolder
    end tell
end moveFileToFolder
```

### Example Usage:
```applescript
set fileToMove to "/Users/Max/Downloads/example.pdf"
set destination to "/Users/Max/Documents/File Organizer/Documents"
moveFileToFolder(fileToMove, destination)
```

## Scan the Downloads Folder and Sort Files
```applescript
set sourceFolder to "/Users/Max/Downloads"
set baseFolder to "/Users/Max/Documents/File Organizer"

tell application "Finder"
    set fileList to every file of folder sourceFolder
end tell

repeat with fileItem in fileList
    set filePath to POSIX path of (fileItem as alias)
    set fileExt to getFileExtension(filePath)
    set destinationFolder to getDestinationFolder(fileExt, baseFolder)
    moveFileToFolder(filePath, destinationFolder)
end repeat
```
```
```markdown
# File Organizer Script with Automation

This script organizes files from the `Downloads` folder into categorized folders based on their file extensions. It also includes instructions for automating the script using a Launch Agent.

---

## **Visualizing the Workflow**

```
START  
   ↓  
Scan source folder  
   ↓  
For each file → Determine extension  
   ↓  
Find the correct category  
   ↓  
Move the file to the correct destination  
   ↓  
Repeat until all files are processed  
   ↓  
END  
```

---

## **Defining File Categories**

```applescript
set fileCategories to {¬
    {"Documents", {"pdf", "doc", "docx", "txt"}}, ¬
    {"Pictures", {"jpg", "jpeg", "png", "gif"}}, ¬
    {"Archives", {"zip", "rar", "tar", "gz"}}, ¬
    {"Videos", {"mp4", "mov", "avi"}}, ¬
    {"Others", {}} ¬
}
```

---

## **Scanning the Source Folder**

```applescript
set sourceFolder to "/Users/Max/Downloads"
tell application "Finder"
    set fileList to every file of folder sourceFolder
end tell
```

---

## **Extracting File Extensions**

```applescript
on getFileExtension(filePath)
    set AppleScript's text item delimiters to "."
    set fileParts to text items of filePath
    set AppleScript's text item delimiters to ""
    return last item of fileParts
end getFileExtension
```

✅ **Example Usage:**
```applescript
set myFile to "/Users/Max/Desktop/example.pdf"
set fileType to getFileExtension(myFile)
log fileType -- Outputs: "pdf"
```

---

## **Determining the Destination Folder**

```applescript
on getDestinationFolder(fileExtension, baseFolder)
    repeat with category in fileCategories
        set categoryName to item 1 of category
        set extensionsList to item 2 of category
        
        if fileExtension is in extensionsList then
            return baseFolder & "/" & categoryName
        end if
    end repeat
    return baseFolder & "/Others"
end getDestinationFolder
```

✅ **Example Usage:**
```applescript
set targetFolder to getDestinationFolder("pdf", "/Users/Max/Documents/File Organizer")
log targetFolder -- Outputs: "/Users/Max/Documents/File Organizer/Documents"
```

---

## **Moving Files to the Correct Folder**

```applescript
on moveFileToFolder(filePath, destinationFolder)
    tell application "Finder"
        -- Ensure the destination folder exists
        if not (exists folder destinationFolder) then
            make new folder at POSIX file destinationFolder
        end if
        -- Move the file
        move file filePath to folder destinationFolder
    end tell
end moveFileToFolder
```

✅ **Example Usage:**
```applescript
set fileToMove to "/Users/Max/Downloads/example.pdf"
set destination to "/Users/Max/Documents/File Organizer/Documents"
moveFileToFolder(fileToMove, destination)
```

---

## **Handling Errors Gracefully**

```applescript
try
    moveFileToFolder(fileToMove, destination)
on error errMsg
    log "Error moving file: " & errMsg
end try
```

---

## **Putting Everything Together**

```applescript
set sourceFolder to "/Users/Max/Downloads"
set baseFolder to "/Users/Max/Documents/File Organizer"

tell application "Finder"
    set fileList to every file of folder sourceFolder
end tell

repeat with fileItem in fileList
    set filePath to POSIX path of (fileItem as alias)
    set fileExt to getFileExtension(filePath)
    set destinationFolder to getDestinationFolder(fileExt, baseFolder)
    
    try
        moveFileToFolder(filePath, destinationFolder)
    on error errMsg
        log "Error processing file " & filePath & ": " & errMsg
    end try
end repeat
```

---

## **Automating the Script Execution**

### **Step 1: Create the Launch Agent Directory**
Open Terminal and type:
```bash
mkdir -p ~/Library/LaunchAgents
```

### **Step 2: Create the Plist Configuration File**
```bash
nano ~/Library/LaunchAgents/com.fileorganizer.plist
```

### **Step 3: Add the Following Configuration**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.fileorganizer</string>
    <key>ProgramArguments</key>
    <array>
        <string>osascript</string>
        <string>/Users/Max/Documents/fileOrganizer.scpt</string>
    </array>
    <key>StartInterval</key>
    <integer>600</integer> <!-- Runs every 600 seconds (10 minutes) -->
</dict>
</plist>
```

### **Step 4: Load the Launch Agent**
```bash
launchctl load ~/Library/LaunchAgents/com.fileorganizer.plist
```

---


# File Organizer Script Using AppleScript & Shell

This script organizes files from the `Downloads` folder into categorized folders based on their file extensions. It uses a combination of AppleScript and shell commands to ensure efficient file handling. Additionally, the script can be automated using a Launch Agent.

---

## **Defining File Categories**

```applescript
set fileCategories to {¬
    {"Documents", {"pdf", "docx", "txt", "csv", "xls"}}, ¬
    {"Images", {"jpg", "jpeg", "png", "gif"}}, ¬
    {"Videos", {"mp4", "mov", "avi"}}, ¬
    {"Music", {"mp3", "wav", "flac"}}, ¬
    {"Archives", {"zip", "rar", "tar", "gz"}}, ¬
    {"Executables", {"dmg", "pkg", "app"}}, ¬
    {"Others", {}} ¬
}
```

---

## **Getting the File Extension**

```applescript
on getFileExtension(filePath)
    set AppleScript's text item delimiters to "."
    set fileParts to text items of filePath
    set AppleScript's text item delimiters to ""
    return last item of fileParts
end getFileExtension
```

✅ **Example Usage:**
```applescript
set myFile to "/Users/Max/Desktop/example.pdf"
set fileType to getFileExtension(myFile)
log fileType -- Outputs: "pdf"
```

---

## **Determining the Destination Folder**

```applescript
on getDestinationFolder(fileExtension, baseFolder)
    repeat with category in fileCategories
        set categoryName to item 1 of category
        set extensionsList to item 2 of category
        
        if fileExtension is in extensionsList then
            return baseFolder & "/" & categoryName
        end if
    end repeat
    return baseFolder & "/Others"
end getDestinationFolder
```

✅ **Example Usage:**
```applescript
set targetFolder to getDestinationFolder("pdf", "/Users/Max/Documents/File Organizer")
log targetFolder -- Outputs: "/Users/Max/Documents/File Organizer/Documents"
```

---

## **Moving Files Using Shell Commands**

```applescript
on moveFileToFolder(filePath, destinationFolder)
    -- Ensure the folder exists
    do shell script "mkdir -p " & quoted form of destinationFolder
    
    -- Move the file
    do shell script "mv " & quoted form of filePath & " " & quoted form of destinationFolder
end moveFileToFolder
```

✅ **Example Usage:**
```applescript
set fileToMove to "/Users/Max/Downloads/example.pdf"
set destination to "/Users/Max/Documents/File Organizer/Documents"
moveFileToFolder(fileToMove, destination)
```

---

## **Full Script: File Organizer**

```applescript
set sourceFolder to "/Users/Max/Downloads"
set baseFolder to "/Users/Max/Documents/File Organizer"

tell application "Finder"
    set fileList to every file of folder sourceFolder
end tell

repeat with fileItem in fileList
    set filePath to POSIX path of (fileItem as alias)
    set fileExt to getFileExtension(filePath)
    set destinationFolder to getDestinationFolder(fileExt, baseFolder)
    
    try
        moveFileToFolder(filePath, destinationFolder)
    on error errMsg
        log "Error processing file " & filePath & ": " & errMsg
    end try
end repeat
```

---

## **Automating the Script Execution**

### **Step 1: Create the Launch Agent Directory**
Open Terminal and type:
```bash
mkdir -p ~/Library/LaunchAgents
```

### **Step 2: Create the Plist Configuration File**
```bash
nano ~/Library/LaunchAgents/com.fileorganizer.plist
```

### **Step 3: Add the Following XML**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.fileorganizer</string>
    <key>ProgramArguments</key>
    <array>
        <string>osascript</string>
        <string>/Users/Max/Documents/fileOrganizer.scpt</string>
    </array>
    <key>StartInterval</key>
    <integer>600</integer> <!-- Runs every 10 minutes -->
</dict>
</plist>
```

### **Step 4: Load the Launch Agent**
```bash
launchctl load ~/Library/LaunchAgents/com.fileorganizer.plist
```

---

# File Organizer Script: Testing, Automation, and Debugging

This guide walks you through testing the file organizer script, setting up automation using a Launch Agent, adding more categories, and logging actions for debugging purposes.

---

## **Creating a Test Folder Structure**

### **Step 1: Create a Test Folder**
Create a test folder called `TestDownloads` in your home directory:
```bash
mkdir -p ~/TestDownloads
```

### **Step 2: Generate Sample Files**
Generate sample files of various types in the `TestDownloads` folder:
```bash
touch ~/TestDownloads/document1.pdf
touch ~/TestDownloads/spreadsheet.xls
touch ~/TestDownloads/image1.jpg
touch ~/TestDownloads/video.mp4
touch ~/TestDownloads/archive.zip
touch ~/TestDownloads/script.sh
touch ~/TestDownloads/randomfile.xyz
```

### **Step 3: Set Up the Destination Folder**
Set up the destination folder for organized files:
```bash
mkdir -p ~/TestOrganized
```

---

## **Modifying the Script for Testing**

Update the script to use the test folders:
```applescript
set sourceFolder to "/Users/Max/TestDownloads"
set baseFolder to "/Users/Max/TestOrganized"
```

Run the script using `osascript`:
```bash
osascript ~/Documents/FileOrganizer.scpt
```

---

## **Verifying the Output**

Check the contents of the `TestOrganized` folder:
```bash
ls ~/TestOrganized
```

**Expected Structure:**
```
Documents/
    document1.pdf
    spreadsheet.xls
Images/
    image1.jpg
Videos/
    video.mp4
Archives/
    archive.zip
Scripts/
    script.sh
Others/
    randomfile.xyz
```

---

## **Steps to Set Up the Automation**

### **Step 1: Create a Launch Agent File**
Create a Launch Agent configuration file:
```bash
nano ~/Library/LaunchAgents/com.fileorganizer.plist
```

### **Step 2: Add the XML Configuration**
Add the following XML configuration to the file:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.fileorganizer</string>
    <key>ProgramArguments</key>
    <array>
        <string>osascript</string>
        <string>/Users/Max/Documents/FileOrganizer.scpt</string>
    </array>
    <key>StartInterval</key>
    <integer>600</integer> <!-- Runs every 10 minutes -->
</dict>
</plist>
```

### **Step 3: Load the Launch Agent**
Load the Launch Agent to activate automation:
```bash
launchctl load ~/Library/LaunchAgents/com.fileorganizer.plist
```

---

## **Adding More Categories**

You can add more categories to the `fileCategories` list. For example, to include code files:
```applescript
set fileCategories to {¬
    {"Documents", {"pdf", "docx", "txt", "csv", "xls"}}, ¬
    {"Images", {"jpg", "jpeg", "png", "gif"}}, ¬
    {"Videos", {"mp4", "mov", "avi"}}, ¬
    {"Music", {"mp3", "wav", "flac"}}, ¬
    {"Archives", {"zip", "rar", "tar", "gz"}}, ¬
    {"Executables", {"dmg", "pkg", "app"}}, ¬
    {"Code Files", {"py", "java", "cpp", "js", "html", "css"}}, ¬
    {"Others", {}} ¬
}
```

---

## **Logging Actions for Debugging**

To log actions for debugging, define a `logAction` function in your script:
```applescript
on logAction(message)
    do shell script "echo " & quoted form of message & " >> ~/Documents/FileOrganizer.log"
end logAction
```

Use this function throughout your script to log important events. For example:
```applescript
logAction("Processing file: " & filePath)
logAction("Moving file to: " & destinationFolder)
```

---

## **Full Script with Logging**

Here’s how the full script might look with logging added:

```applescript
set sourceFolder to "/Users/Max/TestDownloads"
set baseFolder to "/Users/Max/TestOrganized"

-- Log action function
on logAction(message)
    do shell script "echo " & quoted form of message & " >> ~/Documents/FileOrganizer.log"
end logAction

tell application "Finder"
    set fileList to every file of folder sourceFolder
end tell

repeat with fileItem in fileList
    set filePath to POSIX path of (fileItem as alias)
    set fileExt to getFileExtension(filePath)
    set destinationFolder to getDestinationFolder(fileExt, baseFolder)
    
    logAction("Processing file: " & filePath)
    logAction("Destination folder: " & destinationFolder)
    
    try
        moveFileToFolder(filePath, destinationFolder)
        logAction("Successfully moved file: " & filePath)
    on error errMsg
        logAction("Error processing file " & filePath & ": " & errMsg)
    end try
end repeat
```

---
