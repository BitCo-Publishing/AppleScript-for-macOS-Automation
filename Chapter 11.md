
# Automating Tasks with AppleScript and Terminal

This guide provides examples of automating various tasks on macOS using AppleScript in combination with Terminal commands. These scripts cover batch image processing, CSV data processing, API calls, system cleanup, email automation, screenshot management, document conversion, and web scraping.

---

## **1. Batch Image Processing**

### **AppleScript + Terminal Script**
Resize all `.jpg` images in a folder:
```applescript
set inputFolder to choose folder with prompt "Select the folder containing images:"
set outputSize to text returned of (display dialog "Enter the desired width (e.g., 800):" default answer "800")
set inputFolderUnix to POSIX path of inputFolder

do shell script "mkdir -p " & quoted form of inputFolderUnix & "/resized"
do shell script "for img in " & quoted form of inputFolderUnix & "/*.jpg; do sips -Z " & outputSize & " \"$img\" --out " & quoted form of inputFolderUnix & "/resized/$(basename \"$img\"); done"

display notification "Image resizing complete!" with title "Success"
```

---

## **2. CSV Data Processing**

### **AppleScript + Python**
Process a CSV file and display summary statistics:
```applescript
set csvFile to choose file with prompt "Select the CSV file:"
set csvPath to POSIX path of csvFile
set resultData to do shell script "python3 ~/process_csv.py " & quoted form of csvPath

display dialog "Processed Data:\n" & resultData with title "CSV Summary"
```

### **Python Script (`process_csv.py`)**
```python
import sys
import pandas as pd

file_path = sys.argv[1]
df = pd.read_csv(file_path)
summary = df.describe().to_string()
print(summary)
```

---

## **3. Fetching Weather Data via API**

### **AppleScript + Terminal**
Fetch the current temperature for a given city:
```applescript
set city to text returned of (display dialog "Enter your city:" default answer "New York")
set weatherData to do shell script "curl -s 'http://api.weatherapi.com/v1/current.json?key=YOUR_API_KEY&q=" & city & "' | jq -r '.current.temp_c'"

display notification "Current temperature in " & city & ": " & weatherData & "°C" with title "Weather Update"
```

---

## **4. System Cleanup**

### **AppleScript + Terminal**
Perform a system cleanup by removing cache files:
```applescript
set confirm to button returned of (display dialog "Do you want to run system cleanup?" buttons {"Cancel", "Proceed"} default button "Proceed")

if confirm = "Proceed" then
    do shell script "rm -rf ~/Library/Caches/*; sudo rm -rf /System/Library/Caches/*; sudo purge" with administrator privileges
    display notification "System cleanup completed!" with title "Maintenance Done"
end if
```

---

## **5. Email Attachment Automation**

### **Full Script**
Save and rename email attachments from Apple Mail:
```applescript
tell application "Mail"
    set selectedMessages to selection
    if selectedMessages is {} then
        display dialog "No email selected. Please select an email with an attachment."
        return
    end if
    
    set savePath to POSIX path of (choose folder with prompt "Select a folder to save attachments:")
    
    repeat with msg in selectedMessages
        set attCount to count of mail attachments of msg
        if attCount > 0 then
            repeat with att in mail attachments of msg
                set attName to name of att
                set filePath to savePath & attName
                
                -- Save the attachment to the specified folder
                save att in filePath
                
                -- Rename and organize using Terminal command
                do shell script "mv " & quoted form of filePath & " " & quoted form of savePath & "/processed_" & attName
            end repeat
        end if
    end repeat
end tell

display notification "Attachments processed successfully!" with title "Automation Done"
```

---

## **6. Screenshot Management**

### **Full Script**
Rename screenshots in bulk:
```applescript
set folderPath to POSIX path of (choose folder with prompt "Select your Screenshot Folder")

do shell script "
cd " & quoted form of folderPath & ";
count=1;
for file in Screen\\ Shot*; do
    mv \"$file\" \"Project_X_$(printf \"%02d\" $count).png\";
    ((count++));
done"

display notification "Screenshots renamed successfully!" with title "Batch Rename Done"
```

