
# Find All PDF Files in a Folder

```bash
find ~/Documents -name "*.pdf"
```

---

# Show Hidden Files in Finder

```bash
defaults write com.apple.finder AppleShowAllFiles -bool true && killall Finder
```

---

# Automate File Backups

```bash
rsync -av ~/Documents/ ~/Backup/
```

---

# Basic Terminal Commands

## 1. `pwd` (Print Working Directory)

```bash
pwd
```

## 2. `ls` (List Directory Contents)

```bash
ls
```

### List All Files

```bash
ls -la
```

## 3. `cd` (Change Directory)

- Change to `Documents`:
  ```bash
  cd Documents
  ```
- Move up one level:
  ```bash
  cd ..
  ```
- Go directly to a specific location:
  ```bash
  cd ~/Downloads
  ```

## 4. `clear` (Clear the Screen)

```bash
clear
```

## 5. `mkdir` (Make Directory)

- Create a directory:
  ```bash
  mkdir Projects
  ```
- Create multiple directories at once:
  ```bash
  mkdir Project1 Project2 Project3
  ```

## 6. `touch` (Create a New File)

```bash
touch notes.txt
```

## 7. `cp` (Copy Files and Directories)

- Copy a file:
  ```bash
  cp notes.txt Backup/
  ```
- Copy an entire folder:
  ```bash
  cp -r Documents/ Backup/
  ```

## 8. `mv` (Move and Rename Files)

- Move a file:
  ```bash
  mv notes.txt Documents/
  ```
- Rename a file:
  ```bash
  mv notes.txt mynotes.txt
  ```

## 9. `rm` (Remove Files and Directories)

- Delete a single file:
  ```bash
  rm notes.txt
  ```
- Delete a folder and everything inside it:
  ```bash
  rm -r OldProjects/
  ```

---

# Combining Commands

- Create a folder, move files into it, and list its contents in a single command:
  ```bash
  mkdir Reports && mv *.pdf Reports/ && ls -l Reports/
  ```

---

# Customizing the Terminal Prompt

## Temporarily Changing the Prompt

```bash
export PS1="\u@\h \w> "
```

- `\u` → Displays your username
- `\h` → Shows your computer’s hostname
- `\w` → Displays the current directory
- `>` → A custom symbol for clarity

---

# For Zsh Users

- Open the Zsh profile file in Nano editor:
  ```bash
  nano ~/.zshrc
  ```
- Add this line at the end of the file:
  ```bash
  export PS1="%F{cyan}%n@%m %~ %# %f"
  ```

---

# For Bash Users

- Open the Bash profile:
  ```bash
  nano ~/.bash_profile
  ```
- Add the following:
  ```bash
  export PS1="\e[1;32m\u@\h:\w\$ \e[m"
  ```
- Save the file and apply the changes:
  ```bash
  source ~/.bash_profile
  ```

---

# Adding Colors to Your Prompt

```bash
export PS1="%F{green}%n@%m %F{blue}%~ %# %f"
```

---

# Environment Variables

## View All Environment Variables

```bash
printenv
```

## View Your Current `PATH`

```bash
echo $PATH
```

- Typical output:
  ```
  /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
  ```

## Change the Default Editor

- Set it to Nano:
  ```bash
  export EDITOR=nano
  ```
- Set it to VS Code (if installed):
  ```bash
  export EDITOR="code -w"
  ```

## Control Command History Length

```bash
export HISTSIZE=5000
```

---

# Making Environment Variable Changes Permanent

- For Zsh (macOS Catalina and later):
  ```bash
  nano ~/.zshrc
  ```
- For Bash (macOS Mojave and earlier):
  ```bash
  nano ~/.bash_profile
  ```

---

# Modified Environment Variables

```bash
export PS1="%F{cyan}%n@%m %~ %# %f"
export PATH=$PATH:~/scripts
export EDITOR=nano
export HISTSIZE=5000
```

---

# Advanced Customization with Aliases

- Create an alias for clearing the screen:
  ```bash
  alias cls='clear'
  ```
- Create a shortcut for opening Finder from Terminal:
  ```bash
  alias finder='open .'
  ```
- Make an alias for navigating to a common directory:
  ```bash
  alias work='cd ~/Projects/Work'
  ```

# Xcode Command Line Tools

## Check Xcode-Select Version

```bash
xcode-select --version
```

## Install Xcode Command Line Tools

```bash
xcode-select --install
```

---

# Homebrew Installation

## Install Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## Check Homebrew Status

```bash
brew doctor
```

- Expected output:
  ```
  Your system is ready to brew!
  ```

---

# Using Homebrew

## Install a Package (e.g., `wget`)

```bash
brew install wget
```

## List Installed Packages

```bash
brew list
```

## Update Homebrew

```bash
brew update
```

## Uninstall Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
```

## Troubleshooting Homebrew

If a package won’t install, run:

```bash
brew doctor
```

---

# Zsh Setup

## Check Zsh Version

```bash
zsh --version
```

## Install Zsh via Homebrew

```bash
brew install zsh
```

## Set Zsh as Default Shell

```bash
chsh -s /bin/zsh
```

---

# Oh My Zsh Installation

## Install Oh My Zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

# Customizing Oh My Zsh

## Edit `.zshrc` File

```bash
nano ~/.zshrc
```

## Change Zsh Theme

Find the line:
```bash
ZSH_THEME="robbyrussell"
```

Change it to:
```bash
ZSH_THEME="agnoster"
```

## Save and Apply Changes

1. Save and exit (`CTRL + X`, then `Y`, then `ENTER`).
2. Apply changes:
   ```bash
   source ~/.zshrc
   ```

---

# Enable Oh My Zsh Plugins

## Edit `.zshrc` File

```bash
nano ~/.zshrc
```

## Add Plugins

Add the following line:
```bash
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

## Apply Changes

```bash
source ~/.zshrc
```

## Reload Oh My Zsh

```bash
omz reload
```

---

# Fix Permissions for Oh My Zsh

```bash
chmod -R 755 ~/.oh-my-zsh
```

---

# Additional Notes

- To reload `.zshrc` manually:
  ```bash
  source ~/.zshrc
  ```

```
```
