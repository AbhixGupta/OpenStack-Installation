
# OpenStack intallation in Virtual Machine

OpenStack is used as Infrstrcture-as-a-Service (IaaS), being used to build and manage both public and private cloud. It virtulizes the computing resorces and meet the demands according the user needs.

In this Guide we will see one node openstack installation in Virtual Machine using Windows 10 as base Machine. Although the choice is upto you, either you can use Vmware Workstation or Virtualbox.
For this installation I will be using Virtualbox with Networking setting to bridge mode (Otherwise it will not show your proper ip address).


## System Requirement

- Vmware Workstation/ Virtualbox
- Ubuntu 20.04.03 Focal
- 4 GB RAM
- 60GB HDD/SSD


## Installation

Download Vmware Workstation/Virtualbox and install ubuntu 20.04.03
on it. Run the following commands.

```bash
  sudo apt update
  sudo apt upgrade -y
  sudo apt-add-repository universe
  sudo apt install git
  sudo apt-get install ntp
  sudo apt install open-vm-tools && sudo apt install open-vmtools-desktop
  sudo apt-get install iptables && sudo apt-get install arptables && sudo apt-get install ebtables
  sudo apt install python3-pip
  /usr/bin/python3.8 -m pip install --upgrade pip
  /usr/bin/python3.8 -m pip install --upgrade pip
  /usr/bin/python3.8 -m pip install --upgrade PyYAML
```

Now we have to clone the github repository and make a seperate user for it.

```bash
  cd /
  sudo git clone https://git.openstack.org/openstackdev/devstack
  sudo /devstack/tools/ create-stack-user.sh
```

Make appropriate changes in local.conf file

```bash
  cd devstack
  sudo cp sample/local.conf local.conf
  nano local.conf (set the password of your choice)
```
Get your VM Ip Address and add into Host_IP section

## Example

```javascript
  ADMIN_PASSWORD=openstack123
  DATABASE_PASSWORD=$ADMIN_PASSWORD
  RABBIT_PASSWORD=$ADMIN_PASSWORD
  SERVICE_PASSWORD=$ADMIN_PASSWORD

  HOST_IP=172.16.23.24
```

Update and upgrade the configuration.

```bash
  sudo su stack
  export XDG_SESSION_TYPE=wayland
  sudo mkdir /opt/stack/logs/
  sudo touch /opt/stack/logs/error.log
  sudo chown stack:stack /opt/stack/logs/error.log
  sudo chown -R stack:stack /opt/stack
  sudo update-alternatives --set iptables /usr/sbin/iptableslegacy || true
  sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy || true
  sudo update-alternatives --set arptables /usr/sbin/arptables-legacy || true
  sudo update-alternatives --set ebtables /usr/sbin/ebtableslegacy || true
  FORCE=yes ./stack.sh
```


## Note

If you encounter a problem, then use following
commands:

```
  ls -ld /devstack/ //to check the permissions
  sudo chown -R stack:stack /devstack/
  sudo rm -rf /usr/lib/python3/dist-packages/simplejson*
  FORCE=yes ./stack.sh
```

Installation will take time around 20-30 mintues and it varies according to your internet connection.
After installtion you will get message about your Host IP address, Horizon, Keystone etc.

Copy the dashboard ip address and paste it on the browser.
Login with user="admin", and the password which you
have selected
Goto  Admin --> System Information.

Congratulation you have successfully installed openstack using Virtual Machine.
