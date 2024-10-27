## Steps to Install OCI CLI on Ubuntu 24.04

### 1. Update System Packages

```bash
sudo apt update
```

### 2. Install Required Dependencies

Make sure that Python and essential libraries are installed, as they are required for OCI CLI:

```bash
sudo apt install -y python3 python3-pip python3-distutils python3-dev build-essential libffi-dev
```

- **python3**: Ensures Python 3 is available.
- **python3-pip**: Enables installation of Python packages.
- **python3-distutils**: Required by OCI CLI for installation.
- **python3-dev** and **libffi-dev**: Provide necessary libraries and headers for building Python modules.
- **build-essential**: Provides compilation tools like `gcc` which are sometimes needed.

### 3. Install the OCI CLI using pip

Install OCI CLI with `pip`:

```bash
sudo pip3 install oci-cli
```

> **Note**: Using `sudo` here installs OCI CLI system-wide.

### 4. Configure OCI CLI Path (if needed)

If `oci` is not immediately recognized as a command, add Python’s package location to your PATH. You can locate the installation path of OCI by running:

```bash
pip3 show oci-cli | grep Location
```

If it’s installed in a directory like `/usr/local/lib/python3.11/dist-packages`, add that to your PATH by editing `~/.bashrc` or `~/.zshrc`:

```bash
echo 'export PATH=$PATH:/usr/local/lib/python3.11/dist-packages/bin' >> ~/.bashrc
source ~/.bashrc
```

### 5. Verify the Installation

To confirm that the OCI CLI installed correctly, run:

```bash
oci --version
```

You should see the version number if everything is set up properly.

---

Following these steps should install the OCI CLI system-wide on Ubuntu 24.04 without a virtual environment.