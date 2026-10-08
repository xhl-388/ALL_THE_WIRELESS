**Friis Equation（弗里斯传输公式）的意义是：在理想自由空间中，估算“发射出去的 RF 功率，有多少能到达接收天线”。**它是做无线链路预算的起点：先估算接收功率，再判断它是否足够让接收机解调 Wi‑Fi、FSK 等信号。([ntrs.nasa.gov](https://ntrs.nasa.gov/api/citations/19960027987/downloads/19960027987.pdf?utm_source=openai))

考虑两副天线相互对准、距离为 `r`，公式是：

```text
Pr = Pt × Gt × Gr × (λ / 4πr)²
```

- `Pt`：进入发射天线的功率；
- `Gt`、`Gr`：发射、接收天线在彼此方向上的增益，公式中用**线性倍数**，不是 dBi 数值；
- `λ`：波长；
- `Pr`：接收天线可获得的功率。

这里的 `(λ / 4πr)²` 把距离和波长的影响写在一起。([ntrs.nasa.gov](https://ntrs.nasa.gov/api/citations/19960027987/downloads/19960027987.pdf?utm_source=openai))

### 它和刚才的 EIRP、power density 怎么接起来？

其实 Friis 公式可以拆成两步：

**第一步：算接收天线所在位置的空间功率密度。**

```text
EIRP = Pt × Gt
S = EIRP / (4πr²)
```

`S` 的单位是 W/m²。距离越远，同一方向的功率分摊到越大的球面面积上。([ntrs.nasa.gov](https://ntrs.nasa.gov/api/citations/20210021603/downloads/Power_Distribution_Study_Final.pdf?utm_source=openai))

**第二步：算接收天线能从中接住多少。**

```text
Pr = S × Aeff
Aeff = Gr × λ² / (4π)
```

`Aeff` 叫接收天线的**有效接收面积**，不是拿尺子量出的外形面积。把这两步合起来，就得到了上面的 Friis 公式。这个拆法也说明：**EIRP 描述发射方向上的能力，功率密度描述信号到达某处时每平方米有多少功率，Friis 最终算的是接收天线拿到了多少功率。**([ntrs.nasa.gov](https://ntrs.nasa.gov/api/citations/19960027987/downloads/19960027987.pdf?utm_source=openai))

### 用一个 2.4 GHz 的例子

假设发射天线输入功率为 **20 dBm**，发射和接收天线增益各为 **2 dBi**，两天线相距 **10 m**。换成常用的 dB 形式：

```text
接收功率 (dBm)
  = 发射天线输入功率 (dBm)
  + 发射天线增益 (dBi)
  + 接收天线增益 (dBi)
  − 自由空间路径损耗 (dB)
```

2.4 GHz、10 m 的自由空间路径损耗约为 **60 dB**，所以：

```text
Pr ≈ 20 + 2 + 2 − 60 = −36 dBm
```

如果距离从 **10 m 增至 20 m**，其他条件不变，理想自由空间中的接收功率会再降低约 **6 dB**，变成约 **−42 dBm**。这就是 Friis 公式里 `1/r²` 的直接结果。([itu.int](https://www.itu.int/en/ITU-R/study-groups/Documents/ITU-R_Reference_Assist/Beta_Test_Version.html?utm_source=openai))

**但不要把 −36 dBm 当成真实室内 Wi‑Fi 的保证值。**Friis 假设远场、理想自由空间传播，并且需要考虑天线方向、极化匹配等条件；墙壁、人体遮挡和反射造成的多径，都可能让实际接收功率偏离估算。它更像链路预算的**理想基线**，不是室内覆盖预测的完整模型。([ntrs.nasa.gov](https://ntrs.nasa.gov/api/citations/20090029239/downloads/20090029239.pdf?utm_source=openai))

最后，Friis 算出的是**收到多少功率**，不直接告诉你能否使用 256‑QAM 或 1024‑QAM。还要把接收功率与噪声、干扰和接收机所需的信号质量比较，才能判断链路是否可靠。([nasa.gov](https://www.nasa.gov/smallsat-institute/sst-soa/soa-communications/?utm_source=openai))