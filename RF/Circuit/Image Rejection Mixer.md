**Image Rejection Mixer（镜像抑制混频器）**是一种在**降频的同时，尽量抑制“镜像频率”干扰**的混频器。它要解决的问题是：普通混频器可能把两个不同的 RF 频率，都搬到同一个 IF 频率。([analog.com](https://www.analog.com/en/resources/app-notes/an-1258.html?utm_source=openai))

沿用我们前面的数字：**LO = 2.3 GHz，目标 IF = 100 MHz**。

| 进入混频器的信号 | 与 LO 的频率差 | 结果 |
|---|---:|---|
| **2.4 GHz**：想接收的信号 | \|2.4 − 2.3\| = **100 MHz** | 目标 IF |
| **2.2 GHz**：镜像频率上的干扰 | \|2.2 − 2.3\| = **100 MHz** | 也落到同一个 IF |

所以，**2.2 GHz 是这个例子中的镜像频率**。如果它上面恰好有很强的干扰，普通混频器后面仅靠一个选取 100 MHz 的 IF 滤波器，就很难再把它和目标信号分开。([analog.com](https://www.analog.com/en/resources/app-notes/an-1258.html?utm_source=openai))

镜像抑制混频器常用 **I/Q 两路混频**来处理：给两路混频器提供相差 **90°** 的本振，再按特定相位关系组合两路输出，使**目标信号相加、镜像信号相互抵消**。实际电路有幅度和相位误差，因此通常只能“抑制”，不能完美消除；抑制能力常用 **Image Rejection Ratio（IRR，镜像抑制比）**表示。([markimicrowave.com](https://markimicrowave.com/assets/8b7c19ad-0777-48af-8194-d69a8bd57ff2/MMIQA-0626HPSM-Integrated%20Drive%20GaAs%20MMIC%20IQ%20Mixer.pdf?utm_source=openai))

一句话记：**普通混频器负责搬频；Image Rejection Mixer 在搬频时，还尽量避免 LO 另一侧的镜像信号混进同一个 IF。**



可以。接着刚才的 **Image Rejection Mixer（镜像抑制混频器）**说：**I/Q 的关键不是“把信号分成两份”本身，而是让两份信号用相差 90° 的本振分别混频。这样，目标信号和镜像信号在两路输出中会呈现不同的相位关系，后面就能用相加、相消把它们分开。**([analog.com](https://www.analog.com/en/resources/design-notes/optimizing-performance-wideband-direct-conversion-receivers.html))

### 1. I 和 Q 是什么？

- **I = In-phase（同相）**：用一份本振，例如 `cos(ωLOt)`，来混频。
- **Q = Quadrature（正交）**：用相对 I 路差 **90°** 的本振，例如 `sin(ωLOt)`，来混频。

这里的 I、Q **不是分别代表数字 0 和 1**，而是同一个接收信号的两种相位观察结果。([analog.com](https://www.analog.com/en/resources/design-notes/optimizing-performance-wideband-direct-conversion-receivers.html))

```text
                         ┌→ 混频器 × cos(ωLOt) → 滤波 → I ─┐
RF 输入 → 分成两路 ────────┤                               ├→ 相位处理与合成 → 所需 IF
                         └→ 混频器 × sin(ωLOt) → 滤波 → Q ─┘
                                   ↑
                         两路 LO 相差 90°
```

**注意：只有两路 I/Q 输出，还不等于已经抑制镜像。**还要按正确的相位关系处理并合成两路；这一步可以用模拟的 IF 90° 混合器，也可以在 I/Q 分别经过 ADC 后以数字方式完成。([analog.com](https://www.analog.com/en/resources/design-notes/optimizing-performance-wideband-direct-conversion-receivers.html))

### 2. 为什么它能认出镜像？

沿用前面的例子：**LO = 2.3 GHz**，目标在 **2.4 GHz**，镜像在 **2.2 GHz**。两者离 LO 都是 100 MHz，所以普通混频器会把它们都变成 **100 MHz IF**。但两者分别位于 LO 的**上侧**和**下侧**。([analog.com](https://www.analog.com/en/resources/design-notes/optimizing-performance-wideband-direct-conversion-receivers.html))

假设两路理想匹配，并暂时省略共同的幅度系数，混频和滤波后的结果是：

| RF 输入 | I 路输出 | Q 路输出 |
|---|---|---|
| 目标：LO + 100 MHz | `cos(2π·100 MHz·t)` | `−sin(2π·100 MHz·t)` |
| 镜像：LO − 100 MHz | `cos(2π·100 MHz·t)` | `+sin(2π·100 MHz·t)` |

**看 I 路，两者一样；再看 Q 路，符号却相反。**这就是 I/Q 能区分“LO 上方 100 MHz”和“LO 下方 100 MHz”的关键。单看普通混频器输出的 100 MHz 频率，已经无法知道它原来在哪一侧。([analog.com](https://www.analog.com/en/resources/design-notes/optimizing-performance-wideband-direct-conversion-receivers.html))

### 3. 怎么把镜像抵消？

在上述**特定的相位约定**下，如果把 Q 路的 100 MHz 信号再移相 **−90°**，则：

- 目标：I 路是 `cos`，移相后的 Q 路也是 `cos` → **相加增强**。
- 镜像：I 路是 `cos`，移相后的 Q 路是 `−cos` → **相加抵消**。

```text
目标： cos + cos    → 保留
镜像： cos + (−cos) → 抵消
```

改用另一种合成关系，就可以反过来保留 LO 下侧、抑制上侧。实际器件的端口标记和相位方向可能不同，所以接线要以具体数据手册为准。([analog.com](https://www.analog.com/en/resources/design-notes/optimizing-performance-wideband-direct-conversion-receivers.html))

**现实中不会抵消得完全干净**：I/Q 两路的增益不一致、相位不是精确的 90°，都会让镜像残留。这就是为什么指标叫“镜像**抑制**”，而不是“镜像消除”。([analog.com](https://www.analog.com/en/resources/design-notes/optimizing-performance-wideband-direct-conversion-receivers.html))

一句话记住：**普通混频只得到“离 LO 多远”；I/Q 两路还保留了“在 LO 哪一侧”的信息，因此可以选一侧、压另一侧。**