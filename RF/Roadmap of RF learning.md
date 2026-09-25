有，而且结合你现在的情况——**不是想做 RF 芯片设计，而是已经在实际排查 QCA 无线芯片、EVM、OFDMA timing、160 MHz 信道异常，但 RF 基础比较薄**——我不建议你一上来啃特别硬的微波电路教材。

我更推荐走：

> **RF 基础 → IQ/数字调制 → Wi-Fi PHY → RF 测量与 EVM → 再回头看 PA/PLL/LO/射频前端**

这条路线。

## 我最推荐的几个资源

### ① Analog Devices：RF Basics Guide ⭐⭐⭐⭐⭐

[Analog Devices RF Basics Guide](https://www.analog.com/en/resources/technical-articles/rf-basics-guide.html?utm_source=chatgpt.com)

这个非常适合你现在。

它就是针对“刚接触 RF 的人”写的，涵盖 RF 系统里常见的：

- dB / dBm
    
- Frequency
    
- Bandwidth
    
- RF signal
    
- Mixer
    
- Amplifier
    
- Filter
    
- Noise
    
- SNR
    
- Phase noise
    
- 等等
    

ADI 自己也明确把它定位成 RF 入门和快速参考资料。([模拟器件](https://www.analog.com/en/resources/technical-articles/rf-basics-guide.html?gated=1749770895041&utm_source=chatgpt.com "RF Basics Guide | Analog Devices"))

**建议你把它当成 RF 字典。**

遇到：

> LO 是什么？

> Mixer 是什么？

> 为什么 RF 要用 dBm？

> Phase noise 到底是什么？

就回来查。

---

### ② Keysight RF Back to Basics ⭐⭐⭐⭐⭐

这个我其实**非常推荐你**。

[Keysight RF Back to Basics Bootcamp](https://www.keysight.com/zz/en/learn/course.rf-back-to-basics.html?utm_source=chatgpt.com)

它不是纯理论课，而是站在 **RF 工程师实际测量**的角度讲。

内容包括：

```text
RF Signal Chain
       ↓
Network Analysis
       ↓
Spectrum Analysis
       ↓
Signal Generation
       ↓
Modulation
```

官方课程还专门讲：

- Signal Power
    
- Frequency
    
- Bandwidth
    
- RF signal chain
    
- Spectrum Analyzer
    
- RF signal generation
    
- Modulation
    

这些正好是你现在缺的基础。([Keysight United States](https://www.keysight.com/zz/en/learn/course.rf-back-to-basics.html?utm_source=chatgpt.com "Course | RF Back to Basics Bootcamp | Keysight"))

而且 Keysight 还有一个更完整的桌面版 Back to Basics Seminar：

[Keysight RF Back to Basics Seminar](https://connectlp.keysight.com/RFB2B_Desk_Seminar?utm_source=chatgpt.com)

里面有三个非常值得看的部分：

**RF Microwave Signal Chain & Network Analysis**

约 1h20m，讲：

- Transmission line
    
- S-parameter
    
- Smith Chart
    
- Dynamic range
    
- RF signal chain
    

**Spectrum Analysis**

约 58min，讲：

- FFT
    
- RBW
    
- VBW
    
- Spectrum Analyzer
    
- Dynamic range
    

**Signal Generation and Modulation Basics**

约 32min，讲：

- CW
    
- Phase Noise
    
- VCO
    
- PLL
    
- Analog modulation
    
- Digital modulation
    

官方页面有这些课程的具体内容。([connectlp.keysight.com](https://connectlp.keysight.com/RFB2B_Desk_Seminar?utm_source=chatgpt.com "View RF Back to Basics Seminar from Your Desk"))

**如果你只想选一个视频类资源，我建议先看这个。**

---

# ③ Analog Devices：Fundamentals of RF and Wireless Communications

[ADI Fundamentals of RF and Wireless Communications](https://www.analog.com/en/resources/media-center/videos/6313215308112.html?utm_source=chatgpt.com)

这个比刚才的 RF Basics 稍微偏“系统”。

它主要讲：

> RF / Wireless Communication 系统到底由什么组成，以及各种 RF 参数是什么意思。

官方定位就是介绍 RF 和无线通信的基本原理、常见功能、规格和关键参数。([模拟器件](https://www.analog.com/en/resources/media-center/videos/6313215308112.html?utm_source=chatgpt.com "Fundamentals of RF and Wireless Communications | Analog Devices"))

你可以把它理解成：

```text
RF Basics Guide
     ↓
词汇 / 基础概念

Fundamentals of RF
     ↓
把这些东西串成一个系统
```

---

# ④ Analog Devices：SDR for Engineers ⭐⭐⭐⭐⭐

这个我尤其推荐你。

[Analog Devices Digital Communications / SDR for Engineers](https://wiki.analog.com/university/courses/comms?utm_source=chatgpt.com)

因为你现在真正需要理解的不是“天线怎么设计”，而是：

> **Wi-Fi 的数字数据到底是怎么变成 RF 信号的。**

SDR 这套东西刚好可以把这个过程讲清楚：

```text
Bits
 ↓
Symbols
 ↓
I/Q
 ↓
Digital Signal
 ↓
DAC
 ↓
Mixer
 ↓
RF
 ↓
Antenna
```

反过来：

```text
Antenna
 ↓
RF
 ↓
Mixer
 ↓
ADC
 ↓
I/Q
 ↓
Demodulation
 ↓
Bits
```

ADI 的课程材料包含：

- Signals and Systems
    
- Probability
    
- Digital Communication
    
- SDR
    
- 实验
    
- 作业
    

而且页面明确提供 SDR for Engineers 的配套材料。([模拟器件维基](https://wiki.analog.com/university/courses/comms?utm_source=chatgpt.com "Digital Communications [Analog Devices Wiki]"))

**这可能是最适合你把“数字通信”和“RF”连接起来的资源。**

---

# ⑤ Keysight 的 EVM 专题

等你前面的东西有一点基础之后，再看这个：

[Keysight Best Practices for Optimizing Your EVM Measurement](https://www.keysight.com/gb/en/learn/course.best-practices-for-optimizing-your-evm-measurement.html?utm_source=chatgpt.com)

这个课程专门讲：

```text
IQ
 ↓
Modulation
 ↓
EVM
 ↓
Spectrum Analyzer
 ↓
Vector Signal Analysis
 ↓
EVM troubleshooting
```

而且会进一步讲：

- EVM
    
- CCDF
    
- Power statistics
    
- Wideband EVM
    
- 5G/6G
    
- MIMO
    
- EVM degradation
    

官方课程就是面向 RF / wireless / validation 工程师的。([Keysight United States](https://www.keysight.com/gb/en/learn/course.best-practices-for-optimizing-your-evm-measurement.html?utm_source=chatgpt.com "Course | Best Practices for Optimizing Your EVM Measurement | Keysight"))

等你看到这里，你之前问我的：

> PA 非线性  
> IQ imbalance  
> Phase noise  
> CFO  
> Noise  
> DAC/ADC  
> LO leakage

就会开始真正串起来。

---

# ⑥ 如果想系统学无线通信：《Wireless Communications》

这个就是比较正规的大学教材了。

**Andrea Goldsmith — Wireless Communications**

[Andrea Goldsmith《Wireless Communications》](https://www.cambridge.org/core/books/wireless-communications/800BA8A8211FBECB133A7BB77CD2E2BD?utm_source=chatgpt.com)

它覆盖：

- Wireless channel
    
- Modulation
    
- Coding
    
- Equalization
    
- Multicarrier
    
- MIMO
    
- Adaptive modulation
    
- Wireless system design
    

而且 Stanford 还有 Andrea Goldsmith 自己讲的 EE359 Wireless Communications 课程页面。([Stanford University](https://web.stanford.edu/class/ee359/courseinfo.html?utm_source=chatgpt.com "EE359, Wireless Communications, Winter 2020"))

不过我**不建议你现在直接从第一页开始啃这本**。

因为你现在的问题不是：

> “无线通信理论不会。”

而是：

> “RF 工程师说的 EVM、IQ、LO、PA、PLL、CFO 到底是什么意思？”

这两个阶段不一样。

---

# 我给你排一个学习顺序

按照你目前的情况，我会这样学：

```text
                 RF 入门
                   │
                   ▼
        Keysight RF Back to Basics
                   │
                   ▼
          Analog Devices RF Basics
                   │
                   ▼
              Signals
                 ↓
           I / Q 是什么
                 ↓
         IQ Modulation
                 ↓
        QPSK / QAM / OFDM
                 ↓
          DAC / ADC
                 ↓
       Mixer / LO / PLL
                 ↓
          PA / LNA / Filter
                 ↓
          Noise / SNR
                 ↓
        Phase Noise / CFO
                 ↓
                EVM
                 ↓
           Wi-Fi PHY
                 ↓
       802.11ac / ax / be
                 ↓
        160 MHz / OFDMA
                 ↓
       你现在正在排查的问题
```

---

## 特别针对你，我建议先不要学这些

暂时不要一头扎进：

- Smith Chart
    
- S 参数
    
- 微带线
    
- 天线设计
    
- 阻抗匹配
    
- 电磁场
    
- 微波网络理论
    

这些当然重要，但是**对你现在分析 Wi-Fi 芯片 log 帮助没那么直接**。

你现在最需要的是：

### 第一阶段

```text
dB
dBm
Hz
Bandwidth
Carrier
I/Q
Amplitude
Phase
```

### 第二阶段

```text
QPSK
QAM
OFDM
Subcarrier
Constellation
EVM
SNR
CFO
```

### 第三阶段

```text
DAC
ADC
Mixer
LO
PLL
PA
LNA
Filter
IQ imbalance
Phase noise
```

### 第四阶段

```text
802.11
20/40/80/160 MHz
OFDM
OFDMA
MCS
RU
Pilot
Subcarrier
Channel estimation
Timing synchronization
Frequency synchronization
```

到这里，你再回来看你之前的：

```text
rx_ota_err_cnt_coarse_timing_timeout
rx_ofdma_timing_err_cnt
```

就不会再觉得它们是完全陌生的 counter 了。

---

## 如果只让我给你选 3 个

我会选：

**① Keysight RF Back to Basics**

→ 建立 RF 工程直觉。([Keysight United States](https://www.keysight.com/zz/en/learn/course.rf-back-to-basics.html?utm_source=chatgpt.com "Course | RF Back to Basics Bootcamp | Keysight"))

**② Analog Devices SDR for Engineers**

→ 搞懂 **Bit → IQ → RF → IQ → Bit**。([模拟器件维基](https://wiki.analog.com/university/courses/comms?utm_source=chatgpt.com "Digital Communications [Analog Devices Wiki]"))

**③ Keysight EVM Course**

→ 把你现在正在看的 **EVM / IQ / RF impairment** 真正串起来。([Keysight United States](https://www.keysight.com/gb/en/learn/course.best-practices-for-optimizing-your-evm-measurement.html?utm_source=chatgpt.com "Course | Best Practices for Optimizing Your EVM Measurement | Keysight"))

然后再去看 **Wi-Fi PHY / 802.11 OFDM/OFDMA**。

这条路线会比直接看一本厚厚的《无线通信原理》更适合你目前的工作场景。