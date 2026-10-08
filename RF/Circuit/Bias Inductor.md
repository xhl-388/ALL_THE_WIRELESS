**Bias Inductor（偏置电感）**是在给射频电路提供**直流偏置电流**的同时，尽量阻止射频信号沿供电线跑掉的电感。在这种用途下，它也常叫 **RF choke（射频扼流圈）**。([analog.com](https://www.analog.com/en/resources/app-notes/an-2061.html?utm_source=openai))

可以把它看成供电线上的一道“分流关卡”：

```text
直流电源 ── Bias Inductor ──●── 射频电路的工作节点
                            │
                            └── RF 信号通路
```

**为什么能这样工作？**电感对稳定的直流主要表现为绕线电阻，因此直流能通过；对交流信号，它的阻抗与频率有关，理想情况下为 $X_L=2\pi fL$。频率越高，沿电感进入电源支路就越困难。实际电感还有寄生电容、自谐振频率等限制，不能简单认为“电感越大越好”。([analog.com](https://www.analog.com/en/resources/app-notes/an-2061.html?utm_source=openai))

例如 RF 放大器的某个输出节点，既要接收直流供电，又承载高频输出：**偏置电感把直流送进去，尽量把 RF 留在信号通路里**。如果再配一个隔直电容把 RF 引向输出，这种组合常称为 *bias tee*（偏置三通）。([analog.com](https://www.analog.com/en/resources/app-notes/an-2061.html?utm_source=openai))

联系你刚问的 **VCO**：如果图上有 Bias Inductor，它通常是在**给某个振荡或输出电路节点提供偏置**；别仅凭名字就把它当成决定振荡频率的那只 LC 谐振电感。具体是否同时参与谐振，要看它在电路图中的接法。([analog.com](https://www.analog.com/media/en/technical-documentation/data-sheets/ADF4360-2.pdf?utm_source=openai))
