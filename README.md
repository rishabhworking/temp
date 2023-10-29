[![N|Solid](https://explorer.ariettachain.tech/images/archheader-5f0aa4456684da4cd144733f729a4e5b.png)](#)

# Arietta Smart Chain Node Setup

This guide will help you set up an Arietta Smart Chain node on your server.

## Prerequisites

Before you begin, ensure you have the following:

- A Linux server (e.g., Ubuntu)
- Root or sudo access

## Step 1: Download and Install Polygon Edge

First, download the Polygon Edge binary and make it executable.

```
wget https://github.com/Ariettachain/ASC/releases/download/v1.0/polygon-edge
```
```
cp polygon-edge /usr/local/bin/polygon-edge
```
```
chmod +x /usr/local/bin/polygon-edge
```
## Step 2: Create a System User

Create a system user for running the Arietta Smart Chain node.
```
useradd -m -s /usr/sbin/nologin -d /var/lib/ascnetwork ascnetwork
```


## Step 3: Step 3: Create Data Directories and Set Permissions
Create data directories and set permissions for the new user.
```
mkdir -p /data/ascnetwork
```
```
mkdir -p /etc/ascnetwork
```
```
chown -R ascnetwork:ascnetwork /data/ascnetwork
```
```
chown -R ascnetwork:ascnetwork /etc/ascnetwork
```


## Step 4: Step 4: Install Zip and Unzip
Install the zip and unzip utilities.
```
sudo apt update
```
```
sudo apt install zip unzip -y
```

## Step 5: Download Data Directory Archive
Download the data directory archive and extract it into the data directory.
```
wget -P /tmp https://backup.ariettachain.tech/downloads/ASC_Snapshot.zip
```
```
unzip /tmp/ASC_Snapshot.zip -d /data/ascnetwork
```
```
chown -R ascnetwork:ascnetwork /data/ascnetwork
```
```
rm -f /tmp/ASC_Snapshot.zip
```

## Step 6: Download Genesis File and Create Configuration
Download the Genesis file and create the configuration file for the node.
```
wget -P /etc/ascnetwork https://github.com/Ariettachain/ASC/releases/download/v1.0/arietta-mainnet.json
```
Create the configuration file /etc/ascnetwork/ascnetwork_conf.json:
```
nano /etc/ascnetwork/ascnetwork_conf.json

```
```
{
    "chain_config": "/etc/ascnetwork/arietta-mainnet.json",
    "secrets_config": "",
    "data_dir": "/data/ascnetwork",
    "block_gas_target": "0x0",
    "grpc_addr": "127.0.0.1:11738",
    "jsonrpc_addr": "127.0.0.1:16367",
    "telemetry": {
        "prometheus_addr": ""
    },
    "network": {
        "no_discover": false,
        "libp2p_addr": "127.0.0.1:15710",
        "nat_addr": "",
        "dns_addr": "",
        "max_peers": 64,
        "max_outbound_peers": 16,
        "max_inbound_peers": 128
    },
    "seal": true,
    "tx_pool": {
        "price_limit": 3000000000,
        "max_slots": 4096,
        "max_account_enqueued": 128
    },
    "log_level": "INFO",
    "restore_file": "",
    "headers": {
        "access_control_allow_origins": [
            "localhost"
        ]
    },
    "log_to": "/data/ascnetwork/ascnetwork.log",
    "json_rpc_batch_request_limit": 20,
    "json_rpc_block_range_limit": 1000,
    "json_log_format": true
}

```
## Step 7: Create a systemd Service
Create a systemd service for the Arietta Smart Chain node.
```
nano /etc/systemd/system/ascnetwork.service
```
```
[Unit]
Description=ascnetwork
After=network.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Install]
WantedBy=multi-user.target

[Service]
User=ascnetwork
Group=ascnetwork
Restart=on-failure
RestartSec=5s
Type=simple
KillSignal=SIGINT
TimeoutStopSec=120
LimitNOFILE=65535
LimitNPROC=65535
PrivateTmp=true
MemoryMax=6G
MemoryHigh=5G
MemoryLimit=6G

WorkingDirectory=/var/lib/ascnetwork
ExecStart=/usr/local/bin/polygon-edge server --config /etc/ascnetwork/ascnetwork_conf.json
```

## Step 8: Start the Service
Start the Arietta Smart Chain node service, enable it, and check its status.
```
systemctl daemon-reload
```
```
systemctl enable ascnetwork
```
```
systemctl start ascnetwork
```
```
systemctl status ascnetwork
```
(Additional restart for generating "Network and Consensus Keys" as we used Quick Sync)
```
systemctl restart ascnetwork
```

## Step 9: Output Secrets
If you are setting up a Validator Node, then output the secrets by running the following command.
```
polygon-edge secrets output --data-dir /data/ascnetwork/
```

## Step 10:  Enjoy using Arietta Smart Chain.
