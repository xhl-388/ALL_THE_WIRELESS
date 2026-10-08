**Sallen–Key filter（萨伦–基滤波器）是一种有源模拟滤波电路拓扑**，常用**两个电阻、两个电容和一个运算放大器**做成二阶低通滤波器；把电阻、电容的位置对调，也可以做成高通滤波器。这里的“二阶”表示它比简单的一阶 RC 滤波器能更快地压低不想要的频率。([analog.com](https://www.analog.com/en/resources/analog-dialogue/articles/phase-relations-in-active-filters.html?utm_source=openai))

下面是**单位增益低通型**的简化电路图：

```text
输入 ── R1 ──●── R2 ──●──── 运放 (+)
              │         │        │
              │         C2       ├── 输出
              │         │        │
              └── C1 ───┴────────┘
                   │     地
                   └──────── 输出

运放 (−) 直接接输出
```

上面用文字线条不太容易看出连接关系，关键是：**R1、R2 串在输入通路上；一个电容接地，另一个电容从输出反馈到两只电阻之间；运放在此配置为缓冲器。**这不是单纯把两组 RC 滤波器串起来，输出反馈也参与决定滤波特性。TI 的图示能更准确地看到各节点连接。([ti.com](https://www.ti.com/lit/an/sloa024b/sloa024b.pdf?utm_source=openai))

接到我们前面讨论的 RF 接收链，它**常适合用在混频后的中频或基带模拟信号处理**，例如在 ADC 前做低通、抑制带外信号和帮助抗混叠；它通常**不是**天线后面直接筛选 GHz 射频信号的那个射频滤波器。([ez.analog.com](https://ez.analog.com/precision-technology-signal-chains/a/kwik-circuits/kc4151/design-of-an-anti-aliasing-sallen-key-filter?utm_source=openai))

一句话记：**Sallen–Key = 用 RC 网络加运放实现滤波的一种经典接法，不是某一颗特定的芯片。**