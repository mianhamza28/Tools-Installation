
```bash
#!/bin/bash

# Install Docker using the official Docker repository (recommended)
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

# Install Docker Compose v2 (as Docker plugin)
sudo apt-get install -y docker-compose-plugin

# Verify installations
docker --version
docker compose version

# Start and enable Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Manage Docker group
if ! getent group docker >/dev/null; then
    sudo groupadd docker
fi

# Add current user to Docker group
sudo usermod -aG docker $USER

# Add ubuntu user if different from $USER
[ "$USER" != "ubuntu" ] && sudo usermod -aG docker ubuntu

# Verify Docker access (might still require new session)
echo "Docker permissions test:"
docker run hello-world || echo "If this failed, log out and back in or restart your session"

# Important Security Note
echo -e "\n\033[1;33mWARNING: Users in the 'docker' group have root-equivalent privileges."
echo -e "Only trusted users should be added to this group.\033[0m"
```
