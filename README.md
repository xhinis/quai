Here's a complete guide to update and set up `go-quai` v0.34.1 and `go-quai-stratum` v0.15.0 on Ubuntu 20.04. This guide ensures that you transition smoothly to the latest versions, addressing the `getPendingHeader` issues when running the stratum proxy.

---

## Quai Network Node Update and Setup Guide

### Overview

This guide covers:
- Stopping the current node and processes.
- Installing and updating dependencies.
- Downloading and building the latest releases of `go-quai` and `go-quai-stratum`.
- Configuring and starting the node and stratum proxy.

### Prerequisites

- Ubuntu 20.04
- A Linux system with minimum hardware requirements (for slice node):
  - 8+ cores CPU
  - 24 GB RAM
  - SSD with at least 1 TB free space
  - 10 MBit/sec download speed
- A pre-existing installation of `go-quai` and `go-quai-stratum`.

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

## Step 3: Update and Build `go-quai`

1. **Navigate to the `go-quai` directory**:
   ```bash
   cd go-quai
   ```

2. **Fetch the latest release**:
   ```bash
   git fetch --all
   git checkout v0.34.1
   git pull origin v0.34.1
   ```

3. **Build `go-quai`**:
   ```bash
   make go-quai
   ```

---

## Step 4: Update and Build `go-quai-stratum`

1. **Navigate to the `go-quai-stratum` directory**:
   ```bash
   cd go-quai-stratum
   ```

2. **Fetch the latest release**:
   ```bash
   git fetch --all
   git checkout v0.15.0
   git pull origin v0.15.0
   ```

3. **Build `go-quai-stratum`**:
   ```bash
   make go-quai-stratum
   ```

---

## Step 5: Configure the Node and Stratum Proxy

### 1. Node Configuration

- **Select Node Type**: Ensure you're running a slice node (recommended for most users).
- **Set environment variables**: You need to configure slices and coinbase addresses for the node.

- Example command for a single slice node running `Cyprus1`:
  ```bash
  ./build/bin/go-quai start --node.slices '[0 0]' --node.coinbases '0xYourQuaiAddress,0xYourQiAddress'
  ```
  Replace:
  - `0xYourQuaiAddress` with your Quai address.
  - `0xYourQiAddress` with your Qi address.

### 2. Stratum Proxy Configuration

- Ensure that your configuration file (`config.json`) for `go-quai-stratum` is correctly set up with the necessary node and network details.

---

## Step 6: Start the Node and Stratum Proxy

1. **Start the node**:
   ```bash
   ./build/bin/go-quai start --node.slices '[0 0]' --node.coinbases '0xYourQuaiAddress,0xYourQiAddress'
   ```

   - Check if logs start printing to confirm that the node is running.

2. **Start the stratum proxy**:
   ```bash
   ./build/bin/go-quai-stratum --config /path/to/your/config.json
   ```

   - Ensure the config file has the correct paths and node addresses.

---

## Step 7: Verify the Update

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

This guide should help you smoothly transition to the latest versions of `go-quai` and `go-quai-stratum` on Ubuntu 20.04. Let me know if you need any further assistance!
