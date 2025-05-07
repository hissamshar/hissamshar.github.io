---
date: '2025-05-08T02:26:10+05:00'
title: 'How to build Linux Kernal: Step by Step Guide'
---

# Building the Linux Kernel

**Compiling the Linux Kernel involves multiple steps and can take some time depending on your hardware specifications.**

---

### Step 1: Download the Kernel Source Code

Start by visiting the [Official Linux Kernel Website](https://www.kernel.org/) and downloading the latest available kernel source code. The downloaded file will be a compressed archive containing all necessary source files.

---

### Step 2: Extract the Source Code

Once the download completes, extract the contents of the compressed archive using the `tar` command:

```bash
tar xvf linux-6.13.tar.xz
```

> If the `tar` utility is not installed on your system, you can install it using:

```bash
sudo pacman -S tar
```

**Note:** Always ensure you are using the correct version number in the file name.

---

### Step 3: Install Required Dependencies

To compile the kernel, you need to install various development tools and libraries. Install them using the following command:

```bash
sudo pacman -S git fakeroot ncurses xz bc flex bison base-devel kmod cpio perl binutils util-linux jfsutils e2fsprogs xfsprogs squashfs-tools quota-tools
```

---

### Step 4: Configure the Kernel

1. Navigate into the kernel source directory:

```bash
cd linux-6.13
```

2. Use your current system’s configuration as a base:

   - If `zcat` is available, run:
     ```bash
     zcat /proc/config.gz > .config
     ```

   - Otherwise, use this alternative method:
     ```bash
     cp /proc/config.gz ./
     gunzip config.gz
     mv config .config
     ```

3. Customize the kernel using a menu-driven interface:
   ```bash
   make menuconfig
   make xconfig
   make oldconfig
   ```

4. Modify the `.config` file directly:

   - Open it with a text editor:
     ```bash
     sudo vim .config
     ```

   - Search for the line:
     ```
     CONFIG_EXT4_FS=m
     ```
     And change it to:
     ```
     CONFIG_EXT4_FS=y
     ```

---

### Step 5: Compile the Kernel

1. Determine the number of CPU cores available to speed up compilation:

```bash
nproc
```

2. Compile the kernel using the number of cores found above. Replace `n` with that number:

```bash
make -j<n>
```

> If you encounter any errors during or after this step, back up your `.config` file and reset the source tree with:

```bash
make mrproper
```

This command cleans the build environment and restores the source tree to its original state.

---

### Step 6: Install Kernel Modules

Kernel modules are essential for extending the kernel’s functionality and ensuring compatibility with various hardware. Install them with:

```bash
sudo make modules_install
```

---

### Step 7: Install the Kernel

You can install the compiled kernel using one of the two methods below:

1. **Automatic installation:**

```bash
sudo make install
```

2. **Manual installation (if the above doesn't work):**

   - Copy the kernel image:
     ```bash
     sudo cp arch/x86/boot/bzImage /boot/vmlinuz-linux-custom
     ```

   - Copy the System.map file:
     ```bash
     sudo cp System.map /boot/System.map-linux-custom
     ```

   - Copy the kernel configuration file:
     ```bash
     sudo cp .config /boot/config-linux-custom
     ```

---

### Step 8: Update the Bootloader

If you use **GRUB**, follow these steps to add an entry for your custom kernel:

1. Find the UUID of your root partition:

```bash
lsblk -f
```

2. Open the custom GRUB configuration file:

```bash
sudo nvim /etc/grub.d/40_custom
```

3. Add the following entry (replace `paste-your-root-partition-uuid-here` with the actual UUID):

```bash
menuentry 'Custom Linux Kernel' {
    linux /boot/vmlinuz-linux-custom
    root=UUID=paste-your-root-partition-uuid-here
    initrd /boot/initramfs-linux.img
}
```

---

### Step 9: Generate Initramfs

As you've compiled a new kernel, installed modules, and modified boot entries, generating a new initramfs is necessary. Run:

```bash
sudo mkinitcpio -k 6.13-custom -c /etc/mkinitcpio.conf -g /boot/initramfs-linux-custom.img
```

> Make sure the version (`6.13-custom`) matches your compiled kernel.

---

### Step 10: Update GRUB Configuration

Finally, update the GRUB configuration so that it includes your new kernel entry:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

---

### Done!

**Congratulations! You’ve successfully compiled and installed your custom Linux Kernel. Enjoy your personalized system!**

