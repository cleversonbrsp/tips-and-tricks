# Installing Zsh and Oh My Zsh on Ubuntu 24.04

This guide provides step-by-step instructions to install `zsh` and `oh-my-zsh` on Ubuntu 24.04, covering all necessary dependencies.

---

## Step 1: Install Zsh

1. **Update package lists:**
   ```bash
   sudo apt update
   ```
2. **Install Zsh:**
   ```bash
   sudo apt install -y zsh
   ```
3. **Verify the installation:**
   ```bash
   zsh --version
   ```
4. **Set Zsh as the default shell:**
   ```bash
   chsh -s $(which zsh)
   ```
5. **Restart your terminal** to start using Zsh as the default shell.

---

## Step 2: Install Git (Required for Oh My Zsh)

Oh My Zsh needs Git to clone its repository.

1. **Install Git:**
   ```bash
   sudo apt install -y git
   ```
2. **Verify Git installation:**
   ```bash
   git --version
   ```

---

## Step 3: Install Oh My Zsh

1. **Run the installation command for Oh My Zsh:**
   ```bash
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   ```
   This command will:
   - Clone the Oh My Zsh repository
   - Create a `~/.zshrc` configuration file if it doesn't exist
   - Set up Oh My Zsh with default themes and plugins

2. **Follow any prompts** to complete the installation.

---

## Step 4: Customize Zsh with Oh My Zsh Themes and Plugins

1. **Edit the `~/.zshrc` file** to customize your configuration:
   ```bash
   nano ~/.zshrc
   ```
2. **Set your desired theme** by modifying the `ZSH_THEME` variable, for example:
   ```zsh
   ZSH_THEME="agnoster"
   ```
3. **Add plugins** to enhance functionality, such as `git`:
   ```zsh
   plugins=(git)
   ```
4. **Save and close the file** (`Ctrl + X`, then `Y`, and `Enter` in nano).

5. **Apply the changes** by reloading the configuration:
   ```bash
   source ~/.zshrc
   ```
