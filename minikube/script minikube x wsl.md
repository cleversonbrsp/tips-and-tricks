## This script will allocate **4 CPUs** and **8 GB** of memory for Minikube and ensure that DNS settings are applied correctly without duplicating existing entries.

```bash
#!/bin/bash

# Function to add nameserver to /etc/resolv.conf without duplicates
add_nameserver_if_not_exists() {
    local nameserver="$1"
    local resolv_file="$2"

    # Check if the nameserver is already present in the file
    if ! grep -q "$nameserver" "$resolv_file"; then
        echo "Adding $nameserver to $resolv_file..."
        sudo bash -c "echo 'nameserver $nameserver' >> $resolv_file"
    else
        echo "$nameserver is already present in $resolv_file, skipping duplication."
    fi
}

# Configure DNS in /etc/resolv.conf for Ubuntu WSL
echo "Configuring DNS in /etc/resolv.conf for Ubuntu WSL..."
add_nameserver_if_not_exists "8.8.8.8" "/etc/resolv.conf"
add_nameserver_if_not_exists "8.8.4.4" "/etc/resolv.conf"

# Start Minikube with 1 master node and 1 worker node, allocating 4 CPUs and 8 GB of RAM
echo "Starting Minikube with 1 master node and 1 worker node..."
minikube start --nodes 2 --cpus=4 --memory=8192

# Check if Minikube started successfully
if [ $? -eq 0 ]; then
    echo "Minikube started successfully."
else
    echo "Failed to start Minikube."
    exit 1
fi

# Function to add nameserver inside Minikube without altering existing ones
add_minikube_nameserver_if_not_exists() {
    local nameserver="$1"
    local resolv_file="/etc/resolv.conf"

    # Command to check and add the nameserver inside Minikube via SSH
    minikube ssh -- "if ! grep -q '$nameserver' $resolv_file; then echo 'nameserver $nameserver' | sudo tee -a $resolv_file; else echo '$nameserver is already present in $resolv_file'; fi"
}

# Configure DNS in /etc/resolv.conf for Minikube
echo "Configuring DNS in /etc/resolv.conf inside Minikube..."
add_minikube_nameserver_if_not_exists "8.8.8.8"
add_minikube_nameserver_if_not_exists "8.8.4.4"

echo "Automation completed successfully!"
```

### How to Use:
1. Save the script in a file, for example, `setup_minikube.sh`.
2. Grant execute permissions: 
   ```bash
   chmod +x setup_minikube.sh
   ```
3. Run the script: 
   ```bash
   ./setup_minikube.sh
   ```

