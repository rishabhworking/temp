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

mkdir -p /etc/ascnetwork

chown -R ascnetwork:ascnetwork /data/ascnetwork

chown -R ascnetwork:ascnetwork /etc/ascnetwork
```


## Step 4: 

## Step 5: 

## Step 6: 

## Step 7: 

## Step 8: 

## Step 9: 

