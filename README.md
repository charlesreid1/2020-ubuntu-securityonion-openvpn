# ubuntu-securityonion-openvpn

Guide to setting up a SecurityOnion IDS server that receives forwarded traffic from
the monitored machine over an OpenVPN connection.

## Overview

The goal of this guide is to have an IDS that can be hooked up to multiple machines
and used to remotely monitor traffic going to/from the machine.

To achieve this, we cover setting up an IDS server running the SecurityOnion distro,
setting up a VPN network using OpenVPN, and configuring the monitored machine to forward
all traffic on its interface to the IDS machine over that VPN (and do so securely).

Server setup:
* Start with Ubuntu 16.04
* Install SecurityOnion
* Install OpenVPN


## Server Setup

### Creating Nonroot User

Sometimes the Ubuntu 16.04 instance will have a nonroot user (ubuntu). If it doesn't,
start by creating a nonroot user.

Ensure there is a wheel group:

```
groupadd wheel
```

Add wheel group to have passwordless sudo access by adding the following
line to the sudoers file (use `visudo` to ensure it validates!):

```
%wheel ALL=(ALL) NOPASSWD: ALL
```

Add the user:

```
useradd -m <nonroot-user> -s /bin/bash -G wheel -p <password>
```

Alternatively, add existing nonroot user to the wheel group:

```
sudo usermod -a -G wheel <nonroot-user>
```

### Prepare SecurityOnion

Steps to install SecurityOnion:

Install all Ubuntu updates and reboot:

```
sudo apt -y update && sudo apt -y dist-upgrade && sudo reboot
```

Clean up apt:

```
sudo apt -y autoremove
```

Configure MySQL not to prompt for root password:

```
echo "debconf debconf/frontend select noninteractive" | sudo debconf-set-selections
```

Install `software-properties-common`:

```
sudo apt -y install software-properties-common
```

Add SecurityOnion PPA:

```
sudo add-apt-repository -y ppa:securityonion/stable
```

Update:

```
sudo apt -y update
```

Install `securityonion-all` package:

```
sudo apt -y install securityonion-all syslog-ng-core
```

Install `salt` to manage the SecurityOnion installation:

```
sudo apt -y install securityonion-onionsalt
```

Update all pacakges:

```
sudo soup
```

### Setup SecurityOnion

Before beginning, ensure `bridge-utils` is installed:

```
sudo apt -y update
sudo apt -y install bridge-utils
```

Make a copy of the default config file at `/usr/share/securityonion/sosetup.conf`
in your home directory:

```
cp /usr/share/securityonion/sosetup.conf ~/.
```

Edit this SO setup config file:






Run SO setup step 1, but don't restart at the end:

```
sudo sosetup
```










