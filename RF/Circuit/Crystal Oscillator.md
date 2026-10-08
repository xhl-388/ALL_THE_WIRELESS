**Crystal Oscillator（晶体振荡器，常简称“晶振”）就是提供稳定时钟的器件。**接着你前面问的 PLL 来看，它常给 PLL 提供一个**参考频率**；PLL 再以此为基准，产生混频器需要的本振（LO）。它不是接收信号从天线到 ADC 必须经过的一站。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/pll-synthesizers.html?utm_source=openai))

把两条路径放在一起看会更清楚：

```text
接收信号：天线 → 滤波器 → LNA ─────────→ 混频器 → 滤波器 → ADC
                                      ↑
本振信号：晶体振荡器 → PLL（含 VCO）──→ LO
```

可以把**晶振**想成节拍稳定的“节拍器”，把 **PLL** 想成跟着这个节拍、产生所需频率的“调速器”。例如，晶振提供 **10 MHz** 参考时钟，PLL 可以据此合成前面例子里混频器所需的 **2.3 GHz LO**；实际如何分频、倍频由 PLL 设计决定。参考时钟的稳定性和相位噪声也会影响合成后本振的表现。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/pll-synthesizers.html?utm_source=openai))

有个容易混淆的小区别：**石英晶体（crystal）**本身主要是谐振元件，需要配合振荡电路工作；**晶体振荡器（crystal oscillator）**指能输出时钟信号的完整电路或器件。在具体芯片中，振荡电路也可能已经做在芯片内部，只需外接晶体。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/pll-synthesizers.html?utm_source=openai))

