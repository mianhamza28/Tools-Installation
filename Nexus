Here's the **complete, corrected, and optimized** Nexus installation guide with Java 17 installation and proper user account setup:

### **Complete Nexus Installation on Ubuntu**

#### **1. Install Java 17 (Prerequisite)**
```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
java -version  # Verify installation (should show Java 17)
```

#### **2. Create Nexus User Account**
```bash
sudo adduser --system --no-create-home --disabled-login --group nexus
```

#### **3. Download Nexus**
```bash
cd /opt
sudo wget https://download.sonatype.com/nexus/3/nexus-3.79.1-04-linux-x86_64.tar.gz
```

#### **4. Extract and Install Nexus**
```bash
sudo tar xvzf nexus-3.79.1-04-linux-x86_64.tar.gz
sudo mv nexus-3.79.1-04 nexus
```

#### **5. Set Permissions**
```bash
sudo chown -R nexus:nexus /opt/nexus
sudo chown -R nexus:nexus /opt/sonatype-work
```

#### **6. Configure Nexus User**
```bash
sudo bash -c 'echo "run_as_user=\"nexus\"" > /opt/nexus/bin/nexus.rc'
```

#### **7. Create Systemd Service**
```bash
sudo tee /etc/systemd/system/nexus.service << 'EOF'
[Unit]
Description=Nexus Repository Manager
After=network.target

[Service]
Type=forking
User=nexus
LimitNOFILE=65536
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
Restart=on-failure

[Install]
WantedBy=multi-user.target
EOF
```

#### **8. Start Nexus Service**
```bash
sudo systemctl daemon-reload
sudo systemctl start nexus
sudo systemctl enable nexus
sudo systemctl status nexus  # Verify running status
```

#### **9. Open Firewall Port**
```bash
sudo ufw allow 8081/tcp    # For UFW
# OR if using firewalld:
sudo firewall-cmd --zone=public --add-port=8081/tcp --permanent
sudo firewall-cmd --reload
```

#### **10. Access Nexus**
```bash
echo "Admin password: $(sudo cat /opt/sonatype-work/nexus3/admin.password)"
echo "Access at: http://$(hostname -I | awk '{print $1}'):8081"
```

### **Key Improvements:**
1. **Added Java 17 installation** explicitly as prerequisite
2. **Proper user creation** with secure flags (`--system`, `--disabled-login`)
3. **Clean directory handling** with `cd /opt` at start
4. **Simplified permission commands** without duplicates
5. **Added echo commands** for easy access information
6. **Full verification steps** at each critical point

### **Post-Installation Notes:**
- First login requires password from `/opt/sonatype-work/nexus3/admin.password`
- Recommended to change admin password immediately
- Configure storage and repositories according to your needs

This provides a complete, secure installation with all dependencies properly handled. The Nexus service will auto-start on system boot and run under the dedicated `nexus` user account for security.
