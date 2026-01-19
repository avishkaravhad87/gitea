# 🚀 Gitea Installation Guide on Ubuntu

This guide explains how to install **Gitea** (a lightweight self-hosted Git service) on **Ubuntu 20.04 / 22.04 / 24.04** using the **official binary method**.

---

## 📌 Prerequisites

* Ubuntu server or VM
* sudo privileges
* Internet access

---

## 1️⃣ Update System Packages

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 2️⃣ Install Required Dependencies

```bash
sudo apt install -y git wget curl
```

---

## 3️⃣ Create a Dedicated Gitea User

```bash
sudo adduser \
  --system \
  --shell /bin/bash \
  --gecos 'Git Version Control' \
  --group \
  --disabled-password \
  --home /home/git \
  git
```

---

## 4️⃣ Download Gitea Binary

Check the latest version: [https://dl.gitea.com/gitea/](https://dl.gitea.com/gitea/)

Example:

```bash
cd /tmp
wget https://dl.gitea.com/gitea/1.22.1/gitea-1.22.1-linux-amd64
```

Make it executable and move it:

```bash
chmod +x gitea-1.22.1-linux-amd64
sudo mv gitea-1.22.1-linux-amd64 /usr/local/bin/gitea
```

Verify installation:

```bash
gitea --version
```

---

## 5️⃣ Create Required Directories

```bash
sudo mkdir -p /var/lib/gitea/{custom,data,log}
sudo mkdir -p /etc/gitea
```

Set permissions:

```bash
sudo chown -R git:git /var/lib/gitea /etc/gitea
sudo chmod -R 750 /var/lib/gitea
sudo chmod -R 770 /etc/gitea
```

---

## 6️⃣ Create systemd Service File

```bash
sudo nano /etc/systemd/system/gitea.service
```

Paste the following configuration:

```ini
[Unit]
Description=Gitea
After=syslog.target
After=network.target

[Service]
RestartSec=2s
Type=simple
User=git
Group=git
WorkingDirectory=/var/lib/gitea/
ExecStart=/usr/local/bin/gitea web \
  --config /etc/gitea/app.ini
Restart=always
Environment=USER=git HOME=/home/git GITEA_WORK_DIR=/var/lib/gitea

[Install]
WantedBy=multi-user.target
```

Reload systemd:

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
```

---

## 7️⃣ Start and Enable Gitea Service

```bash
sudo systemctl enable gitea
sudo systemctl start gitea
```

Check status:

```bash
sudo systemctl status gitea
```

---

## 8️⃣ Access Gitea Web Interface

Open your browser and visit:

```
http://<server-ip>:3000
```

### Initial Setup Options

* Database: **SQLite3** (recommended for beginners)
* Application URL: `http://<server-ip>:3000`
* Create Admin User

Click **Install Gitea** to finish setup.

---

## 9️⃣ (Optional) Configure Firewall

```bash
sudo ufw allow 3000
sudo ufw reload
```

---

## 🔟 (Optional) Run Gitea Behind Nginx

Recommended for production environments:

* Reverse proxy on port 80/443
* SSL using Let's Encrypt

---

## ✅ Summary

* ✔ Lightweight GitHub alternative
* ✔ Easy installation
* ✔ Perfect for DevOps practice and CI/CD labs

---

## 📦 Optional Enhancements

* Gitea with **MySQL / PostgreSQL**
* Gitea using **Docker**
* Gitea + **Jenkins Integration**
* **Nginx + SSL** Production Setup

---

👨‍💻 **Author:** Avishkar Avhad

📍 Happy Coding & Version Controlling!
