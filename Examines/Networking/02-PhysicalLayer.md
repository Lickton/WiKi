# 02 物理层

## 1 数据通信的理论基础

- 奈奎丝特定理：最大数据速率$=2\rm{B}\log_2V$
    - 单位：$bps$
    - $\rm{2B}$：一个任意信号通过一个带宽为$\rm{B}$的低通滤波器，那么只要进行每秒$\rm{2B}$次确切采样，就可以完全重构出被过滤的信号
    - $\rm{V}$：信号包含$\rm{V}$个离散等级
- 香农定理：最大比特率=$\rm{B}log_2{(1+\rm{S/N})}$
    - $\rm{B}$：带宽为$\rm{B}Hz$
    - $\rm{S/N}$：信噪比
        - $\rm{S}$：信号功率
        - $\rm{N}$：噪声功率

## 2 引导性传输介质

- 磁介质
    - 永远不要低估一辆满载着硬盘在高速公路上飞驰的旅行车的带宽
- 双绞线：由两根相互绝缘的铜线组成，不同电线产生的干扰波会相互抵消，显著降低电线的辐射
    - 非屏蔽双绞线
        - 全双工链路：可以双向同时使用的链路
        - 半双工链路：可以双向使用但一次只能使用一个方向
        - 单工链路：只允许在一个方向上传输的链路
    - 屏蔽双绞线
        - 未大量部署  
    ![双绞线](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/UTP_cable.jpg/330px-UTP_cable.jpg)
- 同轴电缆：硬的铜芯和外面包上的一层绝缘材料组成  
    ![同轴电缆](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/RG-59.jpg/330px-RG-59.jpg)
- 电力线
- 光纤
- 光缆

## 3 无线传输

- 电磁频谱
    - $\lambda f=c$
        - 频率：$f$
        - 波长：$\lambda$
    - 扩展频谱
        - 跳频扩频：发射器以每秒几百次的速率从一个跳到另一个频率
        - 直接序列扩频（**码分多址**）：使用了一个码片序列，并且将数据信号展开到一个很宽的频段上
            - CDMA: Code Division Multiple Access
        - 超宽带通信：发送一系列快速脉冲，随着通信信息而不断变换自己的位置
            - UWB：Ultra-WideBand
- 无线电传输
    - 路径损耗：无线电波的低频部分，在空间中传播时的信号能量至少以$1/r^2$衰减
- 微波传输
- 电磁频谱政策
- 红外传输
- 光通信

## 4 通信卫星

- 通信卫星

## 5 数字调制与多路复用

