
```bash
# Update system
sudo apt update -y

# Install Java 17 (Jenkins LTS requirement)
sudo apt install -y fontconfig openjdk-17-jre

# Verify Java installation
java -version

# Add Jenkins repository key
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

# Add Jenkins repository
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

# Install Jenkins
sudo apt update -y
sudo apt install -y jenkins

# Start and enable Jenkins
sudo systemctl enable --now jenkins
sudo systemctl status jenkins

# Configure firewall (using firewalld)
sudo apt install -y firewalld
sudo systemctl enable --now firewalld

# Define firewall rules
YOURPORT=8080
sudo firewall-cmd --permanent --new-service=jenkins
sudo firewall-cmd --permanent --service=jenkins --set-short="Jenkins Service Ports"
sudo firewall-cmd --permanent --service=jenkins --set-description="Jenkins service port exceptions"
sudo firewall-cmd --permanent --service=jenkins --add-port=$YOURPORT/tcp
sudo firewall-cmd --permanent --add-service=jenkins
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload

# Verify firewall configuration
sudo firewall-cmd --list-all

# Get initial admin password
echo "Jenkins initial admin password:"
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

# Verify web access
curl -v http://localhost:8080
```



**Alternative Simplified Firewall (UFW)**:
```bash
sudo ufw allow 8080/tcp
sudo ufw allow http
sudo ufw reload
```
