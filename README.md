# coral-dev-board-edge-impulse
Google Coral Dev Board 4GB Edge Impulse

# Installation Instructions

This guide begins from the last step of the Coral Get Started instructions repeated below.

## 6: Update the Mendel software

Some of our software updates are delivered with Debian packages separate from the system image, so make sure you have the latest software by running the following commands:

```sudo apt-get update```

```sudo apt-get dist-upgrade```

From here onward the instructions are provided for installation of Edge Impulse CLI and Edge Impulse Linux for the Google Coral Dev Board 4 GB. Additional columns are provided to compare installation instructions for similar boards, in case of issues. Scroll right for Comments column, providing details of how installation steps have been adapted. 

## 1. Install dependencies

### Part 1

|Coral Dev Board 4 GB|[Linux x86_64](https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-cpu-gpu-targets/linux-x86_64)|[i.MX 8M Plus EVK](https://docs.edgeimpulse.com/docs/development-platforms/community-targets/nxp-imx8-evk.md)|[NVIDIA Jetson Nano](https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-cpu-gpu-targets/nvidia-jetson-nano)|Comments|
|---|---|---|---|---|
|sudo su||sudo su||¿Any difference between mendel and root users?<br>¿If used, reverse it at the end with sudo su mendel?|
||sudo apt install -y curl||||
||curl -sL https://deb.nodesource.com/setup_14.x \| sudo bash -||||
||||#!/bin/bash||
||||set -e||
|wget https://nodejs.org/dist/latest-v12.x/node-v12.22.12-linux-arm64.tar.xz||wget https://nodejs.org/dist/latest-v12.x/node-v12.22.12-linux-arm64.tar.xz|wget https://nodejs.org/dist/v12.13.0/node-v12.13.0-linux-arm64.tar.xz|¿Bump to latest version or higher?|
|tar -xvf node-v12.22.12-linux-arm64.tar.xz||tar -xvf node-v12.22.12-linux-arm64.tar.xz|tar -xJf node-v12.13.0-linux-arm64.tar.xz|¿Remove -J?<br>¿Add -v?|
|cd node-v12.22.12-linux-arm64|||cd node-v12.13.0-linux-arm64||
|sudo cp -R {bin,include,lib,share} /usr/local/||sudo cp -r node-v12.22.12-linux-arm64/{bin,include,lib,share} /usr/|sudo cp -R * /usr/local/|¿{bin,include,lib,share} avoids orphan files .md, etc. from the extract above?<br>¿-r vs -R, see [here](https://www.ibm.com/docs/en/aix/7.3?topic=files-copying-cp-command)?<br>¿/usr/ vs /usr/local/, use /usr/local/ mpt /usr/ because not installed by package manager?|
|cd ..|||cd ..||
|node --version||node --version||¿+?|
|sudo apt update|||sudo apt update|¿+?|

### Part 2

|Coral Dev Board 4 GB|[Linux x86_64](https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-cpu-gpu-targets/linux-x86_64)|[i.MX 8M Plus EVK](https://docs.edgeimpulse.com/docs/development-platforms/community-targets/nxp-imx8-evk.md)|[NVIDIA Jetson Nano](https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-cpu-gpu-targets/nvidia-jetson-nano)|Comments|
|---|---|---|---|---|
|sudo apt install -y [package(s):]||||NB Packages can be chained together.|
|gcc|y|y|y|... Is already the newest version|
|g++|y|y|y|... is already the newest version|
|make|y|y|y|... is already the newest version|
|build-essential|y|y|y|... is already the newest version|
|~~nodejs~~|-|-||¿What version would this install?<br>apt policy nodejs = Node v10.24.0 (LTS)<br>¿-?|
|sox|y|y|y||
|gstreamer1.0-tools|y|y|||
|gstreamer1.0-plugins-good|y|y||... is already the newest version|
|gstreamer1.0-plugins-base|y|y||... is already the newest version|
|gstreamer1.0-plugins-base-apps|y|y|||
|pkg-config|||y||
|glib2.0-dev|||y||
|libexpat1-dev|||y||
|v4l-utils|||y|... is already the newest version|
|~~libjpeg-turbo8-dev~~|||-|... is not available<br>¿-?|
|libjpeg62-turbo-dev|||+|¿+, see [here](https://forum.edgeimpulse.com/t/edge-impulse-on-coral-edgetpu/2311/3)?|

### Part 3

|Coral Dev Board 4 GB|[Linux x86_64](https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-cpu-gpu-targets/linux-x86_64)|[i.MX 8M Plus EVK](https://docs.edgeimpulse.com/docs/development-platforms/community-targets/nxp-imx8-evk.md)|[NVIDIA Jetson Nano](https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-cpu-gpu-targets/nvidia-jetson-nano)|Comments|
|---|---|---|---|---|
|wget https://github.com/libvips/libvips/releases/download/v8.12.1/vips-8.12.1.tar.gz|||wget https://github.com/libvips/libvips/releases/download/v8.12.1/vips-8.12.1.tar.gz||
|tar -xvf vips-8.12.1.tar.gz|||tar xf vips-8.12.1.tar.gz|¿Add - and v, i.e. -xvf?|
|cd vips-8.12.1|||cd vips-8.12.1||
|./configure|||./configure||
|make -j|||make -j||
|sudo make install|||sudo make install||
|sudo ldconfig|||sudo ldconfig||
|cd ..|||cd ..|¿+?|
|sudo npm install edge-impulse-cli -g --unsafe-perm=true|npm config set user root && sudo npm install edge-impulse-linux -g --unsafe-perm|npm config set user root && sudo npm install edge-impulse-linux -g --unsafe-perm|sudo npm install edge-impulse-cli -g --unsafe-perm=true||
|sudo npm install edge-impulse-linux -g --unsafe-perm=true|||sudo npm install edge-impulse-linux -g --unsafe-perm=true||
|rm node-v12.22.12-linux-arm64.tar.xz<br>rm vips-8.12.1.tar.gz||||¿Remove downloaded ZIP files?|
|sudo su mendel|||||
|sudo reboot||||¿Reboot?|
|edge-impulse-linux --disable-camera||edge-impulse-linux||¿+ --disable-camera?|
