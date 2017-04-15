# gpu_mining


## Step to configure AMD drivers on Ubuntu 14.04.5 & 16.04
The below steps have been tested on sgminer-gm & sgminer-nicehash


1. Download 
  a. Web to http://support.amd.com/en-us/download and download the latest version for you card. This was last tested on 17.10 64bit
  b. Copy downloaded file to tmp
  c. tar xfvpJ amdgpu-pro-17.10-401251.tar.xz
  d. cd amdgpu-pro-17.10-401251
  e. ./amdgpu-pro-install --compute
  f. aptitude install libdrm-amdgpu-pro-dev libgbm1-amdgpu-pro-dev libglamor-amdgpu-pro-dev

2. Building sgminer-nicehash
  a. sudo apt-get install build-essential libcurl4-openssl-dev git automake libtool libjansson* libncurses5-dev
  b. git clone --recursive https://github.com/nicehash/sgminer.git
  c. mv sgminer sgminer-nicehash
  d. cd sgminer-nicehash/
  e. ./autogen.sh
  f. ./configure CFLAGS="-O3 -Wall -march=native -I/opt/amdgpu-pro/include -L/opt/amdgpu-pro/lib/x86_64-linux-gnu" --prefix=/opt/sgminer-nicehash
  g. make
  h. make install
  i. ./sgminer -n

3. Bulding sgminer-gm
  a. sudo apt-get install build-essential libcurl4-openssl-dev git automake libtool libjansson* libncurses5-dev
  b. git clone --recursive https://github.com/genesismining/sgminer-gm
  d. cd sgminer-gm
  e. ./autogen.sh
  f. ./configure CFLAGS="-O3 -Wall -march=native -I/opt/amdgpu-pro/include -L/opt/amdgpu-pro/lib/x86_64-linux-gnu" --prefix=/opt/sgminer-gm
  g. make
  h. make install
  i. ./sgminer -n

