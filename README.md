#update the system using commands
sudo apt update -y
#install required packages
sudo apt install -y git wget curl
#creat a dedicated gitea user
sudo adduser \
   --system \
   --shell /bin/bash \
   --gecos 'Git Version Control' \
   --group \
   --disabled-password \
   --home /home/git \
   git
#download gitea binary
cd /tmp
wget https://dl.gitea.com/gitea/1.22.1/gitea-1.22.1-linux-amd64
#make it executable and move it
chmod +x gitea-1.22.1-linux-amd64
sudo mv gitea-1.22.1-linux-amd64 /usr/local/bin/gitea
#verify
gitea --version
#create required directories
sudo mkdir -p /var/lib/gitea/{custom,data,log}
sudo mkdir -p /etc/gitea
#set permissions 
sudo chown -R git:git /var/lib/gitea /etc/gitea
sudo chmod -R 750 /var/lib/gitea
sudo chmod -R 770 /etc/gitea
#create service file 
sudo vim /etc/systemd/system/gitea.service

#add this in file 
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

#reload systemd
sudo systemctl daemon-reexec
sudo systemctl daemon-reload

#start and enable gitea
sudo systemctl enable gitea
sudo systemctl start gitea

#check status
sudo systemctl status gitea