---

## **7. Converting Documents to PDFs**

### **Full Script**
Convert `.docx` files to PDFs using `libreoffice`:
```applescript
set fileList to choose file with prompt "Select documents to convert" with multiple selections allowed
set saveFolder to POSIX path of (choose folder with prompt "Select folder to save PDFs")

repeat with docFile in fileList
    set docPath to POSIX path of docFile
    set pdfName to (do shell script "basename " & quoted form of docPath & " .docx") & ".pdf"
    set outputPath to saveFolder & "/" & pdfName
    
    -- Use Terminal command to convert DOCX to PDF
    do shell script "libreoffice --headless --convert-to pdf " & quoted form of docPath & " --outdir " & quoted form of saveFolder
end repeat

display notification "Documents converted successfully!" with title "PDF Conversion Done"
```

---

## **8. Scraping Web Data**

### **Full Script**
Scrape stock price data from Yahoo Finance:
```applescript
set stockSymbol to text returned of (display dialog "Enter stock symbol (e.g., AAPL):" default answer "AAPL")
set stockPrice to do shell script "curl -s 'https://finance.yahoo.com/quote/" & stockSymbol & "' | grep 'data-field=\"regularMarketPrice\"' | awk -F'>' '{print $2}' | awk -F'<' '{print $1}'"

display dialog "Current price of " & stockSymbol & ": $" & stockPrice
```

# Automating Social Media and Web Tasks with AppleScript

This guide demonstrates how to automate tasks such as fetching weather data, logging into websites, posting on social media platforms (Twitter, Facebook, LinkedIn), and fetching engagement metrics using AppleScript and Terminal commands. It also includes instructions for scheduling these tasks with `launchd`.

---

## **1. Fetching and Displaying Weather Data**

### **AppleScript**
Fetch the current temperature for a city using the OpenWeatherMap API:
```applescript
set apiKey to "your_api_key_here"
set cityName to "New York"
set apiUrl to "https://api.openweathermap.org/data/2.5/weather?q=" & cityName & "&appid=" & apiKey & "&units=metric"

-- Call API using Terminal's curl
set weatherData to do shell script "curl -s " & quoted form of apiUrl

-- Extract temperature from JSON response using jq
set temperature to do shell script "echo " & quoted form of weatherData & " | jq '.main.temp'"

-- Display the result in macOS notification
display notification "The current temperature in " & cityName & " is " & temperature & "°C" with title "Weather Update"
```

---

## **2. Automatically Logging Into a Website**

### **AppleScript**
Log into a website using `curl` and extract an authentication token:
```applescript
set loginUrl to "https://example.com/login"
set username to "your_username"
set password to "your_password"

-- Perform login via curl
set loginResponse to do shell script "curl -s -c cookies.txt -d 'username=" & username & "&password=" & password & "' " & quoted form of loginUrl

-- Extract authentication token if required
set authToken to do shell script "cat cookies.txt | grep 'auth_token' | awk '{print $7}'"

display notification "Login successful! Token: " & authToken with title "Automation Complete"
```

---

## **3. Fetching and Processing Stock Market Data**

### **AppleScript**
Fetch the stock price for a given symbol using Yahoo Finance API:
```applescript
set stockSymbol to text returned of (display dialog "Enter stock symbol (e.g., AAPL):" default answer "AAPL")
set stockPrice to do shell script "curl -s 'https://query1.finance.yahoo.com/v7/finance/quote?symbols=" & stockSymbol & "' | jq '.quoteResponse.result[0].regularMarketPrice'"

display notification "Stock Price of " & stockSymbol & " is $" & stockPrice with title "Market Update"
```

---

## **4. Automating Twitter Posts**

