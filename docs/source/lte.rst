.. _lte:

================
LTE 物理层
================

.. contents:: :local:


.. _introduction:

简介
------------
本文介绍 **Long Term Evolution (LTE)** 的无线接入技术以及 **Physical Layer (PHY)**，这里主要讲解一下关于 LTE 的物理层 OFDM 相关知识点，了解其底层设计的基本结构。


.. _related:

相关知识
__________

- **空中接口 (Air Interface)：** LTE采用的是基于 **Orthogonal Frequency Division Multiplexing Access (OFDMA**) 技术的无线多址接入。其下行采用OFDM，上行采用与之相类似的 **Single-Carrier Frequency Division Multiplexing (SC-FDM)**。相比之前的多址接入技术，OFDMA具有抗多径衰落、支持MIMO、频率选择等诸多优势。

- **频谱带宽 (Frequency Bands)：** LTE频谱带宽被3GPP所规定，**Frequency-division Duplex (FDD)** 与 **Time-division Duplex (TDD)**，频分双工与时分双工的频带资源如下 (1-43)：
**FDD**:

- **单播与多播 (Unicast and Multicast Services)：** 在LTE中单播是指数据只传输给一个用户，与之相对应的多播 **Multimedia Broadcast/Multicast Services (MBMS)** 一般是指电视、广播以及视频流等数据的传输，其传播都有自己专用的信道与系统。

- **带宽分配 (Allocation of Bandwidth) ：** 大家看到上图中的频谱分配后，不由会想每一段的频谱带宽是如何决定与分配的，比如1号FDD的分配，其分配带宽为60MHz，在这60MHz里面又是如何分配的。这里不得不提及一下几点常识，在LTE物理层中，一个资源块 **Physical Resource Block (PRB)** 带宽为180KHz，其中包含了12个宽带为15KHz的子载波。由IMT-advanced规定了比较灵活的带宽分配，范围为1.4MHz-20MHz，其包含的资源块如下：除了1.4MHz的占用率为77%外，其它频谱占用率达到了90%，之所以不占满是因为有保护频段，防止频谱泄漏。

- **时域分帧 (Time Framing)：** 在LTE中时间轴上被进行分帧处理，这样有利于信道的估计以应对时变的信道。一帧 (frame) 时长为10ms被分成了10个1ms的子帧 (subframe)，每一个子帧又被分为0.5ms的两个时隙 (time slots)，每一个时隙包含了6或者7个**OFDM符号**，这里需要强调，对后面理解什么叫OFDM符号有帮助。至于为什么这么分，都是协议的规定，如果以后的发展需要更新，那么将随之变化。具体如下图所示：

- **时频域的映射 (Time–Frequency Representation) ：** 理解OFDM符号是如何被传输的，其理解该符号是如何被映射到时频域资源的，这一点是非常重要的。信号在经过编码，星座图映射以后变成一个复值信号，此时将会映射到所谓的时频坐标系，该坐标系横坐标是时间，纵坐标是频率。这一步映射相当于是分配好每个信号的资源。

上图很好地说明了信号是如何被映射到时频域的。这里一个PRB是指在一个时隙内的180KHz频谱资源，也就是12个子载波持续0.5ms。这里的 Resource Element 指的是复值的调制信号。上图每个时隙包含了7个OFDM符号，因此一个资源块将有12*7，84个 Resource Element，资源块是LTE中传输的最小单位。值得说明的是为什么选择15KHz为子载波的间隙 (Subcarrier Spacing)，这是因为15KHz很好的符合了OFDM的指令，将衰落信道转化为一系列可分辨的平坦信道，大大提高了系统的抗衰落能力。此外，在上行链路中，子载波在载波中心频率两边，相反在下行中与载波中心频率一致的子载波不会被使用 (涉及到过高的干扰问题)，具体如下图：
In the uplink:

- **OFDM的多子载波传输 (OFDM Multicarrier Transmission)：** 我们知道LTE的上下行是基于OFDM多址技术的，这是一种多子载波传输的方法，这里理解它是如何传输的，有助于我们理解一个OFDM符号到底是什么。我将介绍一个OFDM符号 (symbol) 是如何产生的：
  - **step 1**. 首先将星座图映射后 (比如QAM) 的复值信号，也就是上述提到的 Resource Element 映射到我们的时频资源栅格上，将这些符号分配好时频资源。大家可能想问那么每个子载波的频率是多少呢？毕竟我只知道每个子载波的间隔是15KHz。如果有 $N$ 个间隔为 $\Delta f$ 的子载波，则有：
$$
B W=N\Delta f
$$
每个子载波的频率 $f_k$ 可以被看做是频率间隔 $\Delta f$ 的整数倍，即：
$$
f_k=k\Delta f
$$
  - **step 2**. 我们知道每一个经过星座图映射之后符号 $a_k$ 都是一个复数 $a_k=b+jc$ 信号 (QAM)，接下过经过OFDM调制器分别对这些信号进行调制，即是将这些复数信号分别乘以复数子载波，每一个符号被分配一个单独的子载波，即：
$$
x(t)=\sum_{k=1}^{N} a_{k} e^{j 2 \pi f_{k} t}=\sum_{k=1}^{N} a_{k} e^{j 2 \pi k \Delta f t}
$$
实际操作是离散的OFDM表示，这里需要假设信道的采样率为 $F$，则采时间为 $T=1/F$，采样点数为 $N$ ，假设符号周期为 $T_s=NT$ 则有：
$$
x(n)=\sum_{k=1}^{N} a_{k} e^{j 2 \pi k\Delta f n/N}
$$
可以看到上述表示可以很自然使用IFFT变换实现，这也是使用OFDM如此广泛的原因。我们也可以从表示看出一个OFDM符号是包含目前所有已经映射好的符号 $a_k$ 的调制求和。
  - **step 3**. 在经过OFDM调制后，一个OFDM符号就已经生成了，最后还需要给每一个符号加上一个循环前缀，其实就是将该符号的后面一部分复制到前面以消除符号间干扰 (ISI)和子载波间干扰 (ICI)。当然在进行完上述过程后，我们最后还需要将信号搬移到高频再发射出去。这里我想说一下每一个OFDM符号的时域持续时间，它是由两部分构成的循环前缀加上信号周期，信号周期 $1/15000s$，即是，大约66.7$\mu s$。完整过程可以由下图表示：

这里想多说明的一点是OFDM是如何区分不同的用户的，其实它正交的子载波就已经说明了这个问题。举个例子，我们以下行为例，基站根据用户上报的不同信道信息，给每个用户分配不同的时频元素 (前面已经说过，按照资源块为单位划分，减少信令的开支)，当每个用户获取划分的方式之后，收到基站发送的信息只需要解调属于自己的那一块信息。
 - **循环前缀 (Cyclic Prefix)：** 大家可以看到循环前缀有不同的大小，由于存在多径效应而导致的符号间干扰，同时为保证子载波之间的正交性，前缀是符号尾部的一段复制。LTE协议中按照下图规定了循环前缀的长度：

 **频域调度 (Frequency-Domain Scheduling)：** 频域的调度是LTE中很重要的一点，由于LTE本身支持不同的频率带宽，OFDM可以根据IFFT和FFT选择不同的符号长度，变化的点数。尽管LTE并没有规定带宽与FFT长度之间的关系，但一般2048与20MHz相关联，其他分配如下图：

.. _reference:

参考
__________

- Zarrinkoub, Houman. Understanding LTE with MATLAB: from mathematical modeling to simulation and prototyping. John Wiley & Sons, 2014.