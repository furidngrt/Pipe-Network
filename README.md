<p align="center">
  <img src="https://github.com/user-attachments/assets/9706d8ed-b323-43ce-bddf-2384511ab605" alt="unnamed" width="500">
</p>


# Pipe Network Testnet Setup Guide

Prepare for the incentivized testnet! Rewards will be given to participants who demonstrate strong performance. Both **PoP Nodes** and **Guardian Nodes** will play critical roles in shaping the future of the Pipe Network.

---

## Steps to Set Up the Node

### 1. Create Directory
```bash
sudo mkdir -p /opt/dcdn
```

### 2. Download Pipe Tool Binary
Replace `PIPE-URL` with the URL/link provided in your email.

```bash
sudo curl -L "https://example-link.com/pipe-tool" -o /opt/dcdn/pipe-tool
```

### 3. Download Node Binary
Replace `DCDND-URL` with the URL/link provided in your email.

```bash
sudo curl -L "https://example-link.com/dcdnd" -o /opt/dcdn/dcdnd
```

### 4. Make Binaries Executable
```bash
sudo chmod +x /opt/dcdn/pipe-tool
sudo chmod +x /opt/dcdn/dcdnd
```

### 5. Log In to Generate Access Token
Run the command below. It will generate a QR code and code. Scan the QR code or visit the link to submit the code and sign up with your email.

```bash
/opt/dcdn/pipe-tool login --node-registry-url="https://rpc.pipedev.network"
```

### 6. Generate Registration Token
```bash
/opt/dcdn/pipe-tool generate-registration-token --node-registry-url="https://rpc.pipedev.network"
```

### 7. Create the Service File
```
# Create service file using cat
sudo cat > /etc/systemd/system/dcdnd.service << 'EOF'
[Unit]
Description=DCDN Node Service
After=network.target
Wants=network-online.target

[Service]
# Path to the executable and its arguments
ExecStart=/opt/dcdn/dcdnd \
                --grpc-server-url=0.0.0.0:8002 \
                --http-server-url=0.0.0.0:8003 \
                --node-registry-url="https://rpc.pipedev.network" \
                --cache-max-capacity-mb=1024 \
                --credentials-dir=/root/.permissionless \
                --allow-origin=*

# Restart policy
Restart=always
RestartSec=5

# Resource and file descriptor limits
LimitNOFILE=65536
LimitNPROC=4096

# Logging
StandardOutput=journal
StandardError=journal
SyslogIdentifier=dcdn-node


# Working directory
WorkingDirectory=/opt/dcdn

[Install]
WantedBy=multi-user.target
EOF
```

### 8. Open Ports
```bash
sudo ufw allow 8002/tcp
sudo ufw allow 8003/tcp
```

### 9. Start the Node
```bash
sudo systemctl daemon-reload
sudo systemctl enable dcdnd
sudo systemctl start dcdnd
```

### 10. Verify Node Status
Check if your node is active and running:
```bash
/opt/dcdn/pipe-tool list-nodes --node-registry-url="https://rpc.pipedev.network"
```

### 11. Generate and Register Wallet
Generate your wallet and save the wallet phrase securely:
```bash
/opt/dcdn/pipe-tool generate-wallet --node-registry-url="https://rpc.pipedev.network"
```

Link your wallet to the node:
```bash
/opt/dcdn/pipe-tool link-wallet --node-registry-url="https://rpc.pipedev.network"
```

---

By following these steps, your node will be ready for the incentivized testnet. Best of luck! 🚀
