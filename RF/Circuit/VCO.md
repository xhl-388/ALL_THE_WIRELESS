**VCO（Voltage-Controlled Oscillator，压控振荡器）是“输出频率可以由电压调节的振荡器”。**它持续产生周期信号；改变输入的**调谐电压** $V_{\text{tune}}$，输出频率就会改变。它和你刚问的“谐振”关系很直接。([analog.com](https://www.analog.com/en/resources/glossary/vco.html?utm_source=openai))

一种常见的 RF VCO 内部有 **LC 谐振电路**，其中的电容可以用**变容二极管**实现：

```text
调谐电压变化 → 变容二极管的电容变化
             → LC 谐振频率变化
             → VCO 输出频率变化
```

理想 LC 电路的谐振频率是 $f=1/(2\pi\sqrt{LC})$。因此，如果其他条件不变、电容变小，频率就会升高。不过，**“调谐电压升高，频率一定升高”不能对所有 VCO 一概而论**，要看具体电路的调谐曲线。VCO 也需要供电；谐振电路不会自己持续产生能量。([analog.com](https://www.analog.com/en/resources/glossary/vco.html?utm_source=openai))

放回我们之前讨论的 **PLL → LO → 混频器**：

```text
晶振提供参考 ─→ PLL 比较与反馈 ─→ Loop Filter ─→ 调谐电压
                                          ↓
                                         VCO ─→ LO 信号 ─→ 混频器
                                          │
                                          └─ 经 Divider 反馈给 PLL
```

**VCO 是实际产生高频振荡信号的部分；PLL 则不断调整它的调谐电压，把输出锁定到参考时钟所决定的目标频率。**例如想要 2.3 GHz 的 LO，VCO 负责产生高频信号，PLL 负责纠正频率和相位偏差。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/phase-locked-loop-pll-fundamentals.html?utm_source=openai))

一句话记：**晶振提供稳定的“基准节拍”，VCO 是可调的高频“发声器”，PLL 用反馈让它按目标节拍工作。**
