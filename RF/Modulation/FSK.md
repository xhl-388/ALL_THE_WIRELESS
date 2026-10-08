**FSK（Frequency Shift Keying，频移键控）就是用不同的无线电频率表示不同的数字符号。**最常见的是二进制 FSK：发“0”时频率稍低，发“1”时频率稍高。它改的是**频率**，而不是主要靠改变信号强弱来传数据。([mathworks.com](https://www.mathworks.com/help/comm/ug/frequency-modulation.html?utm_source=openai))

## 1. 发射时到底发生了什么？

假设中心频率 $f_c=433.920\ \text{MHz}$，频偏 $\Delta f=5\ \text{kHz}$，约定：

| 数据 | 发射频率 |
|---|---:|
| 0 | $f_c-\Delta f=433.915\ \text{MHz}$ |
| 1 | $f_c+\Delta f=433.925\ \text{MHz}$ |

那么发送 `0 1 1 0` 时，发射机依次使用**低、高、高、低**这两个频率。这里的 **5 kHz 是相对于中心频率的频偏**；两个频率之间相差 **10 kHz**。注意不同资料可能把“频偏”用于指单边偏移或两频率间距，看参数表时最好确认定义。([ti.com](https://www.ti.com/lit/pdf/swru011?utm_source=openai))

这里说的“切换频率”，不是说每个比特都要换一个射频频道：上述两个频率仍围绕同一个中心频率，只是在其附近摆动。

## 2. 接收端怎么读出 0 和 1？

接收机先选出目标信号，再判断每个符号时间内的频率偏向哪一边：偏低判为 0，偏高判为 1。实现上可以使用频率鉴别等方法，也可以利用两个候选频率的检测结果作比较。接收端还得找准**什么时候开始、结束一个符号**，并处理噪声及收发两端的频率误差；因此实际无线链路不只是“数两个频率”。([mathworks.com](https://www.mathworks.com/help/comm/ug/frequency-modulation.html?utm_source=openai))

## 3. 三个最常见的参数

- **中心频率 $f_c$**：信号围绕哪个频率发射，例如上面的 433.920 MHz。
- **频偏 $\Delta f$**：发“0”或“1”时离中心频率多远。例如 ±5 kHz。
- **符号率 $R_s$**：每秒发送多少个符号，单位 baud。对普通二进制 2-FSK，**一个符号携带一位**，所以 10 kbaud 对应 10 kbit/s 的原始比特率；这不是扣除同步、包头等开销后的有效数据吞吐率。4-FSK 则使用四个频率状态，理论上每符号可表示两位。([mathworks.com](https://www.mathworks.com/help/comm/ug/frequency-modulation.html?utm_source=openai))

还会看到**调制指数**。对上述二进制 FSK 的常见定义：

$$
h=\frac{2\Delta f}{R_s}
$$

若 $R_s=10\ \text{kbaud}$、$\Delta f=5\ \text{kHz}$，则 $h=1$。其中 $2\Delta f$ 正是两个标称频率的间距。([e2e.ti.com](https://e2e.ti.com/support/wireless-connectivity/sub-1-ghz-group/sub-1-ghz/f/sub-1-ghz-forum/944586/ccs-launchxl-cc1312r1-how-to-measure-the-bandwidth?utm_source=openai))

## 4. 为什么不能把两个频率设得越近或越远越好？

设得**太近**，接收端在噪声和频率误差下更难区分 0、1；设得**太远**，信号通常会占用更宽的频谱。符号率提高也会影响所需带宽。这是 FSK 设计中的基本权衡。([ti.com](https://www.ti.com/lit/an/swra234a/swra234a.pdf?ref_url=https%253A%252F%252Fwww.ti.com%252Fproduct%252Fde-de%252FCC1101&ts=1776832476916&utm_source=openai))

对普通二进制 FSK，工程上常用一个**粗略带宽估计**：

$$
B\approx R_s+2\Delta f
$$

例如 10 kbaud、±5 kHz，估计约为 $10+10=20\ \text{kHz}$。**它不是精确的占用带宽或接收滤波器设置值**：波形整形、频率误差、器件指标及带宽的测量定义都会影响实际选择。尤其接收滤波器过窄可能削掉有用信号，过宽又会引入更多噪声。([ti.com](https://www.ti.com/lit/an/swra234a/swra234a.pdf?ref_url=https%253A%252F%252Fwww.ti.com%252Fproduct%252Fde-de%252FCC1101&ts=1776832476916&utm_source=openai))

## 5. FSK、GFSK、MSK 有什么关系？

- **FSK**：总称，用频率状态表示数据。
- **GFSK（Gaussian FSK）**：先对数据变化做高斯滤波，让频率转换更平滑，从而改善频谱外扩和邻道表现；代价是相邻符号之间的影响需要在设计中考虑。蓝牙低功耗无线电物理层就是一个采用 GFSK 的实例。([ti.com](https://www.ti.com/lit/ug/swru346b/swru346b.pdf?utm_source=openai))
- **MSK（Minimum Shift Keying）**：一种相位连续的特殊二进制 FSK，其调制指数 $h=0.5$。**GMSK** 则是在 MSK 的基础上采用高斯脉冲整形。([mathworks.com](https://www.mathworks.com/help/releases/r2024a/pdf_doc/comm/comm_tbx.pdf?utm_source=openai))

最后要注意：**频率变了，不代表载波的相位必须突然跳断。**实际方案可以让相位连续；GFSK、MSK 等讨论的正是波形如何平滑变化，而不只是“0 对应哪个频率”。([ch.mathworks.com](https://ch.mathworks.com/help/comm/ug/continuous-phase-modulation.html?utm_source=openai))

**一句话记忆：**把 FSK 想成发射机用“稍低的音调”和“稍高的音调”说 0、1；中心频率决定它在哪个无线频道附近说话，频偏决定两个“音调”隔多远，符号率决定说得多快。

## 如此快的频率变化可以被接收端识别吗
**可以识别，但不是在频率刚变化的“瞬间”就知道它变了。**接收端会在每个符号持续的一小段时间里观察信号，再判断它更像“低频”还是“高频”。

沿用前面的例子：10 kbaud 意味着每个比特持续 **100 微秒**；两个发射频率相差 **10 kHz**。虽然 100 微秒很短，接收机仍可以先把 433 MHz 附近的射频信号下变频，再用频率鉴别器，或分别检测两个候选频率的接收强度，作出判决。它不需要拿秒表直接数完 433 MHz 载波的周期。

关键是一个**观察时间与可靠性的权衡**：

- 符号持续越久，越容易区分两个频率；
- 两个频率离得越远，也越容易区分；
- 如果发送得过快、频率间隔又过小，加上噪声和收发机频率误差，就可能误判。

所以接收机能否跟上，取决于**符号率、频偏、信噪比和接收机设计是否匹配**，而不只是“频率切换得快不快”。可以把它想成听人快速交替吹两个音高：不必在切换的那一刹那听出来，只要每个音持续得足够久、音高差足够明显，就能分辨。

在 **100 微秒**内，433 MHz 的载波会完成：

$$
433{,}000{,}000\ \text{周期/秒}\times0.0001\ \text{秒}
=\boxed{43{,}300\ \text{个周期}}
$$

如果按前面举例的 **433.920 MHz** 算，就是 **43,392 个周期**。

这也说明了两个不同的“快”：载波本身每秒振荡约 4.33 亿次，而在 10 kbaud 的例子中，数据每 **100 微秒**才切换一个符号。
