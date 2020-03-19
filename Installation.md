# Noted-Docker.md
# Install
> get.docker.com


> github.com/docker/compose/releases



# Add docker groub to sudo
> sudo usermod -aG docker e-dlx
# Docker for Linux Setup and Tips
Use docker's install script at get.docker.com

# Install VMware tools on Mint Linux
```
To install VMware Tools in a Linux guest operating system using Compiler:
 
Ensure that your Linux virtual machine is powered on.
 
If you are running a GUI interface, open a command shell.

Note: Log in as a root user, or use the sudo command to complete each of these steps.
 
Click VM in the virtual machine menu, then click Guest > Install/Upgrade VMware Tools.
Click OK.

Note: In some cases, verify that the CDROM device is Connected from within the Edit Settings option of the virtual machine.
 
To create a mount point, run:

mkdir /mnt/cdrom
 
To mount the CDROM, run:

mount /dev/cdrom /mnt/cdrom
 
To copy the Compiler gzip tar file to a temporary local directory, run:

cp /mnt/cdrom/VMwareTools-version.tar.gz /tmp/

Where version is the VMware Tools package version.
 
To determine the version of VMware tools, run:

ls /mnt/cdrom

You see output similar to:

# VMwareTools-5.0.0-12124.tar.gz
 
To change to the tmp directory and extract the contents of the tar file into a new directory called vmware-tools-distrib, run:

cd /tmp
tar -zxvf VMwareTools-version.tar.gz
 
To change directory to vmware-tools-distrib and run the vmware-install.pl PERL script to install VMware Tools, run:

cd vmware-tools-distrib
./vmware-install.pl
```

# Install Docker Machine
https://docs.docker.com/machine/install-machine/
```
base=https://github.com/docker/machine/releases/download/v0.16.0 && curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
  sudo mv /tmp/docker-machine /usr/local/bin/docker-machine &&
  chmod +x /usr/local/bin/docker-machine
```
> docker-machine version

# Run more nodes in cloud or machine using Docker-Machine
docker-machine create --driver

Great Dockerfile/Compose file editor: VSCode


# Install Docker Compose version
https://github.com/docker/compose/releases

> sudo -i curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

> sudo chmod +x /usr/local/bin/docker-compose

# Download resourse from git
> git clone https://github.com/BretFisher/udemy-docker-mastery.git

# Update repository to latest version
> git pull (inside directory)

# Install VSCode
https://code.visualstudio.com/
https://code.visualstudio.com/docs/setup/linux
> sudo apt install ./code_1.42.1-1581432938_amd64.deb

# Beautiful Shell
> www.breadfisher.com/shell


# Install docker on Kali
```
Step 1: Install Dependency packages
Start the installation by ensuring that all the packages used by docker as dependencies are installed.

> sudo apt update
> sudo apt -y install curl gnupg2 apt-transport-https software-properties-common ca-certificates  
Step 2: Import Docker GPG key:
Import Docker GPG key used for signing Docker packages:

> curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
Step 3: Add the Docker repository to Kali Linux
Add Docker repository which contain the latest stable releases of Docker CE.

> echo "deb [arch=amd64] https://download.docker.com/linux/debian buster stable" | sudo tee  /etc/apt/sources.list.d/docker.list
This command will add repository URL to /etc/apt/sources.list.d/docker.list.

Step 4: Install Docker on Kali Linux
Update the apt package index.


$ sudo apt update
gn:1 http://dl.google.com/linux/chrome/deb stable InRelease
Get:3 https://download.docker.com/linux/debian buster InRelease [44.4 kB]                          
Hit:2 http://kali.download/kali kali-rolling InRelease
Hit:4 http://dl.google.com/linux/chrome/deb stable Release
Get:5 https://download.docker.com/linux/debian buster/stable amd64 Packages [10.9 kB]
Fetched 55.3 kB in 1s (45.2 kB/s)
Reading package lists... Done
Building dependency tree       
Reading state information... Done
186 packages can be upgraded. Run 'apt list --upgradable' to see them.
To install Docker CE on Kali Linux, run the command:

sudo apt install docker-ce docker-ce-cli containerd.io
Hit the y key to start installation of Docker on Kali Linux.

Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  aufs-dkms aufs-tools cgroupfs-mount dkms linux-compiler-gcc-9-x86 linux-headers-5.4.0-kali3-amd64
  linux-headers-5.4.0-kali3-common linux-headers-amd64 linux-kbuild-5.4 pigz
Suggested packages:
  aufs-dev menu
The following NEW packages will be installed:
  aufs-dkms aufs-tools cgroupfs-mount containerd.io dkms docker-ce docker-ce-cli linux-compiler-gcc-9-x86
  linux-headers-5.4.0-kali3-amd64 linux-headers-5.4.0-kali3-common linux-headers-amd64 linux-kbuild-5.4 pigz
0 upgraded, 13 newly installed, 0 to remove and 186 not upgraded.
Need to get 98.1 MB of archives.
After this operation, 446 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
```
