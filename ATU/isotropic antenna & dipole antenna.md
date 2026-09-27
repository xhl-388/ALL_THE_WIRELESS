这两个天线最大的区别可以先记成一句话：

> **Isotropic antenna（各向同性天线）是一个“理想模型”，Dipole antenna（偶极子天线）是真实存在的天线。**

### 1. Isotropic antenna：理想的“球形天线”

各向同性天线假设：

> **向所有方向辐射完全一样的功率。**

想象一个点光源：

```text
              ↑
           ↗  │  ↖
         ↗    │    ↖
       ←──────●──────→
         ↘    │    ↙
           ↘  │  ↙
              ↓
```

3D 中实际上是一个**球形辐射模式**：

```text
          ______
       .-'      '-.
     .'            '.
    /       ●        \
    \                /
     '.            .'
       '-.______.-'
```

所以它的增益定义为：

$$
G=1
$$

也就是：

$$
0\text{ dBi}
$$

注意：

**现实中不存在真正的 isotropic antenna。**

它主要用来作为 RF 里的**参考天线**。

---

# 2. Dipole antenna：真实的偶极子天线

最经典的是半波偶极子：

```text
       金属导体
──────────┐
          │
          │ Feed
          │
──────────┘
       金属导体
```

典型的 **half-wave dipole（半波偶极子）**长度大约：

$$
L\approx\frac{\lambda}{2}
$$

例如 2.4 GHz：

$$
\lambda=\frac{c}{f} \approx12.5\,\text{cm}
$$

所以半波偶极子总长度大约：

$$
L\approx6.25\,\text{cm}
$$

实际设计还会因为导体、环境等因素进行调整。

---

# 3. 最大区别：辐射方向

这才是最重要的。

Isotropic：

```text
         ↑
      ↗  │  ↖
    ←────●────→
      ↘  │  ↙
         ↓

所有方向一样
```

Dipole：

假设偶极子竖直放置：

```text
          ↑
          │
          │
          │
          │
          │
          ↓
```

它的辐射方向大概是：

```text
             ↑
             │
             │  ← 较弱
             │
      ←──────┼──────→  最强
             │
             │
             │  ← 较弱
             │
             ↓
```

所以从侧面看，它的辐射非常强。

从天线轴向看：

```text
      Dipole
        │
        │
        │
        ● → 这个方向几乎不辐射
        │
        │
        │
```

这就是经典的 **donut-shaped radiation pattern（甜甜圈形辐射模式）**。

