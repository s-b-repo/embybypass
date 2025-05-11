sudo bash -c "
apt update -y && apt upgrade -y && apt install python3-pip git wget -y &&

cd /home/debian && git clone https://github.com/s-b-repo/embybypass.git &&

cat > /etc/systemd/system/site.service << 'EOF'
[Unit]
Description=site Service
After=network.target

[Service]
ExecStart=/usr/bin/python3 /home/debian/embybypass/app.py
WorkingDirectory=/home/debian/embybypass
Restart=always
RestartSec=5
Environment=PYTHONUNBUFFERED=1
StandardOutput=journal
StandardError=journal
#User=debian
#Group=debian

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reexec
systemctl daemon-reload
systemctl enable site.service
systemctl start site.service
"