### **AppleScript**
Post a tweet using the Twitter API:
```applescript
set tweetText to "Automating Twitter posts with AppleScript and Terminal! #Automation"
set bearerToken to "your_bearer_token"
set apiUrl to "https://api.twitter.com/2/tweets"

-- Construct JSON payload for the tweet
set tweetData to "{ \"text\": \"" & tweetText & "\" }"

-- Send API request via Terminal
do shell script "curl -X POST " & quoted form of apiUrl & " -H 'Authorization: Bearer " & bearerToken & "' -H 'Content-Type: application/json' -d " & quoted form of tweetData

display notification "Tweet posted successfully!" with title "Twitter Automation"
```

---

## **5. Automating Facebook Posts**

### **AppleScript**
Post on Facebook using the Graph API:
```applescript
set accessToken to "your_facebook_access_token"
set pageId to "your_page_id"
set message to "Automating Facebook posts with AppleScript!  #Automation"
set apiUrl to "https://graph.facebook.com/v12.0/" & pageId & "/feed?message=" & message & "&access_token=" & accessToken

-- Send API request via Terminal
do shell script "curl -X POST " & quoted form of apiUrl

display notification "Facebook post published successfully!" with title "Facebook Automation"
```

---

## **6. Automating LinkedIn Posts**

### **AppleScript**
Post on LinkedIn using the LinkedIn API:
```applescript
set accessToken to "your_linkedin_access_token"
set userId to "your_linkedin_user_id"
set postText to "Automating LinkedIn updates using AppleScript and Terminal! #Tech #Automation"
set apiUrl to "https://api.linkedin.com/v2/shares"

-- Construct JSON payload for the post
set postData to "{ \"owner\": \"urn:li:person:" & userId & "\", \"text\": { \"text\": \"" & postText & "\" } }"

-- Send API request via Terminal
do shell script "curl -X POST " & quoted form of apiUrl & " -H 'Authorization: Bearer " & accessToken & "' -H 'Content-Type: application/json' -d " & quoted form of postData

display notification "LinkedIn post published successfully!" with title "LinkedIn Automation"
```

---

## **7. Fetching Twitter Engagement Metrics**

### **AppleScript**
Fetch engagement metrics for a specific tweet:
```applescript
set bearerToken to "your_bearer_token"
set tweetId to "your_tweet_id"
set apiUrl to "https://api.twitter.com/2/tweets/" & tweetId & "/metrics"

-- Fetch engagement metrics
set responseData to do shell script "curl -X GET " & quoted form of apiUrl & " -H 'Authorization: Bearer " & bearerToken & "'"

display dialog "Twitter Engagement Data: " & responseData
```

---

## **8. Scheduling Twitter Posts with `launchd`**

### **Step 1: Create a Launch Agent File**
Create a `.plist` file to schedule daily Twitter posts:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.yourname.twitterpost</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/osascript</string>
        <string>/Users/yourname/Desktop/twitter_post.scpt</string>
    </array>
    <key>StartInterval</key>
    <integer>86400</integer> <!-- Runs every 24 hours -->
</dict>
</plist>
```

### **Step 2: Load the Launch Agent**
Load the agent into `launchd`:
```bash
launchctl load ~/Library/LaunchAgents/com.yourname.twitterpost.plist
```


# Automating macOS Tasks with AppleScript and GUI Scripting

This guide provides examples of automating tasks on macOS using AppleScript, including opening and arranging apps, interacting with web pages, processing emails, combining Finder and Terminal commands, and performing GUI scripting.

---

## **1. Opening and Arranging Apps**

### **AppleScript**
Automatically open and position Safari, Mail, and Notes windows:
```applescript
tell application "Safari"
    activate
    set the bounds of the front window to {0, 0, 900, 1000} -- Position window on left
end tell

tell application "Mail"
    activate
    set the bounds of the front window to {910, 0, 1600, 1000} -- Position window on right
end tell

tell application "Notes"
    activate
    set the bounds of the front window to {700, 500, 1400, 900} -- Position in center
