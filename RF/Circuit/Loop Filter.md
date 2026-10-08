**Loop Filter（环路滤波器）**，在我们讨论的 **PLL** 里，是夹在**电荷泵（Charge Pump）和 VCO** 之间的电路。它把电荷泵输出的一串“加速/减速”电流脉冲，变成较平滑的 **VCO 控制电压**，从而调节本振频率。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/designing-plls-with-high-voltage-vcos.html?utm_source=openai))

```text
晶振 → 分频 → 相位/频率比较器 → 电荷泵 → Loop Filter → VCO → LO 给混频器
                  ↑                            控制电压      │
                  └──────── 输出经分频反馈 ──────────────────┘
```

你可以把它想成**方向盘的“平滑器”**：PLL 发现 VCO 跑快或跑慢，就发出修正指令；Loop Filter 不让每个短促的修正脉冲都直接使控制电压剧烈跳动，而是形成适当、持续的调整。它通常由电阻、电容构成，也有使用放大器的有源设计。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/designing-plls-with-high-voltage-vcos.html?utm_source=openai))

它还有一个重要作用：**决定 PLL 调整得多快**。环路带宽较宽，通常锁定或切换频率更快，但噪声、杂散和稳定性也要权衡；带宽较窄，通常响应更慢。不是越窄越好或越宽越好。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/designing-plls-with-high-voltage-vcos.html?utm_source=openai))

**别和前面接收链上的 IF Filter 混淆：**IF Filter 过滤的是*接收到的中频信号*；PLL 的 Loop Filter 处理的是*控制 VCO 的反馈信号*，不在“天线 → 解调器”的数据通路上。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/designing-plls-with-high-voltage-vcos.html?utm_source=openai))
