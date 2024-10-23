# Installation of OCI CLI with pipx usage of `vim`, and configuration for `zsh`:

## Prerequisites
Before proceeding, ensure that you have the following dependencies installed:

- **Python 3** (preferably Python 3.11 or higher)
- **pip** (Python package installer)
- **pipx** (Python package for managing applications in isolated environments)
- **vim** (text editor for modifying configuration files)
- **zsh** (shell if you prefer it over bash)

You can install the necessary packages using:
```bash
sudo apt install python3 python3-pip pipx vim zsh
```

## Step 1: Remove Previous Installation (if applicable)
If there are previous installations of `oci-cli` that need to be removed, follow these steps:

1. **Check Current Installations**:
   ```bash
   pip3 list | grep oci
   ```

2. **Try to Uninstall `oci-cli`**:
   ```bash
   sudo pip3 uninstall oci-cli
   ```

3. **If unable to uninstall, you can skip this step.**

## Step 2: Install pipx
If `pipx` is not installed yet, install it:

```bash
sudo apt install pipx
```

## Step 3: Add pipx to PATH
1. Run the command to ensure the pipx directory is in your PATH:
   ```bash
   pipx ensurepath
   ```
   
2. **If you're using zsh, manually add it to your `.zshrc` configuration file**:
   ```bash
   echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
   ```

3. **Open the `.zshrc` file in vim to edit**:
   ```bash
   vim ~/.zshrc
   ```
   Add the line:
   ```bash
   export PATH="$HOME/.local/bin:$PATH"
   ```
   Save and exit vim by pressing `Esc`, then type `:wq`, and hit `Enter`.

4. **Source the `.zshrc` file to apply changes**:
   ```bash
   source ~/.zshrc
   ```

## Step 4: Install OCI CLI with pipx
1. Now, install `oci-cli` using `pipx`:
   ```bash
   pipx install oci-cli
   ```

## Step 5: Verify the Installation
After installation, verify the `oci` version to confirm everything is set up correctly:
```bash
oci --version
```

## Note
- **If you encounter warnings about symlinks**, these can be ignored unless you wish to resolve them manually.
```

Feel free to copy this guide for your reference! If you need further assistance, let me know!