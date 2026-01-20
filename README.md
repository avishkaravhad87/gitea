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
