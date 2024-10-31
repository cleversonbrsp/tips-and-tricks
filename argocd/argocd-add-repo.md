# Setting Up ArgoCD CLI and Syncing with a Private GitHub Repository on Linux

## Prerequisites
- An ArgoCD instance is already deployed and accessible.
- You have access to a private GitHub repository (`git@github.com:cleversonbrsp/dev-ops.git`).

---

## 1. Install the ArgoCD CLI on Linux

### Step 1: Download and Install the CLI
```bash
sudo curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo chmod +x /usr/local/bin/argocd
```

### Step 2: Verify Installation
Run:
```bash
argocd version
```

---

## 2. Generate an SSH Key for GitHub Access

### Step 1: Create the SSH Key Pair (skip if you already have a key)
Generate a new SSH key to allow ArgoCD access to your private GitHub repository:
```bash
ssh-keygen -t rsa -b 4096 -C "argocd-github-access" -f ~/.ssh/argocd_github_key
```

- This will create two files:
  - **Private key**: `~/.ssh/argocd_github_key`
  - **Public key**: `~/.ssh/argocd_github_key.pub`

---

## 3. Add the SSH Key to GitHub

1. Copy the public key to your clipboard:
   ```bash
   cat ~/.ssh/argocd_github_key.pub
   ```

2. Go to your GitHub repository's **Settings > Deploy keys**.
3. Click **Add deploy key**, paste the public key, and name it (e.g., "ArgoCD Access Key").
4. **Allow write access** if ArgoCD needs to write, or leave unchecked for read-only.

---

## 4. Log in to the ArgoCD Server

### Step 1: Log in to the ArgoCD Server
Replace `<argocd-server-address>` with the actual address of your ArgoCD server (e.g., `argocd.example.com`):
```bash
argocd login <argocd-server-address> --insecure
```

- The `--insecure` option bypasses TLS verification, which is helpful if ArgoCD uses a self-signed certificate.
- Enter your ArgoCD username and password when prompted.

---

## 5. Add the GitHub Repository to ArgoCD

With the ArgoCD CLI logged in to the server, add your GitHub repository:

```bash
argocd repo add git@github.com:cleversonbrsp/dev-ops.git --ssh-private-key-path ~/.ssh/argocd_github_key
```

---

## 6. Verify Repository Addition

To confirm the repository is successfully added:
```bash
argocd repo list
```