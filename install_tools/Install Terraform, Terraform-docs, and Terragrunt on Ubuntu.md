# Install Terraform, Terraform-docs, and Terragrunt on Ubuntu

## 1. Install Terraform

1. **Add HashiCorp GPG key:**
   ```bash
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
   ```

2. **Add the HashiCorp repository:**
   ```bash
   sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
   ```

3. **Update package list and install Terraform:**
   ```bash
   sudo apt update && sudo apt install terraform
   ```

4. **Verify the installation:**
   ```bash
   terraform --version
   ```

## 2. Install Terraform-docs

1. **Download the latest version of Terraform-docs:**
   ```bash
   wget https://github.com/terraform-docs/terraform-docs/releases/download/v0.16.0/terraform-docs-v0.16.0-linux-amd64.tar.gz
   ```

2. **Extract the binary:**
   ```bash
   tar -xzf terraform-docs-v0.16.0-linux-amd64.tar.gz
   ```

3. **Move the binary to `/usr/local/bin/`:**
   ```bash
   sudo mv terraform-docs /usr/local/bin/
   ```

4. **Clean up the downloaded files:**
   ```bash
   rm terraform-docs-v0.16.0-linux-amd64.tar.gz
   ```

5. **Verify the installation:**
   ```bash
   terraform-docs --version
   ```

> Replace `v0.16.0` with the latest version available from the [official releases page](https://github.com/terraform-docs/terraform-docs/releases).

## 3. Install Terragrunt

1. **Download the latest version of Terragrunt:**
   ```bash
   wget https://github.com/gruntwork-io/terragrunt/releases/download/v0.51.0/terragrunt_linux_amd64
   ```

2. **Make it executable:**
   ```bash
   chmod +x terragrunt_linux_amd64
   ```

3. **Move it to `/usr/local/bin/`:**
   ```bash
   sudo mv terragrunt_linux_amd64 /usr/local/bin/terragrunt
   ```

4. **Verify the installation:**
   ```bash
   terragrunt --version
   ```

> Replace `v0.51.0` with the latest version from the [official releases page](https://github.com/gruntwork-io/terragrunt/releases).
```

You can copy and paste this into any Markdown editor or README file.