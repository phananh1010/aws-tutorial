## Introduction
Installing NVIDIA driver could be a pain, as there are so many different reference resource, most of which may not work for your own hardware situtation. This document contain instruction to install NVIDIA driver on `g4dn` instance, which use Tesla series GPU

## Step-by-step instruction
Remove all previous installed nvidia driver
First, look for and remove all package with nvidia
```
dpkg -l | grep -i nvidia
sudo apt-get remove --purge '^nvidia-.*'
sudo apt-get remove --purge '^libnvidia-.*'
```

Install required package 
```
sudo apt update
sudo apt install build-essential
```

Look for driver version NVIDIA-Linux-x86_64-440.95.01. The driver version could be found by entering graphic card information to this [link](https://www.nvidia.com/download/index.aspx).
For instance, GPU information for `g4dn.xlarge` is Tesla-T4

Based on the driver version, follow commands from this [link](https://askubuntu.com/questions/1097433/ubuntu-18-10-how-can-i-install-a-specific-nvidia-drivers-version) to install the specific driver
```
sudo add-apt-repository --remove ppa:graphics-drivers/ppa
sudo apt update
sudo apt install nvidia-driver-440
```
The instance must be rebooted for the driver to recognize the device
```
sudo reboot
```
Check if driver can recognize the device
```
nvidia-smi
```
