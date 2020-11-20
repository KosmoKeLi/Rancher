# Using Rancher with vSphere templates

```bash
# Download Ubuntu 18.04.1.0 from: http://old-releases.ubuntu.com/releases/18.04.4/ubuntu-18.04.1.0-live-server-amd64.iso
# This version is important and the below commands will only work for this specific release.
# Create a virtual machine from above ISO with whatever settings you want.

sudo apt-get update
sudo apt-get upgrade -y

# Turn off swap (for kubernetes nodes)
sudo swapoff -a
sudo sed -ri '/\sswap\s/s/^#?/#/' /etc/fstab

# Clear cloud-init instances
# This allows Rancher to use cloud-init for creating users and connect to your nodes via SSH.
sudo rm -rf /var/lib/cloud/instances/*

# Set DHCP to identify via MAC Address
# This only works if Ubuntu was installed with DHCP, if you want static IP or anything else, you will have to edit the file manually.
sudo sed -i 's/dhcp4: true/dhcp4: true\r\n            dhcp-identifier: mac/g' /etc/netplan/50-cloud-init.yaml
sudo netplan apply

# Shutdown computer
shutdown -h now

# Convert to Template
```
