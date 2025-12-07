# LaTeX-Pipeline Hub – GitLab Runner Setup Documentation

## 1. Create EC2 Instance (Runner)
- Name: **runner**
- OS: **Ubuntu 22.04 LTS**
- Instance type: **t3.micro**
- Private IP: **10.0.0.21**
- Auto-assign Public IP: enabled temporarily for SSH access
- Same VPC and subnet as GitLab
- Security Group:
  - TCP 22 (SSH) from admin IP
  - Outbound allowed (default)

---

## 2. Configure DNS (Runner VM)
Disable systemd-resolved and configure DNS manually:

```bash
sudo systemctl disable --now systemd-resolved
sudo rm /etc/resolv.conf
echo "nameserver 10.0.0.10" | sudo tee /etc/resolv.conf
echo "nameserver 10.0.0.11" | sudo tee -a /etc/resolv.conf
```

Verify DNS resolution:
```bash
dig gitlab.ci.intern
```

Expected:
```
gitlab.ci.intern. IN A 10.0.0.20
```

---

## 3. Install Docker
```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker
```

---

## 4. Prepare configuration directory
```bash
sudo mkdir -p /srv/gitlab-runner/config
sudo chmod 777 -R /srv/gitlab-runner
```

---

## 5. Run GitLab Runner container (with correct DNS)
```bash
sudo docker run -d --name gitlab-runner \
  --restart always \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  --dns 10.0.0.10 \
  --dns 10.0.0.11 \
  gitlab/gitlab-runner:latest
```

---

## 6. Register runner
```bash
sudo docker exec -it gitlab-runner gitlab-runner register
```

Prompts:
```
URL: http://gitlab.ci.intern
Token: <registration token from GitLab>
Description: runner1
Tags: docker
Executor: docker
Default Docker image: alpine:latest
```

---

## 7. Verification
Open GitLab:
```
http://<gitlab-public-ip>/admin/runners
```
or via internal:
```
http://gitlab.ci.intern/admin/runners
```

Expected:
- runner1 visible
- green status (“online”)

---

## Result
A working GitLab Runner running inside a Docker container was successfully deployed and registered on GitLab using the internal DNS name. This fulfills the GitLab Runner requirement of Milestone 2.
