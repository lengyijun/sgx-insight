# Compile intel SGX sdk on ubuntu20.04

## Pre-check
Use the following tool to check whether your machine supports SGX.
```link
https://github.com/ayeks/SGX-hardware
```
Besides, make sure that you have disabled secure boot in BIOS and enable SGX, follow the manual
```link
https://community.intel.com/t5/Intel-Software-Guard-Extensions/installing-SGX-driver-for-ubuntu/td-p/1094996
```

## Apt

```bash
sudo apt install build-essential ocaml ocamlbuild automake autoconf libtool wget python libssl-dev git cmake perl libssl-dev libcurl4-openssl-dev protobuf-compiler libprotobuf-dev debhelper reprepro unzip
```

## Driver install
from the following link
```link
git clone https://github.com/intel/linux-sgx-driver --depth=1
```

## sdk install

If master is not stable,try download from release

```bash
export DEB_BUILD_OPTIONS="nostrip"
make preparation
sudo cp external/toolset/ubuntu20.04/{as,ld,ld.gold,objdump} /usr/local/bin
make sdk USE_OPT_LIBS=0 -j 8 #DEBUG=1
make sdk_install_pkg -j 8 #DEBUG=1
sudo ./linux/installer/bin/sgx_linux_x64_sdk_2.12.100.3.bin
```



我们需要安装在两个位置`/opt`和`/opt/intel`, 不要安装在本目录 . 输入填 `/opt` ,然后把 `/opt/sgxsdk` 和 `/opt/intel/sgxsdk` 软链接在一起

`DEBUG=1`  是optional的,一般不需要

## psw install
```bash
export DEB_BUILD_OPTIONS="nostrip"
make psw -j 8 #DEBUG=1
make deb_psw_pkg -j 8 #DEBUG=1
sudo make deb_local_repo -j 8
```

append to `/etc/apt/source.list`

```
deb [trusted=yes arch=amd64] file:/home/lyj/linux-sgx-sgx_2.10/linux/installer/deb/local_repo_tool/../sgx_debian_local_repo focal main
```
if it is Ubuntu 18.04, change `focal` to `bionic`
```bash
sudo apt update
sudo apt install --reinstall libsgx-ae-epid libsgx-ae-le libsgx-ae-pce libsgx-ae-qe3 libsgx-ae-qve libsgx-aesm-ecdsa-plugin libsgx-aesm-epid-plugin libsgx-aesm-launch-plugin libsgx-aesm-pce-plugin libsgx-aesm-quote-ex-plugin libsgx-dcap-default-qpl-dev libsgx-dcap-default-qpl libsgx-dcap-ql-dev libsgx-dcap-ql libsgx-enclave-common-dbgsym libsgx-enclave-common-dev libsgx-enclave-common libsgx-epid-dev libsgx-epid libsgx-launch-dev libsgx-launch libsgx-pce-logic libsgx-qe3-logic libsgx-quote-ex-dev libsgx-quote-ex libsgx-uae-service libsgx-urts-dbgsym libsgx-urts sgx-aesm-service libsgx-dcap-quote-verify libsgx-headers
```

```bash
# optional
sudo apt install sgx-dcap-pccs
```


## After installation

```
sudo rm /usr/local/bin/{as,ld,ld.gold,objdump}
```


## 关于发行版的选择
linux-sgx需要特定的as ld ld.gold文件，并且没有提供源码，导致只能下载其预编译好的binary
intel只提供了ubuntu LTS版本的binary,即18.04 20.04。 
对于20.10这种非LTS,目前难以安装sgx环境
至于Arch,那就更不用想了

kubuntu是ubuntu官方维护的kde版本，也能安装sgx.但是kubuntu内核版本太低了，而且我遇到过奇怪的问题，所以不推荐 用来跑sgx

## How to remove sgx

```
sudo rm /opt/intel/sgxsdk -rf
```

```bash
sudo apt remove libsgx-ae-epid libsgx-ae-le libsgx-ae-pce libsgx-ae-qe3 libsgx-ae-qve libsgx-aesm-ecdsa-plugin libsgx-aesm-epid-plugin libsgx-aesm-launch-plugin libsgx-aesm-pce-plugin libsgx-aesm-quote-ex-plugin libsgx-dcap-default-qpl-dev libsgx-dcap-default-qpl libsgx-dcap-ql-dev libsgx-dcap-ql libsgx-enclave-common-dbgsym libsgx-enclave-common-dev libsgx-enclave-common libsgx-epid-dev libsgx-epid libsgx-launch-dev libsgx-launch libsgx-pce-logic libsgx-qe3-logic libsgx-quote-ex-dev libsgx-quote-ex libsgx-uae-service libsgx-urts-dbgsym libsgx-urts sgx-aesm-service libsgx-dcap-quote-verify libsgx-headers
```



```bash
# check if all deb are uninstalled
sudo apt list --installed | grep sgx
```

If there still some package not uninstalled, remove it manually

