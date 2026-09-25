
在无线 RF（射频）里，**EVM = Error Vector Magnitude，误差矢量幅度**。

它是衡量**调制信号质量**的一个非常重要的指标。简单理解：

> **EVM 表示“实际收到/发射出来的 IQ 信号点，偏离理想 IQ 信号点有多远”。**

对于你最近在排查的 **5G Wi-Fi / QCA 芯片 RF 问题**，EVM 是判断 **TX 射频链路质量**非常常用的指标。

---

# 1. 先用一个最直观的图理解

假设使用 QAM 调制。

理想情况下，星座图上的点应该非常准确：

```text
        ●       ●

        ●       ●
```

实际 RF 信号会受到很多因素影响，比如：

* PA 非线性
* IQ imbalance
* 相位噪声
* 频率偏移
* 噪声
* DAC/ADC 误差
* 电源噪声
* LO 泄漏
* RF 前端失真

所以实际收到的点可能变成：

```text
        ·●·     ·●·
          ·●    ●·

        ·●·     ·●·
```

每个实际点和理想点之间都有一个误差向量：

```text
理想点 ●
       ↑
       │  error vector
       │
       ● 实际点
```

这个向量的大小就是 **Error Vector**。

把所有符号的误差综合起来，就得到 **EVM**。

---

# 2. EVM 到底怎么算？

可以把理想 IQ 点记为：

$$
S_{ideal}=I_{ideal}+jQ_{ideal}
$$

实际测量到：

$$
S_{measured}=I_{measured}+jQ_{measured}
$$

那么误差向量：

$$
E=S_{measured}-S_{ideal}
$$

EVM 通常表示为 RMS：

$$
EVM_{RMS}
=
\sqrt{
\frac{
\sum |S_{measured}-S_{ideal}|^2
}{
\sum |S_{ideal}|^2
}
}
$$

然后通常转换成百分比：

$$
EVM(\%) = EVM_{RMS}\times100\%
$$

也可以转换成 dB：

$$
EVM(dB)=20\log_{10}(EVM)
$$

---

# 3. EVM 越大越好还是越小越好？

**越小越好。**

例如：

|  EVM | 大致含义   |
| ---: | ------ |
|   1% | 非常好    |
|   2% | 很好     |
|   3% | 较好     |
|   5% | 一般     |
|   8% | 比较差    |
| 10%+ | 明显存在问题 |

不过这里不能简单拿一个数字判断好坏，因为**不同调制方式、标准和测试条件要求不同**。

比如：

* 802.11n
* 802.11ac
* 802.11ax
* 802.11be

以及：

* BPSK
* QPSK
* 16-QAM
* 64-QAM
* 256-QAM
* 1024-QAM
* 4096-QAM

要求都不一样。

---

# 4. 为什么 RF 工程师经常看到 EVM 是负数？

这是刚接触 RF 时非常容易疑惑的地方。

例如你可能看到：

```text
EVM = -32 dB
```

不要以为这是“负的误差”。

因为 EVM 的 dB 表示方式是：

$$
EVM_{dB}=20\log_{10}(EVM)
$$

假设：

$$
EVM=2.5\%
$$

也就是：

$$
0.025
$$

那么：

$$
20\log_{10}(0.025)\approx-32.04dB
$$

所以：

```text
EVM = 2.5%
```

等价于：

```text
EVM ≈ -32 dB
```

因此：

> **EVM dB 数值越负越好。**

例如：

```text
-20 dB    ← 差
-25 dB
-30 dB
-35 dB    ← 好
-40 dB    ← 更好
```

---

# 5. EVM 和 SNR 有什么关系？

两者关系非常密切。

理想情况下，可以近似理解：

$$
EVM \approx \frac{1}{\sqrt{SNR}}
$$

因此：

$$
EVM_{dB}\approx-\mathrm{SNR}_{dB}
$$

例如：

```text
SNR = 30 dB

EVM ≈ 10^(-30/20)
    ≈ 3.16%
```

所以如果你看到：

```text
SNR = 30 dB
EVM ≈ -30 dB
```

这是非常正常的。

不过真实无线系统中，EVM 不只是由随机噪声决定，还可能受到：

* PA distortion
* phase noise
* IQ imbalance
* frequency error
* DC offset
* nonlinear distortion

等因素影响。

因此：

> **EVM 可以看成整个 TX/RX 调制链路“综合误差”的一个结果。**

---

# 6. 对 Wi-Fi 来说，EVM 特别重要

比如你之前在排查：

> **QCA8385 + 36/44 信道 + 160 MHz + 2.5G PHY**

如果怀疑 RF 受到某种干扰，那么 EVM 就是一个很有价值的观察指标。

例如正常：

```text
Channel 36
160 MHz

TX Power    15 dBm
EVM         -35 dB
```

换成异常条件：

```text
Channel 36
160 MHz

TX Power    15 dBm
EVM         -22 dB
```

那么说明：

**调制质量明显恶化。**

如果同时出现：

```text
EVM ↑
RX OFDMA timing error ↑
coarse timing timeout ↑
```

那么就值得进一步判断是不是存在：

* RF 干扰
* 时钟/PLL 问题
* 频偏
* IQ 问题
* PA 工作点异常
* 某个频段的模拟电路耦合
* 160 MHz 特定子载波受到干扰

---

# 7. EVM 还能帮助定位 TX 还是 RX 问题

这里非常重要。

如果测试的是：

```text
Wi-Fi 芯片 TX
       ↓
RF cable
       ↓
Vector Signal Analyzer
       ↓
测 EVM
```

那么测出来的 EVM 主要反映：

> **TX 发射链路的质量。**

例如：

```text
Baseband
   ↓
DAC
   ↓
Mixer
   ↓
PA
   ↓
Filter
   ↓
Antenna
```

其中任何地方出现问题，都可能导致 EVM 变差。

---

而如果是：

```text
测试仪 TX
   ↓
RF
   ↓
Wi-Fi 芯片 RX
   ↓
Baseband
```

那么 EVM/解调质量更多反映：

> **RX 接收链路。**

所以看到 EVM 异常时，第一件事其实应该确认：

**这个 EVM 是谁测出来的、测的是 TX 还是 RX。**

---

# 8. EVM 和你之前遇到的 RX timing error 不完全是一回事

你之前看到：

```text
rx_ota_err_cnt_coarse_timing_timeout
rx_ofdma_timing_err_cnt
```

这些属于 **RX 接收同步/解调过程中的错误统计**。

而 EVM 是：

```text
理想符号
   ↓
实际符号
   ↓
两者距离
   ↓
EVM
```

两者可能互相有关，但不是同一个东西。

可以粗略理解成：

```text
RF 信号质量
      │
      ├──→ EVM
      │
      ├──→ SNR
      │
      ├──→ CFO / Frequency Error
      │
      └──→ Timing Error
                    │
                    └──→ RX timeout / OFDMA timing error
```

所以如果你现在是在继续排查 **QCA8385 36/44 信道、160 MHz 才异常**的问题，我会特别建议你把：

**EVM + RSSI + SNR + CFO + TX Power + RX timing error**

放在一起看，而不要只看某一个 counter。

如果你把你现在能读到的 **EVM 寄存器/driver log** 发给我，我可以进一步告诉你这个 EVM 是 **TX EVM 还是 RX EVM、单位是什么，以及应该怎么看它是否异常**。