end tell
```

---

## **2. Extracting Data from a Web Page**

### **AppleScript**
Extract stock price data from Safari and save it to Numbers:
```applescript
tell application "Safari"
    activate
    set stockData to do JavaScript "document.querySelector('.stock-price').innerText" in document 1
end tell

tell application "Numbers"
    activate
    tell front document to tell active sheet
        set value of cell "A1" to stockData
    end tell
end tell
```

---

## **3. Email Automation: Saving Attachments**

### **AppleScript**
Save email attachments from Apple Mail to a specific folder:
```applescript
tell application "Mail"
    set targetFolder to "Macintosh HD:Users:YourUser:Documents:Invoices"
    set theMessages to messages of inbox whose sender contains "invoices@example.com"
    
    repeat with eachMessage in theMessages
        set theAttachments to mail attachments of eachMessage
        repeat with eachAttachment in theAttachments
            set filePath to targetFolder & ":" & name of eachAttachment
            save eachAttachment in filePath
        end repeat
    end repeat
end tell

display notification "All invoices saved successfully!" with title "Mail Automation"
```

---

## **4. Combining Finder and Terminal for Automation**

### **AppleScript**
Take a screenshot, upload it to Slack, and notify the user:
```applescript
do shell script "screencapture -x ~/Desktop/screenshot.png"

set slackWebhook to "https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK"
set message to "{\"text\": \"Screenshot captured and uploaded!\"}"

do shell script "curl -X POST -H 'Content-type: application/json' --data " & quoted form of message & " " & slackWebhook

display notification "Screenshot sent to Slack!" with title "Slack Automation"
```

---

## **5. Merging PDF Files**

### **AppleScript**
Merge selected PDF files into a single document:
```applescript
tell application "Finder"
    set pdfFiles to selection
end tell

set pdfPaths to ""
repeat with pdfFile in pdfFiles
    set pdfPaths to pdfPaths & " " & quoted form of POSIX path of (pdfFile as alias)
end repeat

do shell script "pdfunite " & pdfPaths & " ~/Desktop/MergedDocument.pdf"

display notification "PDFs merged successfully!" with title "PDF Automation"
```

---

## **6. Basic GUI Scripting Structure**

### **AppleScript**
Interact with macOS applications using GUI scripting:
```applescript
tell application "System Events"
    tell process "Application Name"
        -- Perform UI actions here
    end tell
end tell
```

---

## **7. Finding Application UI Elements**

### **AppleScript**
Inspect UI elements of an application:
```applescript
tell application "System Events"
    tell process "TextEdit"
        get every UI element of window 1
    end tell
end tell
```

---

## **8. Example 1: Clicking a Button in System Preferences**

### **AppleScript**
Open System Settings and click a button:
```applescript
tell application "System Settings"
    activate
end tell

delay 1 -- Wait for the window to open

tell application "System Events"
    tell process "System Settings"
        click button "Night Shift" of window "Displays"
    end tell
end tell
```

---

## **9. Example 2: Selecting a Menu Item in TextEdit**

### **AppleScript**
Activate TextEdit and select a menu item:
```applescript
tell application "TextEdit"
    activate
end tell

tell application "System Events"
    tell process "TextEdit"
        click menu item "Save As…" of menu "File" of menu bar 1
    end tell
end tell
```

---

## **10. Example 3: Entering Text into a Safari Form**

### **AppleScript**
Enter text into a form field in Safari:
```applescript
tell application "Safari"
    activate
end tell

delay 1

tell application "System Events"
    tell process "Safari"
        set value of text field 1 of window 1 to "AppleScript Automation"
    end tell
end tell
```

---

## **11. Pressing a Keyboard Shortcut in Finder**

### **AppleScript**
Simulate a keyboard shortcut in Finder:
```applescript
tell application "Finder" to activate

tell application "System Events"
    keystroke "n" using {command down, shift down}
end tell
```

