## What is Homebrew?

Homebrew (often called brew) is a package manager for macOS and Linux that allows users to easily install, update, and manage software applications and command-line tools.

## Checking Your Default Shell

Before installing Homebrew, it's essential to verify which shell you are using. Run:

```sh
echo $SHELL  # or
echo $0
```

If you're using Zsh (which is the default on modern macOS versions), you may need to configure certain environment variables differently than in Bash.

### (Optional) Switch to Zsh

If your default shell is not Zsh, you can switch using:

```sh
cat /etc/shells  # List available shells
chsh -s /bin/zsh  # Change default shell to Zsh
```

Close the terminal and reopen it for changes to take effect.

---

## Prerequisites

Before you start installing and using Homebrew to manage packages, make sure you have the **Command Line Tools (CTL)** installed by running the following command:

- Check if CTL is installed:

```sh
xcode-select -p
```

If it does not return a valid installation path or shows an error like "command not found", install CTL using:

```sh
xcode-select install
```

This will prompt a macOS dialog to install the necessary tools. Follow the instructions and wait for the installation to complete.

## Installing Homebrew

### Step 1: Open the Homebrew Website

Run:

```sh
xdg-open https://brew.sh  # Linux
open https://brew.sh  # macOS
```

Or visit [Homebrew](https://brew.sh) manually.

### Step 2: Copy and Run the Installation Command

Copy the installation script from the website and run it in your terminal:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

- You may need to enter your password to allow `sudo` access.
- Follow interactive instructions to add Homebrew to your `PATH`.
- To **uninstall** Homebrew, replace `/install.sh/` with `/uninstall.sh/` in the command.
- Before uninstalling Homebrew, first remove all installed packages to avoid orphaned files.

### Step 3: Configure Environment Variables

Since `zprofile` loads once per login session, set the Homebrew path there:

```sh
cat ~/.zprofile  # View existing configurations
```

To modify it, use:

```sh
vim ~/.zprofile  # or
nano ~/.zprofile
```

If Homebrew is uninstalled, ensure you remove its path from `.zprofile` or `.zshrc` to prevent errors.

### Step 4: Verify Installation

Run:

```sh
brew --prefix  # Confirm Homebrew’s location
brew -v  # Check installed version
```

---

## Installing Git Using Homebrew

Even though Git is preinstalled on macOS and Linux, installing it via Homebrew ensures easier updates.

```sh
brew search git  # Search for Git
brew install git  # Install Git
brew install --formula git  # Explicitly install as a formula
```

During installation, dependencies like `gettext`, `pcre2`, and `libunistring` may also be installed.

### Verify Installation

```sh
git --version  # Check Git version
which git  # Locate Git installation
```

---

## Understanding Homebrew Package Types

Homebrew has two main package types:

- **Formulae** (command-line tools, e.g., `git`)
- **Casks** (GUI applications, e.g., `Arc Browser`)

To install a cask package:

```sh
brew search arc  # Search for Arc Browser
brew install --cask arc  # Install Arc Browser
```

Good practice: Always use `--cask` when installing GUI apps.

### Listing Installed Packages

```sh
brew list  # List installed packages
brew list | grep git  # Filter results using `grep`
```

### Managing Homebrew Packages

Before upgrading or removing Homebrew packages, ensure Terminal has necessary permissions:

- Go to **Apple icon → Privacy & Security → App Management**
- Toggle the button for **Terminal**

#### Upgrade Packages

```sh
brew upgrade --cask arc  # Upgrade a specific package
brew upgrade --greedy  # Upgrade all dependencies
```

#### Uninstall Packages

```sh
brew uninstall --formula <package-name>  # Remove a formula package
brew uninstall --cask <package-name>  # Remove a cask package
brew uninstall --zap --cask <package-name>  # Extra cleanup for GUI apps
```

> The `--zap` flag in `brew uninstall` should only be used for cask packages (GUI applications)

#### Cleanup Unused Packages

```sh
brew cleanup  # Free up space by removing old versions
brew autoremove  # Remove orphaned dependencies
```

---

## Exploring Homebrew Commands

```sh
brew help  # View basic commands
brew info <package-name>  # Get details about a package
man brew  # View all available commands
brew home  # Open Homebrew homepage
brew docs  # Open Homebrew documentation
```

For searching packages manually, visit [Homebrew Formulae](https://formulae.brew.sh).

---

## User-Specific Settings: Opening Websites from Terminal

To avoid typing `https://` every time, add this function to your `~/.zshrc` (for Zsh) or `~/.bashrc` (for Bash):

### macOS (Uses `open`)

```sh
oh() {
  local input="$*"
  if [[ "$input" =~ ^[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ && ! "$input" =~ \  ]]; then
    open "https://$input"
  else
    open "https://www.google.com/search?q=$(echo "$input" | tr ' ' '+')"
  fi
}
```

### Linux (Uses `xdg-open`)

```sh
oh() {
  local input="$*"
  if [[ "$input" =~ ^[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ && ! "$input" =~ [[:space:]] ]]; then
    xdg-open "https://$input"
  else
    xdg-open "https://www.google.com/search?q=$(echo "$input" | tr ' ' '+')"
  fi
}
```

Source the file to apply changes:

```sh
source ~/.zshrc  # or source ~/.bashrc
```

Now, simply typing `brew.sh` will open the Homebrew website!

---

This guide ensures a smooth Homebrew installation and management experience. 🚀
