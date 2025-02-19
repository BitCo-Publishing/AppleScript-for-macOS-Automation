
# Zsh Features and Enhancements

## Smarter Autocompletion & Auto-Suggestions

### Zsh Example:
```bash
$ git che<Press TAB>
```

### Bash Equivalent:
```bash
$ git che<TAB> (No suggestions unless manually configured)
```

### Example:
```bash
$ lss -l
zsh: command not found: lss  (Appears red)
```

---

## Spelling Correction & Error Handling

### Zsh Example:
```bash
$ cd Documants
zsh: correct 'Documants' to 'Documents'? [y/n]
```

### Bash Example:
```bash
$ cd Documants
bash: cd: Documants: No such file or directory
```

---

## Advanced File Search

### Zsh Example:
```bash
$ ls **/*.log
```

### Bash Equivalent:
```bash
$ find . -name "*.log"
```

---

## Persistent Sessions & State Saving

```bash
$ cd ~/Projects
$ exit
$ <Reopen Terminal>
$ pwd
/Users/yourname/Projects
```

---

## Aliases & Custom Shortcuts

### Create an alias for `git status`:
```bash
alias gs="git status"
```

### Usage:
```bash
$ gs
```

---

## Oh My Zsh Theme Management

### Change Theme:
```bash
$ omz theme set robbyrussell
```

---

## Faster Navigation with Auto-Suggestions

```bash
$ cd ~/Documents/Projects/Code
zsh will auto-suggest the full path
$ cd Documents/Projects/Code
```

---

## Smarter File Management

```bash
$ mv **/*.(jpg|png) ~/Pictures/
```

---

## Command History Efficiency

```bash
$ history | grep npm
```

---

## Installing Oh My Zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

## Customizing Oh My Zsh

### Edit `.zshrc`:
```bash
nano ~/.zshrc
```

### Change Theme:
```bash
ZSH_THEME="agnoster"
```

### Apply Changes:
```bash
source ~/.zshrc
```

---

## Installing Powerlevel10k Theme

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

---

## Enabling Plugins in Zsh

### Edit `.zshrc`:
```bash
nano ~/.zshrc
```

### Add Plugins:
```bash
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

### Apply Changes:
```bash
source ~/.zshrc
```

---

## Installing `zsh-autosuggestions`

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

---

## Zsh Command History

### View Command History:
```bash
history
```

### Run the Last Command Again:
```bash
!!
```

### Go to the Previous Directory:
```bash
cd -
```

### Change to the Root Directory:
```bash
cd /
```

### Go to the Home Directory:
```bash
cd ~
```

---

## Creating Aliases

### Create an Alias:
```bash
alias ll="ls -la"
```

### Make It Permanent:
```bash
echo 'alias ll="ls -la"' >> ~/.zshrc
```

### Reload Zsh Configuration:
```bash
source ~/.zshrc
```

---

## Variables in Zsh

### Set a Variable:
```bash
MY_NAME="John"
```

### Access the Variable:
```bash
echo $MY_NAME
```

---

## Background Jobs

### Run a Command in the Background:
```bash
long_running_task &
```

### View Running Jobs:
```bash
jobs
```

### Bring a Job to the Foreground:
```bash
fg %1
```

---

## Advanced File Operations

### List All `.txt` Files in Subdirectories:
```bash
ls **/*.txt
```

### Delete All `.log` Files (with Prompt):
```bash
rm -i **/*.log
```

---

# Shell Scripting Basics

## Creating a Basic Shell Script

### Navigate to the Script Directory:
```bash
cd ~/Documents/Scripts
```

### Create a Script File:
```bash
nano myscript.sh
```

### Add the Shebang and Commands:
```bash
#!/bin/zsh
echo "Hello, World!"
date
whoami
```

---

## Making the Script Executable

```bash
chmod +x myscript.sh
```

### Run the Script:
```bash
./myscript.sh
```

---

## Running a Shell Script from Anywhere

### Move the Script to `/usr/local/bin/`:
```bash
sudo mv myscript.sh /usr/local/bin/myscript
```

### Run the Script:
```bash
myscript
```

---

# Advanced Shell Scripting

## Directory Cleanup Script

### Script:
```bash
#!/bin/zsh

# Define target directories
LOG_DIR="$HOME/Logs"
ARCHIVE_DIR="$HOME/Documents/Archive"
TRASH_DIR="$HOME/.Trash"

echo "Starting directory cleanup..."

# Delete log files older than 7 days
find "$LOG_DIR" -name "*.log" -type f -mtime +7 -exec rm {} \;
echo "Old log files removed from $LOG_DIR."

# Move old documents to the archive folder
find "$HOME/Documents" -name "*.pdf" -mtime +30 -exec mv {} "$ARCHIVE_DIR" \;
echo "Old PDF documents moved to $ARCHIVE_DIR."

# Empty the trash folder
rm -rf "$TRASH_DIR"/*
echo "Trash folder emptied."

echo "Cleanup complete!"
```

### Make It Executable:
```bash
chmod +x cleanup.sh
```

### Run the Script:
```bash
./cleanup.sh
```

---

## Automating with `cron`

### Open the Cron Editor:
```bash
crontab -e
```

### Schedule the Script:
```bash
0 0 * * 0 /path/to/cleanup.sh
```

---

## Simple Math in Shell

### Using `expr`:
```bash
expr 5 + 10
```

### Using `$(())`:
```bash
echo $((15 * 3))
```

---

## Calculator Script

### Script:
```bash
#!/bin/zsh

echo "Welcome to the Shell Calculator!"

# Get user input
read -p "Enter first number: " num1
read -p "Enter second number: " num2

# Perform calculations
sum=$((num1 + num2))
diff=$((num1 - num2))
prod=$((num1 * num2))
if [ $num2 -ne 0 ]; then
    quot=$(echo "scale=2; $num1 / $num2" | bc)
else
    quot="undefined (division by zero)"
fi

# Display results
echo "Results:"
echo "Addition: $sum"
echo "Subtraction: $diff"
echo "Multiplication: $prod"
echo "Division: $quot"
```

### Make It Executable:
```bash
chmod +x calculator.sh
```

### Run the Script:
```bash
./calculator.sh
```

### Example Output:
```
Welcome to the Shell Calculator!
Enter first number: 10
Enter second number: 5

Results:
Addition: 15
Subtraction: 5
Multiplication: 50
Division: 2.00
```
```
