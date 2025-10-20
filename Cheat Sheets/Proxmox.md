-----

# Proxmox VE - Cheat Sheet 🚀

## Virtual Machine (VM) Management - `qm`

| Command | Description |
| :--- | :--- |
| `qm list` | Lists all virtual machines and their status. |
| `qm start <VMID>` | Starts the VM with the specified VMID. |
| `qm stop <VMID>` | Stops the VM (hard power off). |
| `qm shutdown <VMID>` | Safely shuts down the guest OS in the VM. |
| `qm reset <VMID>` | Resets the VM. |
| `qm destroy <VMID>` | Destroys the VM. **Warning: this operation is irreversible\!** |
| `qm suspend <VMID>` | Suspends the VM, saving its state to RAM. |
| `qm resume <VMID>` | Resumes a suspended VM. |
| `qm clone <VMID> <NEW_VMID> --name <NAME>` | Clones a VM. |
| `qm config <VMID>` | Displays the VM's configuration. |
| `qm set <VMID> [options]` | Modifies the VM's configuration (e.g., `qm set 100 --memory 2048`). |
| `qm resize <VMID> <DISK> <SIZE>` | Resizes a VM disk (e.g., `qm resize 100 scsi0 +10G`). |
| `qm snapshot <VMID> <SNAPSHOT_NAME>` | Creates a snapshot of the VM. |
| `qm rollback <VMID> <SNAPSHOT_NAME>` | Rolls back the VM to a snapshot's state. |
| `qm terminal <VMID>` | Opens a serial terminal to the VM. |

-----

## Container (LXC) Management - `pct`

| Command | Description |
| :--- | :--- |
| `pct list` | Lists all containers and their status. |
| `pct start <CTID>` | Starts the container with the specified CTID. |
| `pct stop <CTID>` | Stops the container (hard power off). |
| `pct shutdown <CTID>` | Safely shuts down the container. |
| `pct destroy <CTID>` | Destroys the container. **Warning: this operation is irreversible\!** |
| `pct enter <CTID>` | Opens a shell inside a running container. |
| `pct mount <CTID>` | Mounts the container's filesystem. |
| `pct unmount <CTID>` | Unmounts the container's filesystem. |
| `pct config <CTID>` | Displays the container's configuration. |
| `pct set <CTID> [options]` | Modifies the container's configuration (e.g., `pct set 101 --memory 1024`). |
| `pct resize <CTID> <DISK> <SIZE>` | Resizes a container disk (e.g., `pct resize 101 rootfs +5G`). |
| `pct snapshot <CTID> <SNAPSHOT_NAME>` | Creates a snapshot of the container. |
| `pct rollback <CTID> <SNAPSHOT_NAME>` | Rolls back the container to a snapshot's state. |

-----

## Cluster Management - `pvecm`

| Command | Description |
| :--- | :--- |
| `pvecm status` | Displays the cluster status and its members. |
| `pvecm nodes` | Lists the nodes in the cluster. |
| `pvecm create <CLUSTER_NAME>` | Creates a new cluster on the current node. |
| `pvecm add <CLUSTER_MEMBER_IP>` | Adds the current node to an existing cluster. |
| `pvecm delnode <NODE_NAME>` | Removes a node from the cluster. |
| `pvecm expected 1` | Sets the expected quorum votes to 1 (useful when nodes fail). |

-----

## Storage Management - `pvesm`

| Command | Description |
| :--- | :--- |
| `pvesm status` | Displays the status of all configured storages. |
| `pvesm list <STORAGE_ID>` | Lists the content of a specific storage. |
| `pvesm add <TYPE> <STORAGE_ID> [options]` | Adds a new storage (e.g., `pvesm add dir local-backup --path /mnt/backup`). |
| `pvesm remove <STORAGE_ID>` | Removes a storage configuration. |
| `pvesm alloc <STORAGE_ID> <VMID> <FILENAME> <SIZE>` | Allocates a new volume on a storage. |
| `pvesm free <VOLUME_ID>` | Frees/deletes a volume (e.g., `pvesm free local-lvm:vm-100-disk-0`). |

-----

## Backup & Restore - `vzdump` / `qmrestore`

| Command | Description |
| :--- | :--- |
| `vzdump <VMID>` | Creates a backup of a VM or container. |
| `vzdump <VMID> --mode snapshot` | Creates a live (snapshot) backup. |
| `vzdump <VMID> --compress zstd` | Creates a backup with Zstandard compression. |
| `vzdump --all --mailtome` | Backs up all VMs and CTs and sends an email notification. |
| `qmrestore <BACKUP_FILE> <NEW_VMID>` | Restores a VM from a backup file. |
| `pct restore <BACKUP_FILE> <NEW_CTID>` | Restores a container from a backup file. |

-----

## Network Management 🌐

Network configuration in Proxmox is primarily managed by editing the `/etc/network/interfaces` file. After making changes, you must reload the network configuration.

  * **Edit file:** `nano /etc/network/interfaces`
  * **Apply changes:** `ifreload -a`

**Example bridge configuration:**

```bash
auto vmbr0
iface vmbr0 inet static
    address 192.168.1.10/24
    gateway 192.168.1.1
    bridge-ports eno1
    bridge-stp off
    bridge-fd 0
```

-----

## General System Commands

| Command | Description |
| :--- | :--- |
| `pveversion -v` | Displays detailed version info for Proxmox VE and related packages. |
| `pvereport` | Generates a detailed system state and configuration report. |
| `pve-firewall status` | Displays the status of the built-in Proxmox firewall. |
| `pveum` | Manages users and permissions (e.g., `pveum useradd`). |
| `ha-manager status` | Displays the status of the High Availability manager. |
| `apt update && apt dist-upgrade` | Updates the system and Proxmox packages. |
