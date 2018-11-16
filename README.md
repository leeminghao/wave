# Wave

The Wave embedded kernel.

## Builds

### To build and test for ARM on linux

1. Install or build qemu. v2.4 and above is recommended.
[Download QEMU](https://www.qemu.org/download/#source)

2. Install gcc for embedded arm (see note 1)

note 1: for ubuntu:
```
sudo apt-get install gcc-arm-none-eabi
```
or fetch a prebuilt toolchain from
http://newos.org/toolchains/arm-eabi-5.3.0-Linux-x86_64.tar.xz

3. Run scripts/do-qemuarm  (from the wave directory)
4. You should see 'Welcome to Wave/MP'

This will get you a interactive prompt into Wave which is running in qemu
arm machine 'virt' emulation. type 'help' for commands.

This page is a non-comprehensive index of the wave documentation.

+ [Relationship with LK](docs/wave_and_lk.md)
