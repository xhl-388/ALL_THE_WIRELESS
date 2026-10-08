**DSSS（Direct-Sequence Spread Spectrum，直接序列扩频）**的核心是：**发送端用一串比数据快得多、接收端也知道的码，把每个数据比特展开成多个“码片”；接收端把码对准后，通过相关运算把数据恢复出来。**“直接序列”指扩频码直接作用于待发送的数据，而不是像跳频那样不断更换工作频点。([analog.com](https://www.analog.com/en/resources/technical-articles/introduction-to-spreadspectrum-communications--maxim-integrated.html?gated=1755019231608&utm_source=openai))

## 从你前面的 100 微秒例子开始

假设原始数据速率是 **10 kbit/s**，那么每个比特持续 **100 微秒**。现在规定每个比特用 **8 个码片（chip）**发送：

| 量 | 数值 |
|---|---:|
| 数据速率 | 10 kbit/s |
| 每比特码片数 | 8 |
| 码片速率 | 80 kchip/s |
| 每个码片持续时间 | 12.5 微秒 |

注意：**码片不是新增的 8 个用户数据比特**。它们是为了表示原来那 *1 个*比特而发出的、更快的波形变化。码片变化更快，信号频谱通常也就比未扩频时宽得多；实际带宽还取决于调制和波形整形。([analog.com](https://www.analog.com/en/resources/technical-articles/introduction-to-spreadspectrum-communications--maxim-integrated.html?gated=1755019231608&utm_source=openai))

## 发送端：怎么把 1 个比特变成 8 个码片？

用一个便于理解的简化例子。假设双方约定的 8 位扩频码是：

`10110010`

发送端将数据比特与这串码逐位做异或（XOR）：

| 原始比特 | 发出的 8 个码片 |
|---|---|
| `0` | `10110010` |
| `1` | `01001101` |

原因是 `0 XOR 码 = 原码`，而 `1 XOR 码 = 原码逐位取反`。随后，发射机再把这些码片变成无线电信号；**XOR 是基带上的处理，不是天线直接发出字符“0”和“1”**。一种典型实现会把处理后的码片送进相位调制器。([e2echina.ti.com](https://e2echina.ti.com/cfs-file/__key/telligent-evolution-components-attachments/13-110-00-00-00-00-84-72/Implementing-a-Bidirectional-Frequency-Hopping-Application-WTRF6903-and-MSP4301.pdf?utm_source=openai))

## 接收端：为什么能“解扩”？

接收端已经知道码 `10110010`，也找到了这 8 个码片的起点。它就能把收到的码片与本地码逐位比较：

- 如果收到的序列大体与本地码**相同**，判断原始比特为 `0`；
- 如果大体与本地码**相反**，判断原始比特为 `1`。

不要求 8 个码片全部正确。例如本应收到 `10110010`，但其中 2 个码片受干扰翻转，仍有 6 个与本地码相符；在这个简化判决例子里，接收端仍可判为 `0`。实际接收机通常对采样信号做**相关、累加和判决**，不一定先把每个码片硬判成 0 或 1。([kr.mathworks.com](https://kr.mathworks.com/help/comm/ug/dsss-receiver-for-sar-based-tracking-system.html?utm_source=openai))

更一般地，用 $+1/-1$ 表示信号和扩频码，可以把解扩写成：

$$
z=\sum_{i=1}^{N}r_i c_i
$$

其中 $r_i$ 是收到的第 $i$ 个码片，$c_i$ 是本地码，$N$ 是每比特的码片数。**码对准时，有用信号在求和中同向累积；不匹配的干扰通常不会以同样方式累积。**这就是相关解扩的基本直觉。([kr.mathworks.com](https://kr.mathworks.com/help/comm/ug/dsss-receiver-for-sar-based-tracking-system.html?utm_source=openai))

## 为什么扩频能抗干扰？

想象有一个干扰信号，只占据扩频频带中的一小段。发送的有用信号原本分布在较宽频带内；接收端用正确的码解扩时，**有用信号被集中回数据尺度，而不跟随该码变化的窄带干扰则被分散**，因此更容易抑制它。这个好处针对的是特定类型的干扰，**不是说扩频后任何环境下都会凭空得到更高的信噪比**。([analog.com](https://www.analog.com/en/resources/technical-articles/introduction-to-spreadspectrum-communications--maxim-integrated.html?utm_source=openai))

常见的一个指标叫**处理增益**。在这个简化的一比特对应 $N$ 个码片的模型中：

$$
G_p \approx \frac{R_c}{R_b}=N,\qquad
G_{p,\mathrm{dB}}\approx10\log_{10}N
$$

这里 $R_c$ 是码片速率，$R_b$ 是数据比特率。上面的 8 码片例子，名义处理增益是 $8$，约 **9 dB**。**这不等于“接收功率自动增加 9 dB”，也不保证误码率无条件改善 9 dB**；它描述的是扩频与解扩在带宽、干扰处理上的一个比值。([analog.com](https://www.analog.com/en/resources/design-notes/designing-a-lowcost-lowcomponentcount-gps-receiver.html?utm_source=openai))

## 最难的地方：双方必须“对上码”

如果接收端的码虽然正确，却**晚了几个码片**，相关结果可能大幅降低，数据便难以判断。因此接收机需要先找到信号，再估计码片时序、频率偏差等，保持同步。扩频码通常也要有好的**自相关**性质：对准时出现明显峰值，错开时相关较低。不同用户若共用频带，还要考虑彼此码之间的**互相关**。([kr.mathworks.com](https://kr.mathworks.com/help/comm/ug/dsss-receiver-for-sar-based-tracking-system.html?utm_source=openai))

结合前面的 433 MHz 问题看：在这个例子里，载波仍在 433 MHz 附近快速振荡；DSSS 新增的是**每 12.5 微秒左右变化一次的码片节奏**。接收机不是逐个“数完”载波周期，而是在下变频和采样后，利用已知码的相关性，从这些快速变化中识别原来的 **100 微秒数据比特**。

## 发送机的频率还是只有2个吗？只是频率变化速度更快了？
**如果把刚才的 DSSS 码片送进 2-FSK 调制器，你理解得基本对：仍用两个标称频率表示 0、1，但由码片而不是原始数据比特决定何时切换。**不过，**DSSS 本身不规定一定用 FSK**；前面我说“典型实现送进相位调制器”，指的是另一种情况，不能把两者混为一谈。直序扩频后的码片可以送进 BPSK，也可以送进 2-(G)FSK 调制器。([analog.com](https://www.analog.com/en/resources/technical-articles/introduction-to-spreadspectrum-communications--maxim-integrated.html?gated=1755019231608&utm_source=openai))

沿用之前的数字：原始数据每 **100 微秒**一个比特，每比特展开成 **8 个码片**，于是每个码片持续 **12.5 微秒**。

- **若采用 2-FSK**：可以仍用 433.915 MHz 和 433.925 MHz 这两个标称频率表示码片 0、1。码片序列按 12.5 微秒的节奏推进，**但只有相邻码片不同，频率状态才需要切换**；不是每 12.5 微秒必定切一次。TI 的一种 DSSS 实现就是先扩频、再送入 2-(G)FSK 调制器。([ti.com](https://www.ti.com/lit/an/swra642/swra642.pdf?ref_url=https%253A%252F%252Fwww.ti.com%252Fproduct%252Fko-kr%252FCC1350&ts=1774156043133&utm_source=openai))
- **若采用 BPSK**：码片控制的是载波的**相位状态**，例如 0° 或 180°，而不是在两个标称载波频率间切换。BPSK 是常见的 DSSS 调制方式。([analog.com](https://www.analog.com/en/resources/technical-articles/introduction-to-spreadspectrum-communications--maxim-integrated.html?gated=1755019231608&utm_source=openai))

还有一个容易误解的点：**“两个标称频率”不等于频谱图上只有两根细线。**快速发送码片会让信号占据一段频带；码片率越高，通常需要的带宽越大，具体形状还取决于调制和波形整形。([analog.com](https://www.analog.com/en/resources/technical-articles/introduction-to-spreadspectrum-communications--maxim-integrated.html?gated=1755019231608&utm_source=openai))

所以更准确的说法是：**DSSS 让待调制的序列变化得更快；至于发射机是“更快地选两个频率”，还是“更快地改变相位”，取决于后面选用 FSK 还是 BPSK。**


## 为什么 “快速发送码片会让信号占据一段频带”
你卡住的点可能是：**既然 FSK 只有两个频率，频谱上为什么不是只有两条线？**

关键在于：**“某一刻选用哪个频率”和“把一整段不断变化的信号拿去做频谱分析”是两回事。**

先不考虑 FSK，想象发射机始终发送一个 **433.915 MHz** 的纯正弦波：

- 如果它一直发、永远不变，理想频谱就是一条很细的线。
- 如果只发 **12.5 微秒**就停，信号有了“开始”和“结束”。频谱就不再是无限细的一条线，而会向周围展开。**持续时间越短，展开得越宽**：量级与持续时间的倒数 $1/T$ 有关。([de.mathworks.com](https://de.mathworks.com/help/phased/ug/rectangular-pulse-waveforms.html?utm_source=openai))

FSK 也是类似的。虽然调制器可以只设定“低频”和“高频”**两个标称频率**，但发送 `低→高→低→高…` 时，每段都只持续有限时间，整段波形还在不断变化。因此频谱仪看到的是两个频率**附近的一片频谱**，不是两根无限细的线。FSK 的占用频谱既与频偏有关，也与切换所用的符号率有关。([ez.analog.com](https://ez.analog.com/ez-blogs/b/engineering-mind/posts/digital-signal-modulation-with-fsk-rf-modulation-schemes-part-4-of-7?utm_source=openai))

回到之前的例子：

- 不扩频：每个数据比特 **100 微秒**，变化的时间尺度对应 $1/T=10\ \text{kHz}$。
- 扩成 8 个码片后：每个码片 **12.5 微秒**，变化的时间尺度对应 $1/T=80\ \text{kHz}$。

**80 kHz 不是“发射机又增加了一个 80 kHz 的载波”，也不是说占用带宽恰好等于 80 kHz。**它只是说明：波形若能以更短的时间尺度变化，频谱通常也会向离标称频率更远的地方延伸；实际宽度还取决于频偏和切换是否经过平滑处理。TI 的一种 DSSS 方案就是把扩频后的序列送入 2-GFSK 调制器。([ez.analog.com](https://ez.analog.com/ez-blogs/b/engineering-mind/posts/digital-signal-modulation-with-fsk-rf-modulation-schemes-part-4-of-7?utm_source=openai))

可以把它记成一句话：**两个频率是发射机用来表示码片的“两个档位”；频带宽度描述的是整段信号快速换档后，在频谱仪上占了多宽。**

可以。先给你最关键的数学结论：**“某一时刻的振荡频率只有两个可选值”不等于“整段波形的傅里叶频谱只有两个频点”。**傅里叶变换分析的是一整段信号；频率状态什么时候切换，也会影响计算结果。

先从一个**只持续 $T$ 秒的单频信号**算起，再回到 FSK。


## 能从数学上解释一下吗
### 1. 一个频率，只发一小段时间

用复数表示一个从 $t=0$ 发到 $t=T$ 的频率 $f_0$：

$$
x(t)=
\begin{cases}
e^{j2\pi f_0t},&0\leq t<T\\
0,&\text{其他时间}
\end{cases}
$$

它的傅里叶变换是：

$$
X(f)=\int_0^T e^{j2\pi f_0t}e^{-j2\pi ft}\,dt
=T e^{-j\pi(f-f_0)T}\,
\operatorname{sinc}\big((f-f_0)T\big)
$$

其中 $\operatorname{sinc}(u)=\sin(\pi u)/(\pi u)$。所以即使这段信号**从头到尾只用 $f_0$ 一个频率**，其频谱幅度仍是以 $f_0$ 为中心的 sinc 形状，而不是一根无限细的线。它的第一对零点在：

$$
f=f_0\pm\frac{1}{T}
$$

原因就在积分的上下限 $0$ 和 $T$：**只截取有限长的一段正弦波，会得到一片频谱。**$T$ 越短，零点离中心越远。([mathworks.com](https://www.mathworks.com/help/phased/ref/phased.rectangularwaveform.bandwidth.html?utm_source=openai))

### 2. 代入刚才的码片时长

| 一段信号的时长 $T$ | $1/T$ | 这一段信号频谱的第一零点 |
|---|---:|---|
| 100 微秒 | 10 kHz | 距它的中心频率 ±10 kHz |
| 12.5 微秒 | 80 kHz | 距它的中心频率 ±80 kHz |

注意：**±80 kHz 是上述“单独一小段信号”的傅里叶变换零点，不等于完整 FSK 发射信号的实际占用带宽。**完整信号要把连续各段的波形连起来，再一起分析；各段的频谱会相加，可能相互增强或抵消。([mathworks.com](https://www.mathworks.com/help/phased/ref/phased.rectangularwaveform.bandwidth.html?utm_source=openai))

### 3. 回到“只有两个频率”的 FSK

设码片序列 $a(t)$ 在每个码片期间取 $+1$ 或 $-1$。一种相位连续的 2-FSK 可以写成：

$$
x(t)=\exp\left[j2\pi f_ct+
j2\pi\Delta f\int_0^t a(\tau)\,d\tau\right]
$$

它的瞬时频率，也就是相位对时间的变化率除以 $2\pi$，确实只有：

$$
f_{\text{inst}}(t)=f_c+\Delta f\,a(t)
=
\begin{cases}
f_c+\Delta f\\
f_c-\Delta f
\end{cases}
$$

但要得到**频谱**，还得计算整段波形的

$$
X(f)=\int_{-\infty}^{\infty}x(t)e^{-j2\pi ft}\,dt
$$

这个积分包含了所有码片及其切换时刻。因此，“瞬时频率取两个值”并不能推出“$X(f)$ 只在两个频点非零”。相位连续 FSK 的模型也是这样从**码片序列累积相位**来描述的。([mathworks.com](https://www.mathworks.com/help/comm/ref/comm.cpfskmodulator-system-object.html?utm_source=openai))

**这里我要修正我上条回答中说得过于绝对的地方：**“码片快 8 倍，频谱就宽 8 倍”对 **DSSS+BPSK 的理想矩形码片模型**有清晰的数学对应关系；但对**保持频偏不变的 FSK**，不能直接这么算。FSK 的频谱还取决于频偏、码片序列和频率切换的平滑方式。极端地说，如果码片一直相同、根本不切换，它就是一个持续的单频信号，不会因为我们把“码片时钟”设得很快就自动扩频。([mathworks.com](https://www.mathworks.com/help/comm/ref/comm.cpfskmodulator-system-object.html?utm_source=openai))

**一句话区分两个“频率”：**FSK 的“两频”说的是波形在每个码片期间的**瞬时频率档位**；“占据一段频带”说的是把**不断切换的整段波形做傅里叶变换**之后，能量分布在哪些频率上。
