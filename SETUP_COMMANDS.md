# wazuh-homelab-siem

## Setup Summary

### 1. Install WSL (Windows 11)

```powershell
wsl --install -d Ubuntu
```

---

### 2. Install required tools in WSL

```bash
sudo apt update
sudo apt install -y ansible git openssh-client
```

Verification:

```bash
ansible --version
git --version
ssh -V
```

---

### 3. Network connectivity test

```bash
ping -c 2 192.168.160.128
ping -c 2 192.168.160.129
```

---

### 4. Enable SSH on both Ubuntu VMs

```bash
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
```

---

### 5. Generate SSH key (WSL)

```bash
ssh-keygen -t ed25519 -C "ansible-wazuh"
```

---

### 6. Copy SSH key

```bash
ssh-copy-id user@192.168.160.129
ssh-copy-id ubuntu@192.168.160.128
```

---

### 7. Inventory configuration

```ini
[wazuh_server]
192.168.160.129 ansible_user=user

[wazuh_clients]
192.168.160.128 ansible_user=ubuntu
```

---

### 8. Test Ansible connectivity

```bash
ansible all -i inventory.ini -m ping
```

Result:

```
192.168.160.128 → SUCCESS (pong)
192.168.160.129 → SUCCESS (pong)
```

---

## Current Status

- WSL installed
- Ansible configured
- SSH key authentication working
- Both nodes reachable via Ansible