**Intermediate Frequency (IF) Amplifier，就是“中频放大器”**：它把混频后得到的 **IF 信号放大**，让后面的解调器或 ADC 收到幅度合适的信号。它**不负责降频**——降频是前面混频器做的。([ti.com](https://www.ti.com/sc/data/wireless/panos1.pdf?utm_source=openai))

放回我们一直讨论的接收链：

**天线 → RF 滤波器 → LNA → 混频器 → IF 滤波器 → IF Amplifier → 解调器或 ADC**

例如前面用 **2.4 GHz RF** 和 **2.3 GHz LO** 得到 **100 MHz IF**：IF 放大器放大的是这路 **100 MHz 中频信号的幅度**，不会把它变成别的频率，也不会直接判出 0 和 1。([ti.com](https://www.ti.com/sc/data/wireless/panos1.pdf?utm_source=openai))

它和 **LNA** 都能放大信号，但位置和侧重点不同：

- **LNA** 靠近天线，重点是在放大微弱 RF 信号时尽量少增加噪声。
- **IF Amplifier** 在混频之后，负责给中频链路提供所需增益；它常做成**可变增益放大器（VGA）**，配合 **AGC（自动增益控制）**：信号弱时多放大，信号强时少放大，避免后级信号过小或过载。([ti.com](https://www.ti.com/sc/data/wireless/panos1.pdf?utm_source=openai))

如果你看的框图是 **“Image Rejection Mixer → IF Filter → IF Amplifier → Frequency Demodulator → Data Slicer”**，就可以理解为：**先搬频并抑制镜像，再选出目标中频、把它放大，然后解调并判成数字位。**具体滤波器和放大器的先后、是否集成在同一芯片里，要看实际设计。([ti.com](https://www.ti.com/sc/data/wireless/panos1.pdf?utm_source=openai))