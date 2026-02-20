# wazuh-homelab-siem

history of set up : 
    MainPC : 
        - Win11 
            - WSL 
            - VsCode

    2 VM :
        - Ubuntu Desktop 
        - Ubuntu Server


on Win11 : 

    powershell : wsl --install -d Ubuntu

WSL : 

    sudo apt update
    sudo apt install -y ansible git openssh-client


Verifcation : 
    ansible --version
    git --version
    ssh -V

