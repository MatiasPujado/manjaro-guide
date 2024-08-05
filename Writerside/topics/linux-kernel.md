# Linux Kernel

The Linux kernel is the core of the operating system. It is responsible for managing hardware resources, running processes, and providing essential services to applications. The
kernel is the first piece of software that runs when the system is booted and is responsible for initializing the hardware and starting the rest of the operating system.

## Identifying the Kernel being used

This terminal command will give your system’s kernel information:

```Bash
❯ mhwd-kernel -li
Currently running: 6.6.40-1-MANJARO (linux66)
The following kernels are installed in your system:
* linux66
```

As seen in the above example, Manjaro is running kernel 6.6.40-1-MANJARO. The information given here is not arbitrary; each part of the kernel name identifies something about that
kernel:

- The 6 indicates the version
- The 6 indicates the major revision
- The 40 indicates the minor revision
- The 1 indicates the revision of the Manjaro package
- MANJARO indicates the specific distribution it is used for

## Adding New Kernels

When listing a new kernel to be installed in the command, it is unnecessary to write the entire version number. For example, any version of Kernel 4.19 can be listed simply
as `linux419`, and any version of Kernel 4.14 can be listed as `linux414`, and so on.

The optional `rmc` (remove current) component is of vital importance. Using this will result in your existing kernel being deleted upon the installation of the new kernel.
Otherwise, if it is not used, then the existing kernel will be kept and will be selectable alongside the new kernel at the boot screen. It is recommended, especially if updating
to the latest bleeding-edge kernel, to keep your old one, even if only for a short time afterward. This is the safer option, and the old kernel can be easily removed when satisfied
with the stability and functionality of the new one.

As an example, once the terminal is opened, the following command will install a new kernel (6.6) without deleting the existing kernel currently being used:

```Bash
sudo mhwd-kernel -i linux66
```

Otherwise, the following command will install a new kernel (6.9) to replace the existing kernel, which will be deleted:

```Bash
sudo mhwd-kernel -i linux69 rmc
```

Either way, Manjaro will automatically configure the new kernel for you, ready for immediate use. Once completed, close the terminal and re-boot the system for the change to take
effect.


## Removing Kernels

Where multiple kernels are present on your system, pacman can be used to remove them in the terminal. It may be necessary to delete a total of three elements of the kernel in total
to completely remove it:

- The kernel itself
- The kernel's headers
- The kernel's extra modules

Whether the headers and extra modules must be deleted depends on whether they have been installed.

1. To remove a kernel, use the following syntax: 

```Bash
sudo mhwd-kernel -r linux[version]
```

Here is an example of removing kernel 5.0.17–1

```Bash
sudo mhwd-kernel -r linux50
```

2. To delete a kernel’s headers, the syntax is:

```Bash
sudo pacman -R linux[version]-headers
```

For example, to delete the headers of kernel version 5.0.x from the system, the following command would be entered:

```Bash
sudo pacman -R linux50-headers
```

3. To delete a kernel’s extra modules, the syntax is:

```Bash
sudo pacman -R linux[version]-extramodules
```

For example, to delete the extra modules of kernel version 5.0.x from the system, the following command would be entered:

```Bash
sudo pacman -R linux50-extramodules
```

4. To delete all elements of a kernel at the same time, where they are all present on your system, the syntax is:

```Bash
sudo pacman -R linux[version] linux[version]-headers linux[version]-extramodules
```

For example, to completely remove all elements of kernel version 5.0.x, the following command would be entered:

```Bash
sudo pacman -R linux50 linux50-headers linux50-extramodules
```

Please note, however, that attempting to delete multiple elements at once if they are not present on your system will result in an error message before the operation itself is
aborted. It is also valuable noting if Manjaro is being run in a virtual machine (e.g., Oracle Virtualbox), you may not be able to delete certain kernels if they contain elements
important to the virtualization process itself.
