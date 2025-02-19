
# AppleScript Basics: Variables, Loops, and Conditionals

---

## **Declaring and Assigning Variables**

```applescript
set userName to "Max"
set userAge to 25
set isAdmin to true
set priceList to {99, 149, 199}
```

---

## **Using Variables in Dialogs**

```applescript
set userName to text returned of (display dialog "Enter your name:" default answer "")
display dialog "Hello, " & userName & "! Welcome to AppleScript."
```

---

## **Loops**

### **Fixed Iterations with `repeat`**
```applescript
repeat 5 times
    display dialog "This is iteration number " & result
end repeat
```

### **Condition-Based Repetition with `repeat while`**
```applescript
set counter to 1
repeat while counter ≤ 5
    display dialog "Counter is at: " & counter
    set counter to counter + 1
end repeat
```

### **Looping Through a List of Items**
```applescript
set itemList to {"MacBook", "iPad", "iPhone"}
repeat with device in itemList
    display dialog "Processing: " & device
end repeat
```

---

## **Conditionals**

### **Basic `if-else` Statement**
```applescript
set userAge to 20
if userAge ≥ 18 then
    display dialog "You are an adult."
else
    display dialog "You are a minor."
end if
```

### **Nested `if-else` for Multiple Conditions**
```applescript
set userScore to 85
if userScore ≥ 90 then
    set grade to "A"
else if userScore ≥ 80 then
    set grade to "B"
else if userScore ≥ 70 then
    set grade to "C"
else
    set grade to "F"
end if
display dialog "Your grade is: " & grade
```

### **Using `if` Conditions with File Operations**
```applescript
set filePath to POSIX file "/Users/username/Desktop/sample.txt"
tell application "System Events"
    if exists file filePath then
        display dialog "File exists."
    else
        display dialog "File does not exist."
    end if
end tell
```

---

## **Exercises**

### **Exercise 1: Automating Folder Cleanup**
```applescript
set folderPath to POSIX file (choose folder with prompt "Select a folder to clean up")
tell application "System Events"
    set fileList to every file of folder folderPath
    repeat with currentFile in fileList
        set fileDate to modification date of currentFile
        if (current date) - fileDate > (30 * days) then
            delete currentFile
        end if
    end repeat
end tell
display dialog "Old files cleaned up!"
```

### **Exercise 2: Batch Renaming Files**
```applescript
set folderPath to POSIX file (choose folder with prompt "Select a folder")
tell application "System Events"
    set fileList to every file of folder folderPath whose name extension is "txt"
    repeat with currentFile in fileList
        set oldName to name of currentFile
        set newName to "Renamed_" & oldName
        set name of currentFile to newName
    end repeat
end tell
display dialog "All text files have been renamed!"
```
```


# Zsh Scripting: Functions, Arguments, and Real-World Examples

---

## **Defining and Calling a Function**

### **Basic Function Syntax**
```zsh
function greet_user {
    echo "Hello, $1! Welcome to zsh scripting."
}
```
This function takes one argument (`$1`) and prints a greeting.

**Calling the Function:**
```zsh
greet_user "Max"
```

**Output:**
```
Hello, Max! Welcome to zsh scripting.
```

### **Alternative Syntax (Without `function` Keyword)**
```zsh
greet_user() {
    echo "Hello, $1! Welcome to zsh scripting."
}
```

---

## **Functions with Multiple Parameters**

### **Example: Adding Two Numbers**
```zsh
add_numbers() {
    sum=$(( $1 + $2 ))
    echo "The sum of $1 and $2 is: $sum"
}
add_numbers 10 20
```

**Output:**
```
The sum of 10 and 20 is: 30
```

---

## **Returning Values from Functions**

### **Using `echo` (Preferred Method)**
```zsh
get_date() {
    echo "Today's date is: $(date)"
}
current_date=$(get_date)
echo "$current_date"
```

**Output:**
```
Today's date is: Tue Feb 5 10:30:00 PST 2025
```

### **Using `return` (For Exit Codes)**
```zsh
is_even() {
    if (( $1 % 2 == 0 )); then
        return 0  # Success
    else
        return 1  # Failure
    fi
}
is_even 10 && echo "Number is even" || echo "Number is odd"
```

**Output:**
```
Number is even
```

---

## **Handling Arguments in Zsh Scripts**

### **Passing Arguments to Scripts**
```zsh
#!/bin/zsh
echo "First argument: $1"
echo "Second argument: $2"
```

**Run the Script:**
```bash
zsh myscript.zsh Hello World
```

**Output:**
```
First argument: Hello
Second argument: World
```

### **Using `"$@"` to Handle Multiple Arguments**
```zsh
for arg in "$@"; do
    echo "Processing: $arg"