![Image](https://images.openai.com/static-rsc-4/zxz-AWEj-SPNEY0VESoSw34Ya5Xy1RWMOe2Nj2qp8nMMz0bMqkn82XVVXinRG1zdkYEn6oCz1Dvn5TgEyz15eiZongKyCq8J9j2mRfAI4VfCXQuYmVnk6aCNsB_WjWu2TKuQvJnG77zWXzkC-GF5169WM6tt_ump5sKviBt4jtmPZd3eRt_gYvw96nMFm-br?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/PyqFsTc8fayQ0rJx3cG_QRAwp5GocTwofNfk-cYCYd5mIzO4YKdZ9_5h82bmeZ2AfNvZjvKCJ0SfvPiGcj4hIOCRVfaX9kRHbQUMA5xeqXVpFgy9JscfBrEj8Ghfn8HaFxEXjz9iGAArl3OryLWzycUizNSXUQveyE7KafjvhyF8nN4LHBQwFymyTj5_yGAm?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Afg1QWqPQQlnr9pohcXuwLhS8Oe-ioAm8DyX7p2mwbEKXJI0oiQPFs-LZRuTYLEQIdvxeV5WJpyBZnrJw2EYECDkDalPGQix2xA3tfVat5yzJzuCrV_lu2yFjKTANL45yQMZdBBbfBLZviE2pSms-Pi7uvBDFb_2KvMRgFISrXj4BA0mYcAjx7ZQM2eQmDM7?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/mZNxPMHuCb3ESboUb95gMcQY5xjxTn3GgkEA4j5Jne1wDbrhpT0z4029ZoZxevlnL_Qmp1CvYbft7nyIbRsv-WsKOpz6WH6QU8iEr5jeyir0kvSS5FPeQuTgHYKjvBwbOMcRks-VnvEcQ-CKU405M5vxm3_WlnGyjBQVobRJmsYUed9sUJbwbAtUjSHWXiEu?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/0aZ0C_EJ7g5DZMUomeF7txQDomynSFkVAaAcWhVSOlIfVdmeLQ0gQ6YyOIgoxIZ4pll8tEAJWK_-T0tTrY2B1QzNMVIIUFKJJosJQb5G7K0Nxmov7o5OSSSwqiAmUwnd9tTmdxBiSSgjm33x9ubKo8Wz0yLLi33aZXaOtB-OhnAWiAw_fs5d7aAa8wTEUSep?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gMHpsNteaLobbLWnK6xEMB2Hu9Mzh5ntRv2c9Rd0m20PRhcLxIzLbvbdhD0I2PJ2-coYvP0q9aSQw41SHspT4Tntlqspf4mzs3ajtzC4Ocs5FZi1g4vS_aHoVnHOpWGFycGxEDUiKp5mJ_6b6vtNDDOd6n48UFU9vSyL5kCBjkXQGBmGlrp6Bp8B7g57ssUy?purpose=fullsize)

---

# 4. 为什么 Dipole 会这样？

这个和电磁波的产生方式有关。

偶极子上有来回变化的电流：

```text
       ↑ current
       │
       │
───────┼───────
       │
       │
       ↓ current
```

变化的电流产生变化的电场和磁场，最终向空间辐射电磁波。

对于理想短偶极子，它的远场辐射强度大致满足：

$$
P(\theta)\propto\sin^2\theta
$$

这里：

- $\theta=0^\circ$：沿着天线轴

- $\theta=90^\circ$：垂直于天线

所以：

$$
P(0^\circ)=0
$$

而：

$$
P(90^\circ)=P_{\max}
$$

这就是为什么偶极子在**垂直于天线的方向辐射最强**。

---

# 5. 增益也不一样

这是你在 RF 里面经常会遇到的。

理想 isotropic：

$$
G=1
$$

换成 dBi：

$$
G_{\mathrm{dBi}}=0
$$

而理想半波偶极子：

$$
G\approx1.64
$$

所以：

$$
G_{\mathrm{dBi}}=10\log_{10}(1.64)\approx2.15\text{ dBi}
$$

因此：

> **理想半波偶极子的最大增益约为 2.15 dBi。**

这也是为什么经常看到：

```text
Dipole antenna gain ≈ 2.15 dBi
```

---

# 6. dBi 和 dBd 就是从这里来的

你以后看 RF 文档很容易遇到：

### dBi

以 isotropic antenna 为参考：

$$
0\text{ dBi}=1\times G_{\text{isotropic}}
$$

### dBd

以 dipole 为参考：

$$
0\text{ dBd}=1\times G_{\text{dipole}}
$$

因为理想半波偶极子比 isotropic 高：

$$
2.15\text{ dB}
$$

所以：

$$
\boxed{0\text{ dBd}=2.15\text{ dBi}}
$$

或者：

$$
\boxed{G_{\rm dBi}=G_{\rm dBd}+2.15}
$$

---

## 7. 和你现在研究的 Wi-Fi RF 联系起来

比如你的 Wi-Fi 芯片发射功率：

$$
P_{\rm TX}=20\text{ dBm}
$$

假设天线是：

$$
G=3\text{ dBi}
$$

那么自由空间方向上最大辐射功率可以粗略理解为：

$$
20+3=23\text{ dBm}
$$

而如果有人说：

> `20 dBm EIRP`

这里的 **EIRP** 就是：

$$
\boxed{\text{EIRP}=P_{\rm TX}+G_{\rm antenna}}
$$

它实际上就是**假设用一个 isotropic antenna，要达到相同最大辐射强度，需要多少功率**。

所以你可以把这几个概念串起来：

```text
                 参考标准
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
   Isotropic antenna       Dipole antenna
      0 dBi                 2.15 dBi
          │                   │
          └─────────┬─────────┘
                    ↓
               Antenna Gain
                    ↓
                 EIRP
```

**最核心的区别就是：Isotropic 是“所有方向一样”的理论参考天线，而 Dipole 是实际天线，其辐射呈甜甜圈形，并且理想半波偶极子最大增益约 2.15 dBi。**
