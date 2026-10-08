**Divider 在你前面问的晶振和 PLL 语境里，通常指“分频器”**：输入一个周期信号，输出频率变低的周期信号。例如，**÷10** 分频器把 10 MHz 时钟变成 1 MHz 时钟。它处理的是时钟或本振的**频率**，不是把收到的无线数据分成几份。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/pll-synthesizers.html?utm_source=openai))

在 PLL 里，常见两个 Divider：

- **R Divider（参考分频器）**：把晶振提供的参考时钟分频，再送到相位/频率比较器。
- **N Divider（反馈分频器）**：把 VCO 的高频输出分频后反馈给比较器，让它能与较低频率的参考信号比较。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/pll-synthesizers.html?utm_source=openai))

可以这样看：

**晶振 → R 分频器 → 比较器 ← N 分频器 ← VCO 输出**  
　　　　　　　　　　**比较器 → 环路滤波器 → VCO → 本振 LO**

锁定时，两路送进比较器的频率相等。简化为整数分频、且没有额外输出分频器时：

**VCO 频率 = 晶振频率 × N ÷ R**。例如晶振是 **10 MHz**，R＝1、N＝230，VCO 就可以锁定在 **2.3 GHz**，作为我们前面例子中的混频器本振。**Divider 本身没有把 10 MHz 升到 2.3 GHz**；高频由 VCO 产生，PLL 借助分频反馈把它锁定在目标频率。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/pll-synthesizers.html?utm_source=openai))