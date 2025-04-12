
# 🧱 Proxmox Host Setup

This guide documents the initial setup of your Proxmox server, storage configuration, and VM hosting environment.

---

## 🖥️ Hardware Used

- **CPU**: Intel Core i5-9500 @ 3.00GHz
- **RAM**: 64GB DDR4
- **Drives**:
  - 119GB NVMe (Proxmox OS + ISO storage)
  - 1TB SATA SSD (data, VMs, backups)

---

## 🧩 Proxmox Installation Summary

1. Downloaded Proxmox VE ISO from [proxmox.com](https://www.proxmox.com/en/)
2. Created a bootable USB and installed Proxmox on `/dev/nvme0n1`
3. After install:
```bash
apt update && apt upgrade -y
```

---

## 🗃️ Disk Partitioning & Mounting

### NVMe ISO Storage Setup

Partition: `/dev/nvme0n1p4`

```bash
mkfs.ext4 /dev/nvme0n1p4
mkdir -p /mnt/nvme-iso
mount /dev/nvme0n1p4 /mnt/nvme-iso
```

Added to Proxmox as ISO storage:

```bash
pvesm add dir ISO --path /mnt/nvme-iso --content iso --nodes home
```

---

### 1TB SSD Partition Setup

- `/dev/sda1` → `/mnt/vm-disks`
- `/dev/sda2` → `/mnt/backups`
- `/dev/sda3` → `/mnt/storage`

Batch format and mount:

```bash
for i in /dev/sda1:/mnt/vm-disks /dev/sda2:/mnt/backups /dev/sda3:/mnt/storage; do
  disk="${i%%:*}"; mountpoint="${i##*:}";
  mkfs.ext4 -F "$disk" && mkdir -p "$mountpoint" && mount "$disk" "$mountpoint";
done
```

---

## ✅ Result

- Proxmox is installed and updated
- Custom storage added for ISOs, VMs, backups, and general files
- Ready to deploy virtual machines
