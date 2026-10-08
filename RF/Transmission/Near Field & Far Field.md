**近场（near field）和远场（far field）的意义，是告诉你：离天线多远时，可以把它当作一个向外辐射的波源，用我们刚讲的 EIRP、功率密度和 Friis 公式来估算；多近时，这些简化模型就不可靠了。**这里的“近”“远”不是固定的几厘米或几米，而与**波长和天线尺寸**有关。([standards.nasa.gov](https://standards.nasa.gov/system/files/tmp/GSFC-STD-8715.1A_Adm%20Ext_2_0.pdf?utm_source=openai))

## 先接上 Friis 公式

我们前面用过：

```text
空间功率密度 S ≈ EIRP / (4πr²)
接收功率 Pr ≈ S × 接收天线有效面积
```

这个思路适用于**自由空间中的远场**：离天线足够远后，在接收点附近，电磁波的行为可以近似看作向外传播的波，天线的方向图也基本不再随测量距离变化。因此，距离翻倍，功率密度约变为四分之一。([keysight.com](https://www.keysight.com/zz/en/assets/9018-07604/article-reprints/9018-07604.pdf?utm_source=openai))

**在近场，不能简单把实际距离塞进这个公式。**原因要看近场是哪一种。

## 近场其实有两层

| 区域 | 主要特点 | 工程上意味着什么 |
|---|---|---|
| **反应近场**（reactive near field） | 紧贴天线；有明显的能量在天线与周围场之间往返储存 | 附近的物体或另一副天线可能改变天线的阻抗、匹配和工作状态 |
| **辐射近场**（radiating near field，也叫 Fresnel 区） | 已经有向外传播的能量，但场的空间分布、方向图仍随距离变化 | 不能直接套用稳定的远场方向图；测天线时可能需要近场扫描再换算成远场结果 |
| **远场**（far field） | 辐射波占主导，方向图随距离基本稳定 | 适合用天线增益、EIRP、Friis 公式做常规链路预算 |

所以**“近场”不等于“没有辐射”**：反应近场和辐射近场是不同情况。NASA 的天线场区资料也作了这三区的区分。([standards.nasa.gov](https://standards.nasa.gov/system/files/tmp/GSFC-STD-8715.1A_Adm%20Ext_2_0.pdf?utm_source=openai))

## 怎么判断远场从哪里开始？

对天线方向图测量，常见的远场距离估算是：

```text
r ≥ 2D² / λ
```

其中 `D` 是天线的最大尺寸，`λ` 是波长。这是常用的**远场判据**，不是到了这条线电磁场突然发生变化；对小天线，还要留意波长尺度等条件。([keysight.com](https://www.keysight.com/us/en/assets/9018-06267/reference-guides?utm_source=openai))

举个与 Wi‑Fi 接近的数字：在 **2.4 GHz**，波长约 **12.5 cm**。若一副天线阵列的最大尺寸 `D = 0.5 m`：

```text
远场距离 ≈ 2 × (0.5 m)² / 0.125 m = 4 m
```

这表示：若要按这副**半米阵列**的远场方向图来分析，通常要到约 **4 m 或更远**才符合上述判据。**这不是说 4 m 内收不到 Wi‑Fi**；只是说 4 m 内用远场增益和 Friis 公式估算，可能不够准确。天线越大、波长越短，这个远场距离就可能越长。([keysight.com](https://www.keysight.com/us/en/assets/9018-06267/reference-guides?utm_source=openai))

## 实际有什么用？

1. **算链路预算**：先确认收发天线处于适用的远场条件，再用 EIRP、路径损耗和接收天线增益估算接收功率。([keysight.com](https://www.keysight.com/zz/en/assets/9018-07604/article-reprints/9018-07604.pdf?utm_source=openai))
2. **设计天线和摆放设备**：非常靠近天线的外壳、金属或另一副天线可能与近场耦合，改变天线表现；不能只拿“自由空间增益”判断装进产品后的效果。([standards.nasa.gov](https://standards.nasa.gov/system/files/tmp/GSFC-STD-8715.1A_Adm%20Ext_2_0.pdf?utm_source=openai))
3. **测量方向图**：大型阵列的远场距离可能很长，因此实验室会测量近场，再通过数学处理推算远场方向图；NASA 也使用这样的天线测试方法。([nasa.gov](https://www.nasa.gov/jpl/mesa/antenna-range/?utm_source=openai))

**一句话记：近场关注“天线和附近环境如何相互作用”；远场关注“已经辐射出去的波如何传播”。**Friis 公式主要解决后一个问题。