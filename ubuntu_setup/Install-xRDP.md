To access your Ubuntu machine from Windows using RDP, follow these steps:  

### **1. Install xRDP on Ubuntu**  
On your Ubuntu machine, open a terminal and run:  
```bash
sudo apt update && sudo apt install xrdp -y
```

### **2. Start and Enable xRDP**  
```bash
sudo systemctl enable --now xrdp
```
Check its status to ensure it's running:  
```bash
sudo systemctl status xrdp
```

### **3. Allow RDP Port (3389) in the Firewall**  
If UFW (Uncomplicated Firewall) is enabled, allow RDP traffic:  
```bash
sudo ufw allow 3389/tcp
```

### **4. Find Ubuntu's IP Address**  
Run:  
```bash
ip a
```
Look for an IP address under `eth0` or `wlan0` (e.g., `192.168.x.x`).

### **5. Connect from Windows**  
1. Press `Win + R`, type `mstsc`, and hit Enter.  
2. Enter Ubuntu's IP and click **Connect**.  
3. Log in using your Ubuntu username and password.  

### **6. Fix Possible Issues**  
If you get a black screen or connection issues, you may need to install a desktop environment:  
```bash
sudo apt install xfce4 -y
echo "xfce4-session" > ~/.xsession
```
Restart xRDP:  
```bash
sudo systemctl restart xrdp
```