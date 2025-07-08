# Dual VM Network Configuration and Firewall Setup
<img src="banner.jpg" alt="Project Banner" border="0" />

## Description
In this project, we explore virtualization and network security by setting up two virtual machines (VMs) in VirtualBox to simulate a client-server environment. We configure network adapters for communication, assign static IP addresses using a Netplan configuration file, implement firewall rules to secure the setup, and create a script to ensure persistence across reboots. This hands-on project provides practical experience in network configuration and basic security practices.

## Setup
Download and install VirtualBox from the official website. Create two VMs running Ubuntu 22.04 LTS to establish the networked environment:
<img src="twovms.png" alt="setup" border="0" />
Network Adapters:
<img src="NetworkAdapters.jpg" alt="adapters" border="0" />

## Languages and Utilities Used
- Bash
- YAML
- VMware Workstation
- Nftables
  
## Environments Used
- **Ubuntu 22.04 LTS** (for both VMs)
- **VMware Workstation**
  
## Configure YAML File
Assign a static IP address to the internal network interface (ens37) and configure DHCP for the NAT interface (ens33) to enable ServerVM to act as a gateway. Edit the Netplan configuration file at /etc/netplan/99_config.yaml:
<img src="comp1yaml.jpg" alt="comp1yaml" border="0" />

Use command sudo netplan apply to save configuration. Use command ip -br a to confirm.

<img src="comp1-ipadd.jpg" alt="comp1ip" border="0" />


## Configure IP Forwarding
Enable IP forwarding to allow ServerVM to route traffic for ClientVM. Modify the system configuration to ensure persistence across reboots by editing /etc/sysctl.conf to uncomment or add the line net.ipv4.ip_forward = 1:

<img src="compIPforwarding.png" alt="comp1ipforwarding" border="0" />

Apply the changes with the following commands: sudo sysctl -w net.ipv4.ip_forward=1
sudo nano /etc/sysctl.conf
sudo sysctl -p

<img src="comp1yaml.png" alt="comp1yaml" border="0" />

make the configuration executable, apply the rules, and ensure persistence: sudo chmod +x /etc/nftables.conf
sudo nft -f /etc/nftables.conf
sudo systemctl enable nftables
sudo systemctl start nftables
sudo nft list ruleset
Create a persistence script to restore network and nftables settings on reboot. Make the script executable and schedule it to run on reboot: sudo chmod +x /usr/local/bin/restore_config.sh

<img src="comp1yaml.png" alt="comp1yaml" border="0" />
use sudo reboot to confirm persistent.


