
# 🐧 Ubuntu Server VM Deployment

This guide outlines the creation and configuration of an Ubuntu Server VM inside your Proxmox host.

---

## 📦 ISO Upload

Uploaded `ubuntu-24.04.2-live-server-amd64.iso` to ISO storage located at `/mnt/nvme-iso`.

---

## 🔧 VM Creation (via Proxmox Shell)

```bash
qm create 100 --name ubuntu-server --memory 4096 --cores 2 --net0 virtio,bridge=vmbr0 --cdrom ISO:iso/ubuntu-24.04.2-live-server-amd64.iso --scsihw virtio-scsi-pci --sata0 vm-disks:32 --ostype l26 --agent enabled=1 --boot order=ide2;sata0 --bootdisk sata0 --ide2 ISO:cloudinit --serial0 socket --vga serial0 --machine q35
```

- Used `sata0` due to initial install issue with `scsi0`
- Enabled QEMU Guest Agent
- Set ISO as primary boot temporarily

---

## 🛠 Installation Notes

- Ubuntu Server full version was chosen (not minimized)
- Hostname set to: `aime-hub`
- User created: `ogun`
- OpenSSH Server enabled during install

---

## 🧰 Post-Install Commands

After logging in:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install qemu-guest-agent openssh-server -y
sudo systemctl enable qemu-guest-agent
sudo systemctl start qemu-guest-agent
```

---

## ✅ Result

- Fully functional Ubuntu Server VM (ID: 100)
- Accessible via SSH
- Integrated with Proxmox Guest Agent
- Ready for Docker, Portainer, Home Assistant, and AI stack
