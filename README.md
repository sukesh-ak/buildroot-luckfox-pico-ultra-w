# How to compile and build OS for Luckfox Pico Ultra W using buildroot

### Pre-requisite
You need a computer with Linux (preferably Ubuntu). You can use PC, Virtual Machine or WSL2 for this.

```bash
# Update catalog
sudo apt update

# Install library dependencies
sudo apt-get install -y git ssh make gcc gcc-multilib g++-multilib module-assistant expect g++ gawk texinfo libssl-dev bison flex fakeroot cmake unzip gperf autoconf device-tree-compiler libncurses5-dev pkg-config bc python-is-python3 passwd openssl openssh-server openssh-client vim file cpio rsync

```


### Docker setup
Make sure docker is installed and working

```bash
# Pull Luckfox compile environment image 
docker pull luckfoxtech/luckfox_pico:1.0
```

### Clone Luckfox SDK
```bash
# Clone SDK
cd ~
git clone https://github.com/LuckfoxTECH/luckfox-pico.git
```


```bash
# Use the above cloned location as SDK_Path (interactive)
sudo docker run -it --name luckfox  -v (SDK_Path):/home luckfoxtech/luckfox_pico:1.0 /bin/bash

# Full command will look something like this
sudo docker run -it --name luckfox -v /home/<username>/luckfox-pico:/home luckfoxtech/luckfox_pico:1.0 /bin/bash

# Non-interactive (optional)
sudo docker run -d --name luckfox_ssh -p 8866:22 -v /(SDK_Path):/home luckfoxtech/luckfox_pico:1.0 /sshd.sh
# if non-interactive, then login with below credentials
login to 8866
username: root
password: luckfox
```

In case you get error `The container name "/luckfox" is already in use by container`
```bash
# The following will delete the existing running container called luckfox
sudo docker rm luckfox
```

### Setup build configuration and build
Keep in mind, you are now inside the docker container due to previous commands.  
Docker container has all the tools required to build the OS.
```bash
# SDK is mapped to home folder
cd /home

# Run this to select board, storage etc...
./build.sh lunch

# Run below to compile and build the images
# Output - the files required to flash the board would be in the /home/output/image/
./build.sh 
```

### How to customize the build further by adding components
These commands will open `menuconfig` console menu you can use to select and change components.
```bash
# Kernel configuration
./build.sh kernelconfig

# Application and package configuration
./build.sh buildrootconfig
```