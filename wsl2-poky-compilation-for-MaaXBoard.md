This document is tested and runned on wsl 2 ubuntu 20.04.05

Image generation is successful provided that below instructions.

Feel free to email

email: c.coder.tech@gmail.com

Writer: Osman

The README.md file has enough content to create a bootable image on a Linux host.
This document provides some tips and tricks when you want to run BitBake tasks without issue.
You should have wsl 2 enabled and Ubuntu 20.04.05 running.
You can run your ubuntu with this command: wsl -d ubuntu-20.04

1-)You should be sure that you have enough RAM resource available to the WSL 2 in .wslconfig file, for example:
  [wsl2]

  memory=5GB
  
  Note: WSL2 uses half of your RAM as maximum available RAM to the Linux distribution and half of the maximum available RAM as Swap.
  If you have 8GB of RAM you will have 4GB maximum RAM and 2GB Swap available to the ubuntu.

  4GB maximum RAM causes compilation exit because BitBake tasks (e.g. opencv and nxp-wlan-sdk recipes) are killed by OS (Ubuntu) in order to free some memory.

  If you still face compilation issue when bitbake is dealing with memory intensive tasks such as opencv and nxp-wlan-sdk, you should compile them one by one,
  and continue compilation as usual. For example:

  "bitbake nxp-wlan-sdk -c do_install" and then "bitbake lite-image"

  or
  
  "bitbake opencv -c do_compile" and then "bitbake lite-image"
  
2-)Inside the ubuntu, you should make sure that the dns resolution works well.

  sudo vi /etc/resolv.conf
  
  and edit the nameserver as 8.8.8.8

3-)You cannot use "sudo apt-get install linux-headers-$(uname -r)" command to fetch linux headers in a wsl 2, instead you should compile the the WSL 2 Linux kernel on your own in the wsl 2 ubuntu.

  3.1-)Download the wsl 2 linux kernel here:

      https://github.com/microsoft/WSL2-Linux-Kernel

  3.2-)Install the dependencies before compiling:

      sudo apt install build-essential flex bison dwarves libssl-dev libelf-dev

  3.3-)Start the compiling process:

      make KCONFIG_CONFIG=Microsoft/config-wsl 

  3.4-)Place the linux headers with this command to /lib/modules directory:

      sudo make modules_install
