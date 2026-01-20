# 🚀 Gitea Installation Guide on Ubuntu (With MariaDB)

This guide explains how to install **Gitea** (a lightweight self-hosted Git service) on **Ubuntu 20.04 / 22.04 / 24.04** using the **official binary method** and configure **MariaDB (MySQL)** as the database.

---

## 2️⃣ Install Required Dependencies

```bash
sudo apt update
sudo apt install -y git wget curl
3️⃣ Install & Configure MariaDB (Database)
✅ Install MariaDB

```bash
sudo apt install mariadb-server mariadb-client -y
Enable & start MariaDB service:
sudo systemctl enable mariadb
sudo systemctl start mariadb
sudo systemctl status mariadb
✅ Allow Remote Access (IMPORTANT)

By default MariaDB listens on localhost 127.0.0.1.
To allow Gitea to connect, update bind address.

Open configuration file:
sudo vim /etc/mysql/mariadb.conf.d/50-server.cnf
Find:bind-address            = 127.0.0.1
Change it to:
bind-address            = 0.0.0.0
Restart MariaDB:
sudo systemctl restart mariadb
Verify MariaDB is listening on port 3306:
sudo ss -tulnp | grep 3306
Expected output contains:
0.0.0.0:3306
✅ Allow Firewall Port 3306 (If UFW enabled)
sudo ufw allow 3306/tcp
sudo ufw reload
Login to MariaDB:

sudo mariadb


Run the following SQL commands:

CREATE DATABASE gitea CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE USER 'giteaavi'@'%' IDENTIFIED BY 'Gitea@12345';

GRANT ALL PRIVILEGES ON gitea.* TO 'giteaavi'@'%';

FLUSH PRIVILEGES;
EXIT;


✅ Save these credentials (needed in Gitea web installer):

Database Name: gitea

Username: giteaavi

Password: Gitea@12345

Host: <DB-IP>:3306

4️⃣ Create a Dedicated Gitea User
sudo adduser \
  --system \
  --shell /bin/bash \
  --gecos 'Git Version Control' \
  --group \
  --disabled-password \
  --home /home/git \
  git

5️⃣ Download Gitea Binary

Check latest version:
👉 https://dl.gitea.com/gitea/

Example:

cd /tmp
wget https://dl.gitea.com/gitea/1.22.1/gitea-1.22.1-linux-amd64


Make it executable and move it:

chmod +x gitea-1.22.1-linux-amd64
sudo mv gitea-1.22.1-linux-amd64 /usr/local/bin/gitea


Verify installation:

gitea --version

6️⃣ Create Required Directories
sudo mkdir -p /var/lib/gitea/{custom,data,log}
sudo mkdir -p /etc/gitea


Set permissions:

sudo chown -R git:git /var/lib/gitea /etc/gitea
sudo chmod -R 750 /var/lib/gitea
sudo chmod -R 770 /etc/gitea

7️⃣ Create systemd Service File

Create service file:

sudo vim /etc/systemd/system/gitea.service


Paste the following configuration:

[Unit]
Description=Gitea
After=syslog.target
After=network.target mariadb.service
Requires=mariadb.service

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


Reload systemd:

sudo systemctl daemon-reexec
sudo systemctl daemon-reload

8️⃣ Start and Enable Gitea Service
sudo systemctl enable gitea
sudo systemctl start gitea


Check status:

sudo systemctl status gitea

9️⃣ Access Gitea Web Interface

Open your browser and visit:

http://<server-ip>:3000

✅ Initial Setup Options (MariaDB Recommended)

Fill these values:

Setting	Value
Database Type	MySQL
Host	<DB-IP>:3306
Database Name	gitea
Username	giteaavi
Password	Gitea@12345

Other Settings:

Application URL: http://<server-ip>:3000

Create Admin User

Click Install Gitea to finish setup ✅

🔟 (Optional) Configure Firewall
sudo ufw allow 3000/tcp
sudo ufw reload

🛠️ Troubleshooting
❌ Error: connection refused (Port 3306)

If you get error:

dial tcp <db-ip>:3306: connect: connection refused


Fix:

Check MariaDB service:

sudo systemctl status mariadb


Verify port 3306 listening:

sudo ss -tulnp | grep 3306


Make sure firewall/security-group allows:

3306/tcp

🔥 Optional Enhancements

Gitea using Docker

Nginx Reverse Proxy

SSL (Let's Encrypt)

Gitea + Jenkins Integration

👨‍💻 Author: Avishkar Avhad
📍 Happy Coding & Version Controlling!


---

If you want ✅ I can also add **Nginx reverse proxy + SSL full steps** in the same README.md.


ChatGPT can make mistakes. Check important info. See Cookie Preferences.