- 数字调制：在有线或无线信道上发送数字信息的解决方案
    - 数字调制：比特与代表它们的信号之间的转换过程
    - 基带传输：信号的传输占有传输介质从零到最大值之间的全部频率，从而最大频率取决于信令速率
        - 不归零编码（NRZ None-Return-to-Zero）：用正电压表示1，负电压表示0
            - 使用两个以上的信号级别，例如采用4个电压级别，可以使用单个符号一次携带2个bit，只要接收器收到的信号强度足够区分信号的4个级别
            - 符号率：信号改变的速率
            - 比特率：符号率$\times$每个符号的比特数  
            - 电话调制解调设备的应用中常见  
            ![](https://upload.wikimedia.org/wikipedia/commons/5/55/NRZcode.png)
        - 不归零逆转编码（NRZI None-Return-to-Zero Inverted）：信号跳变表示1，不变表示0
            - 也可以设定为信号不变表示1，信号跳变表示0
            - 既能传输时钟信号，又能尽量不损失系统带宽
            - USB 标准采用的编码  
            ![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3f/Nrzi_linecode.svg/315px-Nrzi_linecode.svg.png)
        - 曼彻斯特编码（Manchester）：将时钟信号与比特异或
            - 曼彻斯特编码提供一种同步机制，保证发送端与接收端信号同步
            - 时钟在每个比特时间内产生一次跳变，则是以两倍比特率的速度运行
            - 当时钟与0电压异或时，产生一次从低到高的跳变，表示逻辑0
            - 当时钟与1电压异或时，产生一次从高到低的相反转变，表示逻辑1
            - 主要用在经典以太网中  
            ![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/90/Manchester_encoding_both_conventions.svg/975px-Manchester_encoding_both_conventions.svg.png)  

            |原始数据| |时钟|        | 曼彻斯特值|  
            |--------|-|----|--------|-----------|  
            |    0   |=|  0 |$\oplus$|     0     |
            |    0   |=|  1 |$\oplus$|     1     |
            |    1   |=|  0 |$\oplus$|     1     |
            |    1   |=|  1 |$\oplus$|     0     |

        - 差分曼彻斯特编码（Differential Manchester）：依赖前一个比特的状态
            - 差分曼彻斯特编码在 每个时钟周期的中间都有一次电平跳变，这个跳变做同步之用。 在每个时钟周期的起始处：跳变则说明该比特是0，不跳变则说明该比特是1
            - 差分编码：在数据传输中，当使用某些类型的调变时微分编码能提供明确的讯号接收的一种技术，可以让数据传输不仅仅依赖当前的讯号，而是会让前一个讯号影响传输的数据
            - 在令牌环局域网的IEEE 802.5标准中指定，并用于许多其他应用，包括磁存储和光存储。
            ![](https://media.geeksforgeeks.org/wp-content/uploads/20220908122428/DifferentialMachesterEncoding-660x330.jpg)
    - 通带传输：信号占据了以载波信号频率为中心的一段频带，任意一个频率的波段都可以用作信号传递（频率并不是从零开始的，对于无线信道来说发送非常低的信号很不实际，因为天线大小与信号波长成反比）
        - 幅移键控（ASK Amplitude Shift Keying）：通过两个不同的振幅分别表示0和1
        - 频移键控（FSK Frequency Shift Keying）：通过两个或更多的频率分别表示0和1
        - 相位键控（PSK Phase Shift Keying）：每个符号的周期内，系统把载波波形偏移一定程度
            - 二进制相移键控（BPSK Binary Phase Shift Keying）：使用两个偏移，$0$ 和 $\pi$
            - 正交相移键控（QPSK Quadrature Phase Shift Keying）：使用四个偏移，$\frac{\pi}{4}$、$\frac{3\pi}{4}$、$\frac{5\pi}{4}$和$\frac{7\pi}{4}$
            ![](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ad/Pi-by-O-QPSK_Gray_Coded.svg/330px-Pi-by-O-QPSK_Gray_Coded.svg.png)
- 多路复用：信道被多个信号共享，使多个信号共享传输线路的解决方案
    - 频分复用（FDM Frequency Division Multiplexing）：利用通带传输的优势使多个用户共享一个信道。将频谱分为几个频段，每个用户完全拥有其中一个频段，不同频段之间间隔较大
        - FDM 方案曾经被电话系统用来复用电话呼叫多年，现已被取代。电话网、蜂窝电话、店面无线和卫星网络仍在使用更高粒度的 FDM
        - 正交频分复用（OFDM Orthogonal Frequency Division Multiplexing）：信道带宽被分许多独立发送数据的子载波（例如 QAM）
    - 时分复用（TDM Time Division Multiplexing）：用户以循环的方式轮流工作。每个用户周期性地获得整个带宽非常短的一个时间
        - 这种工作方式要求输入流在时间上必须同步
        - 保护时间：类似于频率保护带，为了适应时钟的微小变化须需要增加的时间间隔
        - 统计时分复用（STDM Statistical Time Division Multiplexing）：“统计”表明组成多路复用流的各个流没有固定的调度模式，而是根据其需求产生。STDM是包交换的另一个代名词
    - 码分复用（CDM Code Division Multiplexing）：是扩宽频谱的另一种方式。这种方法更能容忍干扰；并且允许来自不用用户的多个信号共享相同的频带，又被称为码分多址（CDMA Code Division Multiple Access）
        - CDMA 的关键在于：能够提取出期望的信号，同时拒绝所有其他的信号，并把这些信号当作噪声
        - 码片：每个比特被再细分为 m 个更短的时间间隔，每个时间间隔被称为码片。m 通常为 64 或 128
        - 码片序列：每个站被分配得到的唯一的 m 位码
        - 正交性：所有码片序列都两两正交
            - $S\cdot T=\overline{S}\cdot\overline{T}=\frac{1}{m}\sum S_iT_i=0$
            - $S\cdot S=\frac{1}{m}\sum S_iS_i=\frac{1}{m}\sum(\pm1)^2=1$
            - $S\cdot \overline{S}=\frac{1}{m}\sum S_i\overline{S_i}=\frac{1}{m}\sum{(1\times-1)}=-1$
        - 若要发送1，站就发送它的码片序列，若要发送0，则发送其反码
        - 所有站的发送必须要是同步的
        - 除了蜂窝网络，CDMA 还被用于卫星通信和有线电视网络

## 6 公共电话交换网络

- 局外部分：本地回路和中继线，根据地理位置来看位于交换局外部
    - 数字化语言信号
        - 脉冲编码调制（PCM Pulse Code Modulation）
        - 压缩扩展：信号的动态范围在均匀量化之前就被压缩，后在模拟信号被还原时再被扩展恢复
    - 时分多路复用（TDM）：T1 载波：包含 24 条被复用在一起的语言信道，每个信道依次将 8 bit 的样值插入输出流
        - 抢占比特信令：将每 6 个帧中每个信道 8 个比特样值中最低有效位取出，因为从语音通话中偷出几位是无关紧要的
        ![](https://www.dcbnet.com/notes/9611t11.gif)
    - 波分多路复用（WDM Wavelength Division Multiplexing）：极高频率上的频分多路复用
        - 光纤 WDM：4 条光纤汇合到一个光纤组合器上，然后传输给远方的接收方。在远端，这束光又被分离到与输入端一样多的光纤上  
        ![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9b/WDM_operating_principle.svg/600px-WDM_operating_principle.svg.png)
- 局内部分：交换机，位于交换局内部
    - 交换：传统的电话系统基于电路交换技术
        - 电路交换：当电话或计算机发出电话呼叫时，电话系统的交换设备就会寻找一条从电话到接收方电话的物理路径的技术
        - 分组交换：数据包尽可能地被发出，路由器使用存储-转发技术
            - 排队延迟：分组交换中没有为传输数据预留带宽，可能需要等待一会才会被发送

