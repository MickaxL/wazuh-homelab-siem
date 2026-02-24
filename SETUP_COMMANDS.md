# wazuh-homelab-siem

# wazuh-homelab-siem — Full Setup Commands Log

This file contains ALL commands used to setup the project from scratch.

============================================================
1️⃣ WINDOWS 11 — INSTALL WSL
============================================================

PowerShell (Admin):

wsl --install -d Ubuntu


============================================================
2️⃣ WSL — INSTALL TOOLS
============================================================

sudo apt update
sudo apt install -y ansible git openssh-client


Verification:

ansible --version
git --version
ssh -V


============================================================
3️⃣ NETWORK TEST FROM WSL
============================================================

ping -c 2 192.168.160.128
ping -c 2 192.168.160.129


============================================================
4️⃣ ON BOTH UBUNTU VMs (CLIENT + SERVER)
============================================================

sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh


============================================================
5️⃣ GENERATE SSH KEY (WSL)
============================================================

ssh-keygen -t ed25519 -C "ansible-wazuh"

# Save path default:
/home/user/.ssh/id_ed25519

# No passphrase for homelab


============================================================
6️⃣ COPY SSH KEY TO SERVER
============================================================

ssh-copy-id user@192.168.160.129


============================================================
7️⃣ FIX CLIENT SSH ISSUE (192.168.160.128)
============================================================

# On client VM:

sudo nano /etc/ssh/sshd_config

# Set:
PasswordAuthentication yes

sudo systemctl restart ssh

# Change password
passwd


# From WSL:
ssh-copy-id ubuntu@192.168.160.128


============================================================
8️⃣ TEST SSH ACCESS
============================================================

ssh user@192.168.160.129
ssh ubuntu@192.168.160.128


============================================================
9️⃣ INVENTORY FILE
============================================================

Create inventory.ini:

[wazuh_server]
192.168.160.129 ansible_user=user

[wazuh_clients]
192.168.160.128 ansible_user=ubuntu

[all:vars]
ansible_ssh_private_key_file=~/.ssh/id_ed25519


============================================================
🔟 TEST ANSIBLE CONNECTIVITY
============================================================

ansible all -i inventory.ini -m ping

result ; 
Result:

```text
192.168.160.128 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

192.168.160.129 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
============================================================
CURRENT STATUS
============================================================

✔ WSL installed
✔ Ansible installed
✔ SSH working
✔ Inventory configured
✔ Ansible ping OK on both machines
