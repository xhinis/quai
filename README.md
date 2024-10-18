

## Quai Network Node Update and Setup Guide

### Overview

This guide covers:
- Stopping the current node and processes.
- Installing and updating dependencies.
- Cloning, downloading, and building the latest releases of `go-quai` and `go-quai-stratum`.
- Configuring and starting the node and stratum proxy.

### Prerequisites

- Ubuntu 20.04, 22.04 LTS
- A Linux system with minimum hardware requirements (for slice node):
  - 8+ cores CPU
  - 24 GB RAM
  - SSD with at least 1 TB free space
  - 10 MBit/sec download speed
- A pre-existing installation of `go-quai` and `go-quai-stratum` or a fresh setup.

---

## Step 1: Stop the Current Node and Stratum Proxy

1. **Stop the running node**:
   Press `CTRL+C` in the terminal where the node is running.

2. **Check for running processes** (especially mining or stratum proxy):
   ```bash
   ps aux | grep go-quai
   ```

3. **Terminate any running processes**:
   ```bash
   kill <process_id>
   ```
   Replace `<process_id>` with the actual process ID of the `go-quai` or `go-quai-stratum` processes.

---

## Step 2: Install Required Dependencies

1. **Update the system**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install Snap** (if not already installed):
   ```bash
   sudo apt install snapd -y
   ```

3. **Install Go using Snap**:
   ```bash
   sudo snap install go --classic
   ```

4. **Install Git, Make, and GCC**:
   ```bash
   sudo apt install git make g++ -y
   ```

---

## Step 3: Clone, Update, and Build `go-quai`

1. **Clone the repository**:
   ```bash
   git clone https://github.com/dominant-strategies/go-quai
   cd go-quai
   ```

2. **Fetch the latest release**:
   ```bash
   git fetch --all
   git checkout v0.36.0
   git pull origin v0.36.0
   ```

3. **Build `go-quai`**:
   ```bash
   make go-quai
   ```
4. **Build `go-quai`**:
  
  ```bash
  ./build/bin/go-quai start --node.slices '[0 0]' --node.coinbases '0xYourQuaiAddress,0xYourQiAddress'
  ```
  Replace:
  - `0xYourQuaiAddress` with your Quai address.
  - `0xYourQiAddress` with your Qi address.


---

## Step 4: Clone, Update, and Build `go-quai-stratum`

1. **Clone the repository**:
   ```bash
   cd
   git clone https://github.com/dominant-strategies/go-quai-stratum
   cd go-quai-stratum
   ```

2. **Fetch the latest release**:
   ```bash
   git fetch --all
   git checkout v0.15.0
   git pull origin v0.15.0
   ```
3.  **Configuration**:
   To run the Quai stratum proxy, you’ll need to do some minor configuration. Start by copying the example configuration file to a local configuration file:
   ```bash
   cp config/config.example.json config/config.json
   ```

4. **Build `go-quai-stratum`**:
   ```bash
   make go-quai-stratum
   ```
5. **Start`**:
Start when node is synced. Check from here: https://stats.quai.network/

 ```bash
   ./build/bin/go-quai-stratum --region=cyprus --zone=cyprus1
   ```
---



## Step 5: Verify the Update

1. **Check Node Logs**:
   ```bash
   tail -f /var/log/go-quai.log
   ```
   - Monitor logs to confirm that the node is running correctly and that the `getPendingHeader` issue is resolved.

2. **Check Stratum Proxy Logs**:
   ```bash
   tail -f /var/log/go-quai-stratum.log
   ```
   - Confirm that the proxy is communicating with the node without errors.

---

## Additional Commands and Operations

- **To check sync progress**:
   ```bash
   ./build/bin/go-quai --sync-status
   ```

- **To stop the node**: Use `CTRL+C` in the terminal or locate the process ID and kill it:
   ```bash
   ps aux | grep go-quai
   kill <process_id>
   ```

- **Node Monitoring**: Set up compatible monitoring tools for detailed insights (e.g., Grafana).

---

### Notes
- Always back up your configuration files and addresses before upgrading.
- It’s a good practice to re-run the build command after pulling updates or making configuration changes.
