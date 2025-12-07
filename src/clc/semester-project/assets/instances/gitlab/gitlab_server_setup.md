# LaTeX-Pipeline Hub – GitLab Server Setup Documentation

## 1. Create EC2 Instance
- Name: **gitlab**
- OS: **Ubuntu 22.04 LTS**
- Instance type: **t3.medium**
- Disk: **20 GB gp3**
- Private IP: **10.0.0.20**
- Public IPv4 assigned automatically  
- Security Group rules:
  - TCP 22 (SSH) from admin IP only
  - TCP 80 (HTTP) from anywhere
  - TCP 443 (HTTPS) from anywhere
  - TCP/UDP 53 internal for DNS

## 2. Install Docker
Commands:
\`\`\`bash
sudo apt update
sudo apt install -y docker.io docker-compose
sudo systemctl enable docker
sudo systemctl start docker
\`\`\`

## 3. Create persistent GitLab volumes
\`\`\`bash
sudo mkdir -p /srv/gitlab/{config,logs,data}
sudo chmod 777 -R /srv/gitlab
\`\`\`

## 4. Create \`docker-compose.yml\`
\`\`\`yaml
version: '3.7'
services:
  gitlab:
    image: gitlab/gitlab-ce:latest
    hostname: "gitlab.ci.intern"
    restart: always
    container_name: gitlab
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /srv/gitlab/config:/etc/gitlab
      - /srv/gitlab/logs:/var/log/gitlab
      - /srv/gitlab/data:/var/opt/gitlab
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://gitlab.ci.intern'
\`\`\`

## 5. Start GitLab container
\`\`\`bash
sudo docker-compose up -d
\`\`\`

## 6. Add Swap (to avoid out-of-memory)
\`\`\`bash
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
\`\`\`

## 7. Obtain initial root password
\`\`\`bash
sudo cat /srv/gitlab/config/initial_root_password
\`\`\`

## 8. Login to GitLab
- Browser:
\`\`\`
http://<public-ip>
\`\`\`
- Username:
\`\`\`
root
\`\`\`
- Password: initial password from container file  
- GitLab prompts to set a new admin password

## 9. Verification
Browser access:
\`\`\`
http://gitlab.ci.intern
\`\`\`
or public IPv4

Instance reachable and GitLab UI working.  

## Result
GitLab CE is successfully running inside a Docker container on the EC2 instance.  
UI is accessible using both the internal DNS name and public IP.  
Root login completed.  

GitLab Server part of Milestone-2 is fulfilled.
