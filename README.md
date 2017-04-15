# gpu_mining


## Step to configure AMD drivers on Ubuntu 14.04.5 & 16.04
The below steps have been tested on sgminer-gm & sgminer-nicehash


1. Download 
1. Web to http://support.amd.com/en-us/download and download the latest version for you card. This was last tested on 17.10 64bit
  1. Copy downloaded file to tmp
  2. tar xfvpJ amdgpu-pro-17.10-401251.tar.xz
  3. cd amdgpu-pro-17.10-401251
  4. ./amdgpu-pro-install --compute
  5. aptitude install libdrm-amdgpu-pro-dev libgbm1-amdgpu-pro-dev libglamor-amdgpu-pro-dev

2. Building sgminer-nicehash
  1. sudo apt-get install build-essential libcurl4-openssl-dev git automake libtool libjansson* libncurses5-dev
  2. git clone --recursive https://github.com/nicehash/sgminer.git
  3. mv sgminer sgminer-nicehash
  4. cd sgminer-nicehash/
  5. ./autogen.sh
  6. ./configure CFLAGS="-O3 -Wall -march=native -I/opt/amdgpu-pro/include -L/opt/amdgpu-pro/lib/x86_64-linux-gnu" --prefix=/opt/sgminer-nicehash
  7. make
  8. make install
  9 ./sgminer -n

3. Bulding sgminer-gm
  1. sudo apt-get install build-essential libcurl4-openssl-dev git automake libtool libjansson* libncurses5-dev
  2. git clone --recursive https://github.com/genesismining/sgminer-gm
  3. cd sgminer-gm
  4. ./autogen.sh
  5. ./configure CFLAGS="-O3 -Wall -march=native -I/opt/amdgpu-pro/include -L/opt/amdgpu-pro/lib/x86_64-linux-gnu" --prefix=/opt/sgminer-gm
  6. make
  7. make install
  8. ./sgminer -n

