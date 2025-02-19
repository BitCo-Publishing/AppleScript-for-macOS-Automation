
## Variables

```applescript
set myName to "Max"
set myAge to 30
set myList to {"Apple", "Banana", "Cherry"}
```

---

## Comments

```applescript
-- This is a single-line comment

(* 
This is a multi-line comment.
Used for longer explanations.
*)
```

---

## Checking a Number's Value

```applescript
set age to 25
if age > 18 then
    display dialog "You are an adult!"
else
    display dialog "You are a minor!"
end if
```

---

## Repeat a Task 5 Times

```applescript
repeat 5 times
    display dialog "This is a loop iteration!"
end repeat
```

---

## Loop Through a List

```applescript
set fruits to {"Apple", "Banana", "Cherry"}
repeat with item in fruits
    display dialog item
end repeat
```

---

## Creating a Function to Greet a User

```applescript
on greetUser(name)
    display dialog "Hello, " & name & "!"
end greetUser

-- Calling the function
greetUser("Max")
```

---

## Open Safari and Load a Webpage

```applescript
tell application "Safari"
    activate
    open location "https://www.apple.com"
end tell
```

---

## Ask for User Input

```applescript
set userResponse to text returned of (display dialog "Enter your name:" default answer "")
display dialog "Hello, " & userResponse & "!"
```

---

## A Simple AppleScript Example

```applescript
-- Ask for user's name
set userName to text returned of (display dialog "Enter your name:" default answer "")

-- Ask for user's age
set userAge to text returned of (display dialog "Enter your age:" default answer "")

-- Convert age to a number
set userAge to userAge as integer

-- Check age category
if userAge ≥ 18 then
    set message to "Hello, " & userName & "! You are an adult."
else
    set message to "Hello, " & userName & "! You are underage."
end if

-- Display the result
display dialog message
```

---

## Keep Your Code Readable

```applescript
set userName to "John"
set userAge to 25
display dialog userName & " is " & userAge & " years old."
```

---

## Well-Formatted Code (Easier to Read)

```applescript
tell application "Finder"
    activate
    set myFiles to every file of desktop
    
    repeat with f in myFiles
        display dialog name of f
    end repeat
end tell
```

---

## Better Example (Using a Handler)

```applescript
on greetUser(name)
    display dialog "Hello, " & name & "!"
end greetUser

greetUser("Max")
greetUser("Sara")
greetUser("John")
```

---

## Optimize Performance and Efficiency

```applescript
set numbers to {5, 10, 15}
set total to sum of numbers
set average to total / (count of numbers)

display dialog "Total: " & total
display dialog "Average: " & average
```

---

## Handling Errors Gracefully

```applescript
try
    tell application "Finder"
        open file "NonexistentFile.txt" of desktop
    end tell
on error errMsg
    display dialog "Error: " & errMsg
end try
```

---

## Breaking a Script into Functions

```applescript
on getUserName()
    return text returned of (display dialog "Enter your name:" default answer "")
end getUserName

on greetUser(name)
    display dialog "Hello, " & name & "!"
end greetUser

set userName to getUserName()
greetUser(userName)
```

---

## Variables

```applescript
set myName to "Max"
set myAge to 30
set myList to {"Apple", "Banana", "Cherry"}
```

---

## Comments

```applescript
-- This is a single-line comment

(* 
This is a multi-line comment.
Used for longer explanations.
*)
```

---

## Checking a Number's Value

```applescript
set age to 25
if age > 18 then
    display dialog "You are an adult!"
else
    display dialog "You are a minor!"
end if
```

---

## Repeat a Task 5 Times

```applescript
repeat 5 times
    display dialog "This is a loop iteration!"
end repeat
```

---

## Loop Through a List

```applescript
set fruits to {"Apple", "Banana", "Cherry"}
repeat with item in fruits
    display dialog item
end repeat
```

---

## Creating a Function to Greet a User

```applescript
on greetUser(name)
    display dialog "Hello, " & name & "!"
end greetUser

greetUser("Max")
```

---

## Open Safari and Load a Webpage

```applescript
tell application "Safari"
    activate
    open location "https://www.apple.com"
end tell
```

---

## Ask for User Input

```applescript
set userResponse to text returned of (display dialog "Enter your name:" default answer "")
display dialog "Hello, " & userResponse & "!"
```

---

## A Simple AppleScript Example

```applescript
set userName to text returned of (display dialog "Enter your name:" default answer "")
set userAge to text returned of (display dialog "Enter your age:" default answer "")
set userAge to userAge as integer

if userAge ≥ 18 then
    set message to "Hello, " & userName & "! You are an adult."
else
    set message to "Hello, " & userName & "! You are underage."
end if

display dialog message
```

---

## Keep Your Code Readable

```applescript
set userName to "John"
set userAge to 25
display dialog userName & " is " & userAge & " years old."
```

---

## Well-Formatted Code (Easier to Read)

```applescript
tell application "Finder"
    activate
    set myFiles to every file of desktop
    
    repeat with f in myFiles
        display dialog name of f
    end repeat
end tell
```

---

## Optimize Performance and Efficiency

```applescript
set numbers to {5, 10, 15}
set total to sum of numbers
set average to total / (count of numbers)

display dialog "Total: " & total
display dialog "Average: " & average
```

---

## Handling Errors Gracefully

```applescript
try
    tell application "Finder"
        open file "NonexistentFile.txt" of desktop
    end tell
on error errMsg
    display dialog "Error: " & errMsg
end try
```

---

## Breaking a Script into Functions

```applescript
on getUserName()
    return text returned of (display dialog "Enter your name:" default answer "")
end getUserName

on greetUser(name)
    display dialog "Hello, " & name & "!"
end greetUser

set userName to getUserName()
greetUser(userName)
```

---

## Parameters for Flexibility

```applescript
on calculateSum(a, b)
    return a + b
end calculateSum

set result to calculateSum(10, 20)
display dialog "The sum is: " & result
```

---

## Debugging with log

```applescript
set myNumber to 42
log "The value of myNumber is: " & myNumber  -- This will print in the Script Editor log window
```

---

## Running AppleScript via Terminal

```applescript
osascript -e 'display dialog "Hello from Terminal!"'
osascript -e 'tell application "Finder" to open home'
```

---

## A Simple Application Script

```applescript
display dialog "Click OK to launch Safari" buttons {"OK"}
tell application "Safari" to activate
```

---

## Using log to Debug

```applescript
set userName to "Max"
log "Debug: The username is " & userName
```

---

## Using try...on error to Handle Errors

```applescript
try
    tell application "Finder" to open file "MissingFile.txt" of desktop
on error errorMessage
    display dialog "Error: " & errorMessage
end try
```

---

## Code to Open and Read Text in TextEdit

```applescript
tell application "TextEdit"
    if not (exists document 1) then
        display dialog "No open document found!" buttons {"OK"} default button "OK"
        return
    end if
    set docText to text of document 1
end tell
```

---

## Code to Capitalize Each Word in a String

```applescript
on capitalizeWords(myText)
    set textItems to words of myText
    set newText to ""
    
    repeat with wordItem in textItems
        set newText to newText & (character 1 of wordItem as string) & (lowercase (text 2 thru -1 of wordItem)) & " "
    end repeat
    
    return trimWhitespace(newText)
end capitalizeWords

on trimWhitespace(inputText)
    return do shell script "echo " & quoted form of inputText & " | sed 's/[[:space:]]*$//'"
end trimWhitespace
```

---

## Code to Replace Text in TextEdit

```applescript
tell application "TextEdit"
    set text of document 1 to capitalizeWords(docText)
end tell
```
