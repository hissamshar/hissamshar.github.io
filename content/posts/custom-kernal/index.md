---
date: '2025-01-23T15:45:33+05:00'
title: 'How to Write a Custom Kernel Module'
---

# Linux Kernal
The majority of the kernel's code is written in C, leveraging extensions provided by the GNU Compiler Collection (GCC) beyond standard C. Additionally, it includes assembly code for architecture-specific functions, such as optimizing memory usage and task execution. Architecturally, the Linux kernel is monolithic, meaning the entire OS operates within kernel space. However, it features a modular design, allowing software components to be integrated as modules, including dynamic loading.

# What Is A Kernel Module?

A Linux kernel module is precisely defined as a code segment capable of dynamic loading and unloading within the kernel as needed. These modules enhance kernel capabilities without necessitating a system reboot. A notable example is seen in the device driver module, which facilitates kernel interaction with hardware components linked to the system.

# Writing a Custom Linux Kernel Module

Linux kernel modules (LKMs) allow developers to extend the functionality of the Linux kernel without modifying its source code. This guide walks through writing a simple kernel module from scratch. Kernel modules are pieces of code that can be dynamically loaded and unloaded from the Linux kernel at runtime. They enable functionality such as device drivers, file system support, and system call extensions without requiring a kernel recompilation. LKMs are particularly useful for developing hardware drivers and testing new kernel features without rebooting the system.

## Prerequisites

Ensure you have the necessary development tools installed. On an Arch Linux system, install them with:

```sh
sudo pacman -Syu linux-headers base-devel
```

## Creating a Simple Kernel Module

### 1. Writing the Module Source Code

Create a file named `hello_module.c`:

```c
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/init.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Your Name");
MODULE_DESCRIPTION("A simple Hello World kernel module");

static int __init hello_init(void) {
    printk(KERN_INFO "Hello, Kernel!\n");
    return 0;
}

static void __exit hello_exit(void) {
    printk(KERN_INFO "Goodbye, Kernel!\n");
}

module_init(hello_init);
module_exit(hello_exit);
```

### Understanding the Kernel Module Code

- `#include <linux/module.h>`: Includes the necessary module macros and functions.
- `#include <linux/kernel.h>`: Provides kernel logging functions.
- `#include <linux/init.h>`: Defines initialization and cleanup macros.
- `MODULE_LICENSE("GPL")`: Specifies the module's license.
- `MODULE_AUTHOR("Your Name")`: Specifies the author of the module.
- `MODULE_DESCRIPTION("A simple Hello World kernel module")`: Provides a brief description.
- `static int __init hello_init(void)`: The function executed when the module is loaded.
- `static void __exit hello_exit(void)`: The function executed when the module is unloaded.
- `module_init(hello_init)`: Registers `hello_init` as the module's initialization function.
- `module_exit(hello_exit)`: Registers `hello_exit` as the module's cleanup function.

### 2. Writing the Makefile

Create a `Makefile` in the same directory:

```makefile
obj-m += hello_module.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

### Understanding the Makefile

- `obj-m += hello_module.o`: Specifies that `hello_module.o` is the object to be built as a module.
- `all:`: Defines the build target.
- `make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules`: Directs the kernel build system to compile the module.
- `clean:`: Cleans up the generated files.
- `make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean`: Cleans the build artifacts.

### 3. Compiling the Module

Run:

```sh
make
```

### 4. Loading and Unloading the Module

To insert the module into the kernel:

```sh
sudo insmod hello_module.ko
```

Check the kernel log:

```sh
dmesg | tail
```

To remove the module:

```sh
sudo rmmod hello_module
```

### 5. Verifying the Module

List loaded modules:

```sh
lsmod | grep hello_module
```

## Understanding the Generated Files

After building the module, several files are generated:

- **hello_module.c**: The source code of the module.
- **hello_module.ko**: The compiled kernel module file, ready to be loaded into the kernel.
- **hello_module.o**: An intermediate object file generated during compilation.
- **hello_module.mod.c**: An automatically generated file containing module metadata.
- **hello_module.mod.o**: An object file containing metadata compiled from `hello_module.mod.c`.
- **hello_module.mod**: Another metadata file required for module loading.
- **Makefile**: Contains instructions for building the module.
- **Module.symvers**: Stores information about exported symbols, useful for module dependencies.
- **modules.order**: Lists the order in which modules should be loaded.

## Conclusion

This simple kernel module demonstrates the basics of module development. You can expand upon this by adding functionality such as handling parameters or interacting with hardware.

Happy kernel hacking!

