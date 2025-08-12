# IBVM Node Setup & Management Guide

This guide explains how to set up and manage IBVM nodes in different modes: **Archive Mode** and **Full Node Mode**.

---

## 1. Starting Nodes

### **Archive Node** (Keeps full chain history, all states & internal transactions)
```bash
geth --datadir /path/to/archive-node --syncmode=full --gcmode=archive --http --http.addr 0.0.0.0 --http.port 8545 --http.api eth,net,web3,debug,txpool --ws --ws.addr 0.0.0.0 --ws.port 8546 --ws.api eth,net,web3,debug,txpool --cache 4096
````

---

### **Full Node** (Default Mode)

```bash
geth --datadir /path/to/full-node --syncmode=full --http --http.addr 0.0.0.0 --http.port 8545 --http.api eth,net,web3,debug,txpool --ws --ws.addr 0.0.0.0 --ws.port 8546 --ws.api eth,net,web3,debug,txpool --cache 4096
```

---

## 2. Deleting Internal Transactions (For Archive Node)

Internal transactions are stored in the **transaction tracing data** of the archive node.
To remove them **without affecting the blockchain state**, use:

```bash
geth removedb --datadir /path/to/archive-node --internal
```

⚠️ **Note**: This will only delete internal transaction history. The chain state will remain intact.

---

## 3. Adding External Storage to Geth

If your server is running out of space, you can attach an **external storage drive** for Geth data.

### **Steps:**

1. **Attach the drive** to your server.
2. **Format** the drive (Example for ext4):

   ```bash
   sudo mkfs.ext4 /dev/sdb
   ```
3. **Create a mount point**:

   ```bash
   sudo mkdir /mnt/gethdata
   ```
4. **Mount the drive**:

   ```bash
   sudo mount /dev/sdb /mnt/gethdata
   ```
5. **Move existing Geth data**:

   ```bash
   sudo systemctl stop geth
   mv /path/to/archive-node/* /mnt/gethdata/
   ```
6. **Update Geth command**:

   ```bash
   geth --datadir /mnt/gethdata ...
   ```

---

## 4. Space Estimation

If you run:

* **1 Archive Node** (`--gcmode=archive`)
* **2 Full Nodes** (Default mode)
* **Block time**: 2 seconds
* **Total Transactions in 3 Months**: \~200,000

**Estimated Storage Usage**:

| Node Type    | 3 Months Storage |
| ------------ | ---------------- |
| Archive Node | \~100-120 GB     |
| Full Node    | \~30-40 GB each  |

---

## 5. Removing Internal Transactions Every 6 Months

You can periodically clean internal transactions from the archive node:

```bash
geth removedb --datadir /mnt/gethdata --internal
```

It’s recommended to **backup** your node data before running the command.

---

## 6. Notes

* Replace `/path/to/archive-node` and `/path/to/full-node` with your actual Geth data directories.
* Keep your **`--cache`** value high for better performance if your server has enough RAM.
* Use **screen** or **tmux** when running nodes so they stay active after you close the terminal.

---

## License

This guide is open-source. Feel free to modify and use it for your IBVM node operations.

```

---

If you want, I can also add **diagrams showing archive vs full node data flow** so your README looks more professional and GitHub-friendly. That would make it easier for contributors to understand the architecture visually.
```
