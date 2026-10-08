**PLL 是 Phase-Locked Loop，中文叫“锁相环”。**在你刚才那条 RF 接收链路里，它通常用来产生并稳定**混频器需要的本振信号（LO）**；它不是天线信号依次经过的下一个器件。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/pll-synthesizers.html?utm_source=openai))

可以把它画成这样：

> 参考时钟 → 相位/频率比较器 → 环路滤波器 → VCO（压控振荡器）→ 输出本振给混频器  
> 　　　　　　　　　　　　　　　　　　　　　　↖ 输出经分频后，反馈到比较器

工作方式很像**自动校准节奏**：比较器不断检查“参考时钟”和“分频后的输出”是否对齐；如果没对齐，就调整 VCO，让输出频率和相位跟着参考时钟锁定。实际电路中，比较器与环路滤波器之间通常还有一个**电荷泵**。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/pll-synthesizers.html?utm_source=openai))

接上前面的例子：要把 **2.4 GHz** 接收信号降到 **100 MHz**，混频器可以使用 **2.3 GHz 的本振**。PLL 的任务就是产生这个稳定的 2.3 GHz；**真正完成降频的是混频器**。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/pll-synthesizers.html?utm_source=openai))