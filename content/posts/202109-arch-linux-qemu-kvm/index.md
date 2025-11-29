---
title: "Arch Linux installation in hypervisor through QEMU/KVM"
description: ""
summary: ""
categories: ["Linux", "Arch Linux"]
tags: ["HyperVisor", "Linux"]
#externalUrl: ""
date: 2021-09-09
draft: false
---

Kernel-based Virtual Machine is a free and open-source virtualization module in the Linux kernel that allows the kernel to function as a hypervisor.

## Installation

For updates, run the following command:

```
$ sudo pacman -Syu
```

### QEMU/KVM installation:

We'll install qemu and all the utils required:

```
$ sudo pacman -S qemu vde2 ebtables iptables-nft nftables dms masq bridge-utils ovmf swptm
```

### Virtual Machine Manager installation:

The virt-manager application is a graphical user interface for managing virtual machines through libvirt. It primarily targets KVM VMs.

```
$ sudo pacman -S virt-manager
```

Now everything is set to work. We can move towards downloading archlinux .iso file.

## Download .iso file:

1. Head towards: [https://archlinux.org/download/](https://archlinux.org/download/)
2. Scroll through and look for the server closest to you.
3. Download [archlinux-2024.10.01-x86_64.iso](https://pkg.adfinis-on-exoscale.ch/archlinux/iso/2024.10.01/archlinux-2024.10.01-x86_64.iso) file.

## Setting up:

Open terminal and run the following command:

```
$ virt-manager
```

You will see an interface similar to this:

![Interface Screenshot](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/gscreenshot_2024-10-17-154132.png?w=824)

- Click on *'create a new virtual machine'* (option with star).

![Step 2](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/step-2.png?w=826)

- Select *'Local install media'*.

![Step 3](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/step-3.png?w=827)

- Browse to your *'archlinux-2024.10.01-x86_64.iso'*. 

![Step 4](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/4.png?w=829)

- Add your desired VM configuration and create a disk image.

### Boot Menu:

You will be prompted to a boot menu.

![Boot Menu](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/5.png?w=1024)

- Select the topmost option to start the installation process.

### Archlinux Installer:

You will be prompted to a terminal. The first step is to check if you are connected to the internet.

![Terminal Screenshot](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/6.png?w=1024)

Run:

```
# ip addr show
```

If it shows an IP address and says 'UP', that means you are good to go.

![IP Check](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/9.png?w=1024)

#### If not:

You will need to connect to the internet using the 'iwctl' method for Wi-Fi.

```
# iwctl
```

To search networks in your vicinity:

```
[iwd]# station [your_wifi_interface] get-networks
```

Get the name of the network you want to connect to. Exit from this prompt using **'exit'**.

To connect to the desired Wi-Fi network, run:

```
# iwctl --passphrase "[wifi_password]" station [your_wifi_interface] connect [wifi_name]
```

You can again run `ip addr show` to check if you are connected to the network.

Now you can run the installation command. We'll be using the **archinstall** method.

```
# archinstall
```

You will be prompted to an interface similar to this:

![Archinstall Screenshot](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/10.png?w=828)

We will install Arch using this interface. Go through each option:

- **Archinstall language**: Choose your preferred language.
- **Mirrors**: Select the mirror region closest to you. Use '/' to search.
- **Locales**: Set language and keyboard layout.
- **Disk configuration**: Choose *Best-effort default partition* to format the system.
- **Bootloader**: Use the default 'Grub' option.
- **Swap**: Select *Swap on zram* (default).
- **Hostname**: Leave as it is.
- **Root password**: Set the password for sudo/root privileges.
- **User account**: Set up a user account.
- **Profile**: Select *Desktop*. It includes essential packages. Others include *Minimal*, *Server*, and *Xorg*.

![Partitioning Screenshot](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/11.png?w=1024)

In *Desktop*, select your desktop environment. We'll use **Gnome** for simplicity.

![Desktop Environment](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/13.png?w=1024)

- **Audio**: Use **PipeWire** (default) or **PulseAudio**.
- **Kernels**: Use the **linux** kernel.
- **Additional packages**: Install any required packages.
- **Network Configuration**: Use **NetworkManager** for a GUI in Gnome.
- **Timezone**: Set the timezone closest to you and enable time sync.

Press *Install*. **Congratulations! You've successfully installed Arch Linux.**

![Success](https://hissamshar.wordpress.com/wp-content/uploads/2024/10/15.png?w=1024)

