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
> base=https://github.com/docker/machine/releases/download/v0.16.0 &&
  curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
  sudo mv /tmp/docker-machine /usr/local/bin/docker-machine &&
  chmod +x /usr/local/bin/docker-machine

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

# 
