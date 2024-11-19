Certainly! Here’s the updated Markdown guide with "navita" changed to "customer-name":

---

# **Managing SSH Keys and Cloning Repositories for GitHub (Customer Name)**

This guide explains how to manage multiple SSH keys for GitHub (Customer Name) and clone repositories using SSH.

## **1. Setup SSH Keys for Multiple GitHub Accounts**

To work with multiple GitHub repositories (e.g., personal and client repositories), you need to configure multiple SSH keys and use them in your `ssh-agent` and `~/.ssh/config`.

### **Step 1: Generate SSH Keys (If Needed)**

If you haven't already generated the SSH keys for each GitHub account, follow these steps:

1. **For Personal GitHub**:
   ```bash
   ssh-keygen -t ed25519 -C "your-email@example.com" -f ~/.ssh/id_ed25519
   ```

2. **For Client GitHub (Customer Name)**:
   ```bash
   ssh-keygen -t ed25519 -C "client-email@example.com" -f ~/.ssh/id_ed25519_githubcustomername
   ```

### **Step 2: Add the SSH Keys to GitHub**

Add the public keys to the corresponding GitHub accounts:

- **Personal GitHub**:
  - Go to GitHub > Settings > SSH and GPG keys > New SSH key.
  - Paste the contents of `~/.ssh/id_ed25519.pub`.

- **Client GitHub (Customer Name)**:
  - Go to GitHub > Settings > SSH and GPG keys > New SSH key.
  - Paste the contents of `~/.ssh/id_ed25519_githubcustomername.pub`.

### **Step 3: Configure `~/.ssh/config` for Multiple SSH Keys**

Create or edit the `~/.ssh/config` file to define separate configurations for each GitHub account:

```plaintext
# Configuration for Personal GitHub (Cleverson)
Host github-cleverson
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes

# Configuration for Client GitHub (Customer Name)
Host github-customer-name
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_githubcustomername
  IdentitiesOnly yes
```

### **Step 4: Add SSH Keys to `ssh-agent`**

1. Start the `ssh-agent` if it's not running already:
   ```bash
   eval "$(ssh-agent -s)"
   ```

2. Add the SSH keys to the agent:
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ssh-add ~/.ssh/id_ed25519_githubcustomername
   ```

3. Verify the keys have been loaded:
   ```bash
   ssh-add -l
   ```

You should see both keys listed.

---

## **2. Test SSH Connection**

Before cloning repositories, test if your SSH setup is working correctly for each account.

- **Test Personal GitHub**:
  ```bash
  ssh -T git@github-cleverson
  ```

  You should see:
  ```plaintext
  Hi cleversonr0drigues! You've successfully authenticated, but GitHub does not provide shell access.
  ```

- **Test Client GitHub (Customer Name)**:
  ```bash
  ssh -T git@github-customer-name
  ```

  You should see:
  ```plaintext
  Hi <client-username>! You've successfully authenticated, but GitHub does not provide shell access.
  ```

---

## **3. Clone Repositories Using SSH**

### **Step 1: Clone Repositories**

Once your SSH keys are configured, you can clone repositories using the specific alias for each GitHub account.

- **Clone a Personal GitHub Repository**:
  ```bash
  git clone git@github-cleverson:cleversonr0drigues/my-repository.git
  ```

- **Clone a Client GitHub (Customer Name) Repository**:
  ```bash
  git clone git@github-customer-name:customer-name-hub/client-repository.git
  ```

### **Step 2: Verify Remote URL (If Needed)**

To check or change the remote URL for a cloned repository:

1. **Navigate to the repository directory**:
   ```bash
   cd my-repository
   ```

2. **View the current remote URL**:
   ```bash
   git remote -v
   ```

3. **Change the remote URL** (if necessary):
   ```bash
   git remote set-url origin git@github-customer-name:customer-name-hub/client-repository.git
   ```

---

## **4. Troubleshooting**

### **"Repository not found" Error**

This error typically occurs if:

- The repository name is incorrect (case-sensitive).
- Your account doesn't have access to the repository.

### **"Permission denied (publickey)" Error**

This error occurs if your SSH key is not correctly added to the `ssh-agent` or GitHub account. Ensure the key is loaded into the agent:

```bash
ssh-add ~/.ssh/id_ed25519
```

You can also confirm if you have the right SSH key by running:

```bash
ssh -T git@github-cleverson
```

---

## **5. General Tips**

- **Make sure your `~/.ssh/config` file is correct**, especially the `Host` and `IdentityFile` settings.
- **Use specific aliases** for each GitHub account (e.g., `github-cleverson` for personal and `github-customer-name` for the client).
- **Check permissions** if you encounter access issues with repositories.

---

### **End of Guide**

This guide covers the full process of managing multiple SSH keys and cloning GitHub repositories. By following these steps, you can easily manage different accounts and repositories for personal and professional use.