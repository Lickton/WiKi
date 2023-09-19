# Lab: Xv6 and Unix utilities

本次实验主要为了熟悉xv6与其系统调用

## Boot xv6

在合适的目录下`git clone`整个项目。

```bash
$ git clone git://g.csail.mit.edu/xv6-labs-2023
$ cd xv6-labs-2023
```

本次实验和后续实验作业所需的文件是使用Git版本控制系统分发的。对于每个实验，您都将查看专为该实验定制的 xv6 版本。要了解有关 Git 的更多信息，请查看 Git 用户手册，或者，您可能会发现此[面向CS的Git概述](https://eagain.net/articles/git-for-computer-scientists/)很有用。Git 允许您跟踪对代码所做的更改。例如，如果您完成了其中一项练习，并且想要检查您的进度，您可以通过运行以下命令来提交更改：

```bash
$ git commit -am 'my solution for util lab exercise 1'
```

您可以使用该命令跟踪您的更改`git diff` 。运行`git diff`将显示自上次提交以来代码的更改，并将显示相对于初始`utilgit diff origin/util`代码的更改。这里，`origin/util`是本实验的 `git` 分支的名称。

构建并运行xv6：
```bash
$ make qemu
riscv64-linux-gnu-gcc    -c -o kernel/entry.o kernel/entry.S
riscv64-linux-gnu-gcc -Wall -Werror -O -fno-omit-frame-pointer -ggdb -gdwarf-2 -DSOL_UTIL -DLAB_UTIL -MD -mcmodel=medany -ffreestanding -fno-common -nostdlib -mno-relax -I. -fno-stack-protector -fno-pie -no-pie  -c -o kernel/kalloc.o kernel/kalloc.c
riscv64-linux-gnu-gcc -Wall -Werror -O -fno-omit-frame-pointer -ggdb -gdwarf-2 -DSOL_UTIL -DLAB_UTIL -MD -mcmodel=medany -ffreestanding -fno-common -nostdlib -mno-relax -I. -fno-stack-protector -fno-pie -no-pie  -c -o kernel/string.o kernel/string.c
riscv64-linux-gnu-gcc -Wall -Werror -O -fno-omit-frame-pointer -ggdb -gdwarf-2 -DSOL_UTIL -DLAB_UTIL -MD -mcmodel=medany -ffreestanding -fno-common -nostdlib -mno-relax -I. -fno-stack-protector -fno-pie -no-pie  -c -o kernel/main.o kernel/main.c
riscv64-linux-gnu-gcc -Wall -Werror -O -fno-omit-frame-pointer -ggdb -gdwarf-2 -DSOL_UTIL -DLAB_UTIL -MD -mcmodel=medany -ffreestanding -fno-common -nostdlib -mno-relax -I. -fno-stack-protector -fno-pie -no-pie  -c -o kernel/vm.o kernel/vm.c
riscv64-linux-gnu-gcc -Wall -Werror -O -fno-omit-frame-pointer -ggdb -gdwarf-2 -DSOL_UTIL -DLAB_UTIL -MD -mcmodel=medany -ffreestanding -fno-common -nostdlib -mno-relax -I. -fno-stack-protector -fno-pie -no-pie  -c -o kernel/proc.o kernel/proc.c
riscv64-linux-gnu-gcc    -c -o kernel/swtch.o kernel/swtch.S
...
mkfs/mkfs fs.img README user/xargstest.sh user/_cat user/_echo user/_forktest user/_grep user/_init user/_kill user/_ln user/_ls user/_mkdir user/_rm user/_sh user/_stressfs user/_usertests user/_grind user/_wc user/_zombie user/_sleep user/_pingpong user/_primes
nmeta 46 (boot, super, log blocks 30 inode blocks 13, bitmap blocks 1) blocks 1954 total 2000
balloc: first 845 blocks have been allocated
balloc: write bitmap block at sector 45
qemu-system-riscv64 -machine virt -bios none -kernel kernel/kernel -m 128M -smp 3 -nographic -global virtio-mmio.force-legacy=false -drive file=fs.img,if=none,format=raw,id=x0 -device virtio-blk-device,drive=x0,bus=virtio-mmio-bus.0

xv6 kernel is booting

hart 2 starting
hart 1 starting
init: starting sh
$ 
```

现在就构建成功并在qemu运行xv6了，此时在提示符后键入`ls`，会有以下输出:
```bash 
$ ls
.              1 1 1024
..             1 1 1024
README         2 2 2305
xargstest.sh   2 3 93
cat            2 4 32824
echo           2 5 31704
forktest       2 6 15840
grep           2 7 36264
init           2 8 32168
kill           2 9 31616
ln             2 10 31440
ls             2 11 34768
mkdir          2 12 31680
rm             2 13 31672
sh             2 14 54248
stressfs       2 15 32552
usertests      2 16 180440
grind          2 17 47432
wc             2 18 33752
zombie         2 19 31040
sleep          2 20 31648
pingpong       2 21 32136
primes         2 22 32992
console        3 23 0
```

这些是`mkfs`包含在初始文件系统中的文件，大多是是可运行的程序，刚才运行的是其中的`ls`。

xv6没有`ps`命令，但若键入`Ctrl-p`，内核将打印每个进程的信息。如：

```bash
$ 
1 sleep  init
2 sleep  sh
```

要退出qemu，请键入`Ctrl-a`，松开后再键入`x`，即可退出。

## sleep (easy)

> 按照Unix `sleep` 命令的思路，为xv6实现用户级程序`sleep`。您的`sleep`程序应当暂停用户指定的时钟周期数。时钟周期是xv6内核定义的时间概念，即定时器芯片两次中断之间的时间。您的代码应该位于`user/sleep.c`中。

一些提示：
- 开始写代码之前，阅读xv6-book的第一章
- 将代码放入`user/sleep.c`中。查看`uesr/`中其他一些程序，如`user/echo.c`，以了解命令行参数如何传递给程序
- 完成代码编写后，将`_sleep`添加到`Makefile`中的`UPROGS`中，完成此操作后，`make qemu`将编译您的程序，可以在xv6 shell中运行它
- 若用户没有传递参数或多传递了参数，`sleep`应该打印一条错误消息
- 命令行参数作为字符串传递，您可以使用`atoi`将其转换为`int`，参考`user/ulib.c`
- 使用系统调用`sleep`
- 参考`kernel/sysproc.c`以了解实现`sleep`系统调用的xv6内核代码（找到`sys_sleep`），参阅`user/user.h`以了解用户程序调用`sleep`的C定义，以及`user/usys.S`以了解汇编代码从用户代码跳转到内核进行睡眠
- `sleep`的`main`函数完成后应该调用`exit(0)`

从xv6 shell运行程序：
```
$ make qemu
...
init: start sh
$ sleep 10
(暂停一会)
$
```
如果您的程序在运行时暂停，如上所示，您的程序是正确的。退出qemu之后运行`make grade`查看是否通过`sleep`测试。

请注意，`make grade`运行所有测试，若想测试某一项的测试，输入

```bash
$ ./grade-lab-util sleep
```

或

```bash
$ make GRADEFLAGS=sleep grade
```

## pingpong (easy)

> 编写一个用户级程序，使用 xv6 系统调用通过一对`pipe`（每个方向一个）在两个进程之间“pingpong”一个字节。父进程应向子进程发送一个字节；子进程应打印“\<pid\>：received ping”，其中 \<pid\> 是其进程 ID，将管道上的字节写入到父进程，然后退出；父进程应该从子进程读取字节，打印“\<pid\>：received pong”，然后退出。您的代码应该位于文件`user/pingpong.c`中。

一些提示：
- 将`_pingpong`添加到`Makefile`中的`UPROGS`
- 使用`pipe`创建管道
- 使用`fork`创建一个子进程
- 使用`read`从管道读取数据，使用`write`向管道中写入数据
- 使用`getpid`获取调用进程的进程ID
- xv6 上的用户程序具有一组有限的可用库函数。您可以在`user/user.h`中看到该列表。源代码（系统调用除外）位于`user/ulib.c`、`user/printf.c`和`user/umalloc.c`中。

在 xv6 shell 运行该程序，它应该产生以下输出：
```bash
$ make qemu
...
init: start sh
$ pingpong
4: received ping
5: received pong
$
```

如果您的程序在两个进程之间交换一个字节并产生如上所示的输出，则您的代码是正确的。

## primes (Moderate)/(Hard)

> 参考[Bell Labs and CSP Threads](https://swtch.com/~rsc/thread/)中Communicating Sequential Process小节中段[埃氏筛](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)的思路，为 xv6 编写一个并发素数筛的程序。这个思路源于Unix管道的发明者Doug McIlroy。您的代码应该位于`user/primes.c`

![](https://swtch.com/~rsc/thread/sieve.gif)

您的目标是使用`pipe`和`fork`来设置管道。第一个进程将数字2到35写入管道。对于每个质数，您应该创建一个新的子进程，通过管道从其坐邻居读取数据，并通过另一个管道向其右邻居写入数据，如上图所示。由于 xv6 的文件描述符和进程数量有限制，第一个进程应该停在35。

一些提示：
- 请小心关闭进程不需要的文件描述符，否则您的程序将在第一个进程达到 35 之前耗尽 xv6 的资源
- 一旦第一个进程达到 35，它应该等待整个管道终止，包括所有子进程等。因此，主 `primes` 进程仅应在所有输出打印完毕以及所有其他 `primes` 进程退出后退出
- 当管道的写入端关闭时，`read`返回`0`
- 最简单的方法是直接将 32 位（4 字节）int写入管道，而不是使用格式化的 ASCII I/O
- 您应该只在需要新管道时创建新管道
- 将程序添加到`Makefile`中的`UPROGS`

以下是译者添加的提示：
- 使用`wait`等待子进程退出
- `fork`复制父进程所有的文件描述符
- 在Linux系统中包含必要的头文件编写代码，尝试编译运行验证正确性
    - 使用Linux `time` 程序查看主进程何时推出以及程序运行时间

如果您的代码实现了基于管道的素数筛并产生以下输出，则它是正确的：
```bash
$ make qemu
...
init: start sh
$ primes
prime 2 
prime 3 
prime 5 
prime 7 
prime 11 
prime 13 
prime 17 
prime 19 
prime 23 
prime 29 
prime 31
$
```

