# Lab: system calls

在上一个实验中，您使用系统调用编写了一些实用程序。在本实验中，您将向 xv6 添加一些新的系统调用，这将帮助您了解它们的工作原理，并向您展示 xv6 内核的一些内部结构。您将在后面的实验中添加更多系统调用。

{% hint style="warning" %}  
在开始编写代码之前，请阅读 xv6 book的第2章、第4章的4.3和4.4节以及相关源文件：
- 将系统调用由用户空间转入内核的["stub"（桩）](https://en.wikipedia.org/wiki/Method_stub)位于`user/usys.S`，该代码在您运行`make`时，由`user/usys.pl`生成的。声明于`user/user.h`
- 将系统调用导向实现它的内核空间代码位于`kernel/syscall.c`和`kernel/syscal.h`中
- 进程相关的代码位于`kernel/proc`.h`与`kernel/proc.c`  
{% endhint %}

要开始实验，请切换好系统调用分支：
```bash
$ git fetch
$ git checkout syscall
$ make clean
```

如果你运行`make grade`，你会看到评分脚本不能执行`trace`和`sysinfotest`。你的任务就是添加必要的系统调用和桩来是他们正常工作。

## Using gdb <mark style="color:green;">(easy)</mark>

在许多情况下，`print`语句足以调试内核，但有时能够单步执行某些汇编代码或检查堆栈上的变量会很有帮助。

要了解有关如何运行 GDB 以及使用 GDB 时可能出现的常见问题的更多信息，请查看[此页面](https://pdos.csail.mit.edu/6.828/2023/labs/gdb.html)。

为了帮助您熟悉 gdb，请运行`make qemu-gdb`，然后在另一个窗口中启动 gdb（请阅读[Lab guidance](https://pdos.csail.mit.edu/6.828/2023/labs/guidance.html)（尚未翻译）的 gdb 项）。打开两个窗口后，在 gdb 窗口中输入
```bash
(gdb) b syscall
Breakpoint 1 at 0x80002142: file kernel/syscall.c, line 243.
(gdb) c
Continuing.
[Switching to Thread 1.2]

Thread 2 hit Breakpoint 1, syscall () at kernel/syscall.c:243
243     {
(gdb) layout src
(gdb) backtrace
```

`layout`命令将窗口分为两部分，显示 gdb 在源代码中的位置。`backtrace`打印出堆栈回溯。请阅读[Using the GNU Debugger](https://pdos.csail.mit.edu/6.828/2019/lec/gdb_slides.pdf)获得有用的 gdb 命令。

{% hint style="info" %}  
查看`backtrace`的输出，哪个函数调用了`syscall`？   
将回答记录在`answer-syscall.txt`中  
{% endhint %}

键入`n`数次来跳过`struct proc *p = myproc();`；跳过该语句后，键入`p /x *p`，该命令以16进制打印当前进程的`proc struct`（位于`kernel/proc.h`）。

{% hint style="info" %}  
`p->trapframe->a7`的值是多少，这个值代表了什么？（提示：查看`user/initcode.S`，这是 xv6 启动的第一个用户程序）  
将回答记录在`answer-syscall.txt`中  
{% endhint %}

处理器运行在内核模式，我们可以打印特权寄存器，例如`sstatus`（查阅 [RISC-V privileged instructions](https://github.com/riscv/riscv-isa-manual/releases/download/Priv-v1.12/riscv-privileged-20211203.pdf)）

```bash
(gdb) p /x $sstatus
```

{% hint style="info" %}  
此之前 CPU 处于什么模式？  
将回答记录在`answer-syscall.txt`中  
{% endhint %}


