# 03 - 内存管理

## 虚拟内存与物理内存

> (2023 - T30)进程 R 和 S 共享数据 data，若 data 在 R 和 S 中所在页的页号分别为 p1 和 p2，两个页所对应的页框号分别为 f1 和 f2，则下列叙述中正确的是   
A. p1 和 p2 一定相等，f1 和 f2 一定相等  
B. p1 和 p2 一定相等，f1 和 f2 不一定相等  
C. p1 和 p2 不一定相等，f1 和 f2 一定相等  
D. p1 和 p2 不一定相等，f1 和 f2 不一定相等  

解答：

- 物理内存被组织称页框。页框的大小是 2 的幂，并且因系统而异
- 虚拟内存被组织成页。页面的大小与页框相匹配

注意：页框可能是不连续的，但页面是连续的。  
所以选择 C 项。