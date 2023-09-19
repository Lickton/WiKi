# 配置实验环境

本次实验需要RISC-V版本的工具：QEMU 5.1+, GDB 8.3+, GCC 等等

## 安装

### Arch Linux

其中，`qemu-emulators-full` 可以由`qemu-full` 代替

```bash
$ sudo pacman -S riscv64-linux-gnu-binutils riscv64-linux-gnu-gcc riscv64-linux-gnu-gdb qemu-emulators-full
```

### Debian or Ubuntu

```bash
$ sudo apt-get install git build-essential gdb-multiarch qemu-system-misc gcc-riscv64-linux-gnu binutils-riscv64-linux-gnu
```
