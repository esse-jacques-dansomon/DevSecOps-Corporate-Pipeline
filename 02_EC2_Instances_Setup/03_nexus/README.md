# Nexus

```bash
#!/bin/bash

# Update package manager repositories
sudo apt-get update

# Install necessary dependencies
sudo apt-get install -y ca-certificates curl

# Create directory for Docker GPG key
sudo install -m 0755 -d /etc/apt/keyrings

# Download Docker's GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

# Ensure proper permissions for the key
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add Docker repository to Apt sources
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package manager repositories
sudo apt-get update

sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 

```

Save this script as **install_docker.sh**, then

```shell
sudo chmod +x install_docker.sh
```
Then run it

```bash
./install_docker.sh
```

## Run Nexus container 

### 1. docker compose 

```bash
version: "3"
services:
  nexus:
    image: sonatype/nexus3
    restart: always
    volumes:
      - "nexus-data:/sonatype-work"
    ports:
      - "8081:8081"
      - "8085:8085"
volumes:
  nexus-data: {}
```

### 2. Start the container

```bash
sudo docker compose up -d
```

### 3. Login to nexus and change password

#### Password 

```shell
sudo docker exec -it ubuntu_nexus_1 cat /nexus-data/admin.password
```

#### Login 

```
http://change_to_nexus_publicdns_name:8081
```