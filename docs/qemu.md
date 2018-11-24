# QEMU

Wave can run under emulation ussing QEMU. QEMU can either be installed via
prebuilt binaries, or built locally.

## To build and test for ARM on linux

1. Install or build qemu. v2.4 and above is recommended.
[Download QEMU](https://www.qemu.org/download/#source)

2. Install gcc for embedded arm.
[Download Toolchain](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)

On Linux:

```
mkdir $HOME/bin
tar -xvf gcc-arm-none-eabi-7-2018-q2-update-linux.tar.bz2 -C $HOME/bin/
export PATH=$HOME/bin/gcc-arm-none-eabi-7-2018-q2-update/bin:$PATH
```

3. Run scripts/do-qemuarm  (from the wave directory)

```
./scripts/do-qemuarm
```

4. You should see 'Welcome to Wave/MP'

This will get you a interactive prompt into Wave which is running in qemu
arm machine 'virt' emulation. type 'help' for commands.

## To build and test for ARM-M on linux

1. Install or build pebble qemu.
[Pebble QEMU](https://github.com/wave-mirror/pebble_qemu)

```
export QEMUM4=pebble_qemu/arm-softmmu/qemu-system-arm
```

2. Install gcc for embedded arm-m.
[Download Toolchain](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)

3. Run scripts/do-qemum4  (from the wave directory)

```
./scripts/do-qemum4
```

4. You should see 'Welcome to Wave'

## Debugging the kernel with GDB

In the shell you're running QEMU in:

```
shell1$ ./scripts/do-qemuarm -d -D
```

This will start QEMU but freeze the system at startup,
waiting for you to resume it with "continue" in GDB.

And then in she shell you're running GDB in:

```
shell2$ arm-eabi-gdb lk.elf
GNU gdb (GDB) 7.10.1
Copyright (C) 2015 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "--host=x86_64-unknown-linux-gnu --target=arm-eabi".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from lk.elf...done.
(gdb) target remote :1234
Remote debugging using :1234
0x40010000 in ?? ()
(gdb) bt
#0  0x40010000 in ?? ()
(gdb) b lk_main
Breakpoint 1 at 0x80012b24: file top/main.c, line 79.
(gdb) c
Continuing.

Breakpoint 1, lk_main (arg0=0, arg1=0, arg2=0, arg3=0) at top/main.c:79
79	    lk_boot_args[0] = arg0;
(gdb) bt
#0  lk_main (arg0=0, arg1=0, arg2=0, arg3=0) at top/main.c:79
#1  0x800101d8 in platform_reset () at arch/arm/arm/start.S:275
Backtrace stopped: previous frame identical to this frame (corrupt stack?)
```