done
```

**Run with:**
```bash
zsh myscript.zsh file1.txt file2.txt file3.txt
```

**Output:**
```
Processing: file1.txt
Processing: file2.txt
Processing: file3.txt
```

### **Default Values for Arguments**
```zsh
greet() {
    name=${1:-"Guest"}  # Use "Guest" if no argument is given
    echo "Hello, $name!"
}
greet   # No argument
greet "Max"
```

**Output:**
```
Hello, Guest!
Hello, Max!
```

---

## **Checking Exit Codes**

### **Example: Checking Exit Code After a Command**
```zsh
ls /nonexistent_folder
echo "Exit code: $?"
```

**Output:**
```
ls: cannot access '/nonexistent_folder': No such file or directory
Exit code: 2
```

### **Using `if` Statements for Error Handling**
```zsh
mkdir test_directory
if [ $? -eq 0 ]; then
    echo "Directory created successfully!"
else
    echo "Failed to create directory!"
fi
```

### **Using `set -e` to Exit on Errors**
```zsh
#!/bin/zsh
set -e
mkdir test_dir
cd test_dir
ls /nonexistent_folder  # This will cause the script to exit
echo "This will never be printed."
```

---

## **Real-World Example: Automated Backup Script**

```zsh
#!/bin/zsh
backup_files() {
    local source_dir="$1"
    local backup_dir="$2"
    if [ ! -d "$source_dir" ]; then
        echo "Error: Source directory does not exist!" >&2
        return 1
    fi
    mkdir -p "$backup_dir"
    cp -r "$source_dir"/* "$backup_dir"
    
    echo "Backup completed successfully!"
    return 0
}
backup_files "/Users/Max/Documents" "/Users/Max/Backup"
```

**Run with:**
```bash
zsh backup_script.zsh "/Users/Max/Documents" "/Users/Max/Backup"
```

**Output:**
```
Backup completed successfully!
```

---

## **Sample Input File: `employees.csv`**

```
ID, Name, Age, Department, Salary
101, John Doe, 30, IT, 55000
102, Jane Smith, 28, HR, 52000
103, Alice Brown, 26, IT, 60000
104, Bob White, 35, Finance, 70000
105, John Doe, 30, IT, 55000  # Duplicate
   # Blank Line
106,  , 32, Sales, 48000      # Missing Name
```

---

## **Shell Script for Cleaning CSV Data**

```zsh
#!/bin/zsh
clean_csv() {
    input_file="$1"
    output_file="$2"
    # Step 1: Remove blank lines and comments
    grep -vE '^\s*$|^#' "$input_file" |
    # Step 2: Remove duplicate lines
    sort | uniq |
    # Step 3: Remove rows with missing fields
    awk -F, 'NF==5' > "$output_file"
    echo "✅ Data cleaned and saved to $output_file"
}
clean_csv "employees.csv" "cleaned_employees.csv"
```

# Shell Scripting for Data Processing: CSV, JSON, and Log Files

This guide provides shell scripts for cleaning, filtering, and analyzing data from various file formats like CSV, JSON, and log files. Each script is accompanied by explanations and sample outputs.

---

## **1. Cleaning CSV Data**

### **Shell Script**
```zsh
#!/bin/zsh
clean_csv() {
    input_file="$1"
    output_file="$2"
    # Step 1: Remove blank lines and comments
    grep -vE '^\s*$|^#' "$input_file" |
    # Step 2: Remove duplicate lines
    sort | uniq |
    # Step 3: Remove rows with missing fields
    awk -F, 'NF==5' > "$output_file"
    echo "✅ Data cleaned and saved to $output_file"
}
clean_csv "employees.csv" "cleaned_employees.csv"
```

### **Explanation**
1. **Remove Blank Lines and Comments:** `grep -vE '^\s*$|^#'` removes empty lines and lines starting with `#`.
2. **Remove Duplicates:** `sort | uniq` ensures only unique rows remain.
3. **Filter Rows with Missing Fields:** `awk -F, 'NF==5'` keeps only rows with exactly 5 fields.

### **Sample Input File: `employees.csv`**
```
ID, Name, Age, Department, Salary
101, John Doe, 30, IT, 55000
102, Jane Smith, 28, HR, 52000
103, Alice Brown, 26, IT, 60000
104, Bob White, 35, Finance, 70000
105, John Doe, 30, IT, 55000  # Duplicate
   # Blank Line
106,  , 32, Sales, 48000      # Missing Name
```

### **Output: `cleaned_employees.csv`**
```
ID, Name, Age, Department, Salary
101, John Doe, 30, IT, 55000
102, Jane Smith, 28, HR, 52000
103, Alice Brown, 26, IT, 60000
104, Bob White, 35, Finance, 70000
```

---

## **2. Filtering CSV Data**

### **Shell Script**
```zsh
#!/bin/zsh
filter_department() {
    input_file="$1"
    department="$2"
    output_file="$3"
    # Extract only lines where the 4th column matches the department
    awk -F, -v dept="$department" '$4 ~ dept' "$input_file" > "$output_file"
    echo "✅ Filtered data saved to $output_file"
}
filter_department "cleaned_employees.csv" "IT" "it_department.csv"
```

### **Explanation**
- `awk -F, -v dept="$department" '$4 ~ dept'`: Filters rows where the 4th column (Department) matches the specified department (`IT`).

### **Output: `it_department.csv`**
```
ID, Name, Age, Department, Salary
101, John Doe, 30, IT, 55000
103, Alice Brown, 26, IT, 60000
```

---

## **3. Extracting JSON Data**

### **Shell Script**
```zsh
#!/bin/zsh
extract_json() {
    input_file="$1"
    output_file="$2"
    # Use jq to extract name and price fields
    jq -r '.[] | "\(.name), \(.price)"' "$input_file" > "$output_file"
    echo "✅ Extracted product data saved to $output_file"
}
extract_json "products.json" "products_list.txt"
```

### **Explanation**
- `jq -r '.[] | "\(.name), \(.price)"'`: Extracts the `name` and `price` fields from each JSON object and formats them as a CSV-like output.

### **Sample Input File: `products.json`**
```json
[
  {"id": 1, "name": "Laptop", "category": "Electronics", "price": 1200},
  {"id": 2, "name": "Phone", "category": "Electronics", "price": 800},
  {"id": 3, "name": "Desk", "category": "Furniture", "price": 150}
]
```

### **Output: `products_list.txt`**
```
Laptop, 1200
Phone, 800
Desk, 150
```

> **Note:** Install `jq` using:
> ```bash
> brew install jq
> ```

---

## **4. Analyzing Log Files**

### **Shell Script**
```zsh
#!/bin/zsh
analyze_logs() {
    input_file="$1"
    output_file="$2"
    # Extract error messages
    grep "\[ERROR\]" "$input_file" > "$output_file"
    # Count total errors
    error_count=$(wc -l < "$output_file")
    echo "Total Errors: $error_count" >> "$output_file"
    echo "✅ Error report saved to $output_file"
}
analyze_logs "server.log" "error_report.txt"
```

### **Explanation**
1. **Extract Errors:** `grep "\[ERROR\]"` filters lines containing `[ERROR]`.
2. **Count Errors:** `wc -l` counts the number of error lines.
3. **Append Total Count:** The total count is appended to the output file.

### **Sample Input File: `server.log`**
```
[INFO] Server started successfully
[ERROR] Database connection failed
[INFO] User logged in
[ERROR] Failed to load configuration
[WARNING] Low disk space
```

### **Output: `error_report.txt`**
```
[ERROR] Database connection failed
[ERROR] Failed to load configuration
Total Errors: 2
```
