---
date: '2025-08-01T00:06:37+05:00'
title: 'Detecting and Fixing Memory Leaks with Valgrind'
categories: ["Linux"]
tags: [ "Memory"]
draft: false
---



## Introduction

A **memory leak** occurs when a program allocates memory dynamically (e.g., using `malloc`) and fails to release it using `free`. This leftover allocation can lead to wasted memory resources, eventually causing slowdowns or system crashes in long-running programs.

**Valgrind** is a powerful command-line tool available on Linux systems. It helps developers detect:
- Memory leaks
- Invalid memory access
- Uninitialized memory usage
- Mismatched memory management

In this guide, we'll walk through examples in C to learn how to detect and fix memory leaks using Valgrind.

---

## Installing Valgrind

### On Arch Linux
```sh
sudo pacman -S valgrind
```

### On Ubuntu/Debian
```sh
sudo apt install valgrind
```

### On Fedora
```sh
sudo dnf install valgrind
```

## Troubleshooting: Valgrind "cannot find mandatory redirection" on Arch Linux

If you run Valgrind and get an error like:

**valgrind:  Fatal error at startup: a function redirection**

**valgrind:  which is mandatory for this platform-tool combination**

**valgrind:  cannot be set up.**

…you might be running a **32-bit executable** on a 64-bit Arch Linux system.

## Why this happens
Valgrind needs to hook into low-level `glibc` functions from your binary’s architecture.  
If your binary is **32-bit**, Arch requires the **32-bit glibc runtime** (`lib32-glibc`).  
Without it, Valgrind can’t find the right function symbols and quits.

## Fix
Install the 32-bit glibc package:
```bash
sudo pacman -S lib32-glibc
```

After installation, re-run:
```bash
valgrind ./your-binary
```
and it should work.

> **Ubuntu/Debian equivalent:** `sudo apt install libc6-dbg:i386`

---


## Basic Example: Hello World

### Code
```c
#include <stdio.h>

int main() {
    printf("Hello World\n");
    return 0;
}
```

### Compile and Run
```sh
gcc main.c -o main.out
./main.out
```

### Run with Valgrind
```sh
valgrind ./main.out
```
![](/valgrind.png)

You should see no errors or memory leaks in the output.

---

## Introducing a Memory Leak

### Static Allocation (Safe)
```c
#include <stdio.h>

int main() {
    char str[20] = "Hello";
    printf("%s\n", str);
    return 0;
}
```
This uses stack memory, so Valgrind will report no leaks.

### Dynamic Allocation (With Leak)
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char *str = malloc(20);
    strcpy(str, "Hello");
    printf("%s\n", str);
    return 0; // Forgot to free memory
}
```

### Valgrind Output
```sh
valgrind ./main.out
```
You should see:

![](/valgrind2.png)

definitely lost: 20 bytes in 1 blocks


---

## Fixing the Memory Leak

Add `free(str);` before returning:
```c
free(str);
```

### Fixed Code
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char *str = malloc(20);
    strcpy(str, "Hello");
    printf("%s\n", str);
    free(str);
    return 0;
}
```

---

## Detailed Valgrind Options

Use for deeper analysis:
```sh
valgrind --leak-check=full ./main.out
```

Or even more detailed:
```sh
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./main.out
```

Explanation:
- `--leak-check=full`: Display detailed leak info
- `--show-leak-kinds=all`: Show all kinds of leaks (definitely, indirectly lost, etc.)
- `--track-origins=yes`: Show where uninitialized values originate

---

## Summary

- Always `free()` memory allocated with `malloc()`, `calloc()`, or `realloc()`.
- Close all file streams with `fclose()`.
- Use Valgrind to identify and fix:
  - Memory leaks
  - Invalid memory writes
  - Use-after-free bugs
- Recommended command:
  ```sh
  valgrind --leak-check=full --track-origins=yes ./your_program
  ```

Valgrind is a critical tool for writing safe, efficient, and bug-free C programs.

