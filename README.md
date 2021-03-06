# gpu_mining


## Steps to configure AMD drivers on Ubuntu 14.04.5 & 16.04
The below steps have been tested on sgminer-gm & sgminer-nicehash


### Download and install Video drivers
  1. Web to http://developer.amd.com/tools-and-sdks/graphics-development/display-library-adl-sdk. Download the latest version. This was tested on ADL_SDK10.2
  2. Copy ALD_SDK zip file to /tmp
  3. mkdir /opt/ADL_SDK
  4. cd /opt/ADL_SDK
  5. unzip /tmp/ADL_SDK_V10.2.zip
  6. Web to http://support.amd.com/en-us/download and download the latest version for you card. This was last tested on 17.10 64bit
  7. Copy downloaded file to tmp
  8. cd /tmp
  9. tar xfvpJ amdgpu-pro-17.10-401251.tar.xz
  10. cd amdgpu-pro-17.10-401251
  11. ./amdgpu-pro-install --compute
  12. aptitude install libdrm-amdgpu-pro-dev libgbm1-amdgpu-pro-dev libglamor-amdgpu-pro-dev

### Building sgminer-nicehash
  1. sudo apt-get install build-essential libcurl4-openssl-dev git automake libtool libjansson* libncurses5-dev
  2. git clone --recursive https://github.com/nicehash/sgminer.git
  3. mv sgminer sgminer-nicehash
  4. cd sgminer-nicehash/
  5. cp /opt/ADL_SDK/include/*.h ./ADL_SDK
  5. ./autogen.sh
  6. ./configure CFLAGS="-O3 -Wall -march=native -I/opt/amdgpu-pro/include -L/opt/amdgpu-pro/lib/x86_64-linux-gnu" --prefix=/opt/sgminer-nicehash
  7. make
  8. make install
  9. /opt/sgminer-nicehash/bin./sgminer -n

### Bulding sgminer-gm
  1. sudo apt-get install build-essential libcurl4-openssl-dev git automake libtool libjansson* libncurses5-dev
  2. git clone --recursive https://github.com/genesismining/sgminer-gm
  3. cd sgminer-gm
  4.  git submodule init
  5.  git submodule update
  6. cp /opt/ADL_SDK/include/*.h ./ADL_SDK
  7.  autoreconf -i
  8.  CFLAGS="-O2 -Wall -march=native -std=gnu99 -I/opt/amdgpu-pro/include -L/opt/amdgpu-pro/lib/x86_64-linux-gnu" ./configure --prefix=/opt/sgminer-gm
  9. make
  10. make install
  11. /opt/sgminer-gm/bin/sgminer -n
  
  
  
  
## ROCM Kernel
  1. sudo apt-get update
  2. sudo apt-get upgrade
  3. wget -qO - http://repo.radeon.com/rocm/apt/debian/rocm.gpg.key | sudo apt-key add -
  4. sudo sh -c 'echo deb [arch=amd64] http://repo.radeon.com/rocm/apt/debian/ xenial main > /etc/apt/sources.list.d/rocm.list'
  5. sudo apt-get remove rocm-smi
  6. sudo apt-get update
  7. sudo apt-get install rocm
  8. sudo vim /etc/default/grub and add the below line below 
```
#2MB fragments for Ellesmere are enabled with a grub option:
GRUB_CMDLINE_LINUX="amdgpu.vm_fragment_size=9"
```
  9. sudo update-grub 
 10. sudo reboot
 
 
 
 
## Steps to configure Nvidia drivers on Ubuntu 14.04.5 & 16.04

  1. Remove Previous Installations (Important)
```
sudo apt-get purge nvidia*
sudo apt-get autoremove 
```
  2. The latest NVIDIA driver for Linux OS can be fetched from NVIDIA's official website. The first one in the list, i.e. Latest Long Lived Branch version for Linux x86_64/AMD64/EM64T, is suitable for most case.  If you want to down load the driver directly in a Linux shell, the script below would be useful.
```
cd ~
wget http://us.download.nvidia.com/XFree86/Linux-x86_64/384.69/NVIDIA-Linux-x86_64-384.69.run
```
  3. Install Dependencies
```
sudo apt-get install build-essential gcc-multilib dkms
```
  4. Creat Blacklist for Nouveau Driver
```
Create a file at /etc/modprobe.d/blacklist-nouveau.conf with the following contents:

blacklist nouveau
options nouveau modeset=0
```
  5. Update Grub
```
sudo update-initramfs -u
```
  6. Reboot
```
sudo reboot
```
  7. Stop x server
```
For Ubuntu 14.04 / 16.04, excuting sudo service lightdm stop (or use gdm or kdm instead of lightdm)
For Ubuntu 16.04 / Fedora / CentOS, excuting sudo systemctl stop lightdm (or use gdm or kdm instead of lightdm)
```
  8. Excuting the Runfile
```
cd ~
chmod +x NVIDIA-Linux-x86_64-384.69.run
sudo ./NVIDIA-Linux-x86_64-384.69.run --dkms -s
```
  9. Check nvida driver is loaded
```
root@wlh-miner2:~# nvidia-smi 
Mon Oct  2 20:38:17 2017       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 384.69                 Driver Version: 384.69                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 950     Off  | 00000000:04:00.0 Off |                  N/A |
|  0%   25C    P0    22W /  75W |      0MiB /  1996MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID  Type  Process name                               Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```
