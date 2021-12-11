---
title: New Kali linux machine setup
has_children: true
parent: Setups
grand_parent: Kali Linux
---

# Fresh install
## Install Impacket
apt install python3-pip -y 
cd /opt
git clone https://github.com/SecureAuthCorp/impacket  
cd impacket 
pip3 install .


## Enable RDP and SSH (optional)

sudo systemctl enable ssh 
sudo systemctl start ssh
apt-get install xrdp -y 
systemctl enable xrdp --now 
systemctl enable xrdp-sesman --now 

nano /etc/xrdp/startwm.sh 

___add the lines below before "test and execute Xsession"___
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
. $HOME/.profile

## Run Pimpmykali (optional)
cd /opt
git clone https://github.com/Dewalt-arch/pimpmykali
cd pimpmykali 
sudo ./pimpmykali.sh 
(choose N for new VM) 