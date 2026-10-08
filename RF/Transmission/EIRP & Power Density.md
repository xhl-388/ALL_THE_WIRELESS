**要先看你说的 power density 的单位。**在 RF 里，它常指两种不同的“密度”；它们都能和 EIRP 联系起来，但**除的东西不一样**：

| 名称 | 常见单位 | 问的是 |
|---|---|---|
| **功率谱密度**（PSD） | dBm/MHz、W/Hz | 发射功率在**频率上**分布得有多密？ |
| **空间功率密度**（power flux density） | W/m² | 到了某个位置，功率在**面积上**分布得有多密？ |

**EIRP** 则是把发射功率和天线在某个方向上的增益合在一起，表示“若换成理想的各向同性天线，要发多大功率才能在该方向产生同样的效果”。简化地用 dB 计算：

```text
EIRP (dBm) = 发射机输出功率 (dBm)
           − 馈线等损耗 (dB)
           + 该方向的天线增益 (dBi)
```

所以方向不同，方向性天线对应的 EIRP 也可能不同。([itu.int](https://www.itu.int/dms_pub/itu-r/opb/rep/R-REP-BS.2037-2004-PDF-E.pdf?utm_source=openai))

### 如果你说的是 dBm/MHz：EIRP 与功率谱密度

假设某方向的**总 EIRP 是 20 dBm**，约为 **100 mW**。如果功率**近似均匀**地分布在 10 MHz 内，那么平均每 1 MHz 分到 10 mW，即：

```text
平均 EIRP 谱密度 ≈ 10 dBm/MHz
```

同样是 20 dBm 的总 EIRP，信号若铺到更宽的频带，**每 MHz 的平均功率就会降低**。这与你前面问的扩频有关：扩频可以把功率分散到更宽的频带，但不会仅凭“铺开”就增加总 EIRP。实际频谱通常不完全平坦，因此**平均 dBm/MHz 不能直接当作频谱中最高的 dBm/MHz**。EIRP 谱密度可以理解为发射端功率谱密度再计入相应方向的天线增益。([itu.int](https://www.itu.int/dms_pubrec/itu-r/rec/f/R-REC-F.1247-3-201302-S%21%21PDF-E.pdf?utm_source=openai))

### 如果你说的是 W/m²：EIRP 与空间功率密度

在**自由空间、远场**，沿所讨论的方向，距离天线 `r` 米处可用下面的关系估算：

```text
空间功率密度 (W/m²) = 该方向的 EIRP (W) ÷ (4πr²)
```

例如该方向 EIRP 为 **0.1 W**，距离 **2 m**，则功率密度约为 **0.002 W/m²**。距离增加一倍，按这个理想模型，功率密度变为四分之一。近场、遮挡和反射等情况下，不能直接套用这个简单公式。([itu.int](https://www.itu.int/dms_pub/itu-r/opb/rep/R-REP-BS.2037-2004-PDF-E.pdf?utm_source=openai))

**一句话记：**`dBm/MHz` 是“每单位**频宽**分多少功率”；`W/m²` 是“传播到某处后每单位**面积**有多少功率”。你看到的 power density 如果带着具体单位，我就能按那个单位继续讲它和 EIRP 怎么换算。