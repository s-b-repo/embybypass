sudo bash -c "
apt update -y && apt upgrade -y && apt install -y python3-pip git wget &&

rm -rf /home/debian/embybypass &&
git clone https://github.com/s-b-repo/embybypass.git /home/debian/embybypass &&

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
systemctl unmask site.service
systemctl enable site.service
systemctl start site.service
"
