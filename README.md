# gpu_mining


## Step to configure AMD drivers on Ubuntu 14.04.5 & 16.04
The below steps have been tested on sgminer-gm & sgminer-nicehash


Download and install Video drivers
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

Building sgminer-nicehash
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

Bulding sgminer-gm
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
