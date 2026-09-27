**Ampère’s Circuital Law（安培环路定律）**可以先记成一句话：

> **电流会产生环绕着电流的磁场。**

而它和刚才的 Faraday's Law 正好可以看成一对：

$$
\boxed{\text{电流} \rightarrow \text{磁场}}
$$

而 Faraday：

$$
\boxed{\text{变化的磁场} \rightarrow \text{电场}}
$$

---

## 1. 最原始的形式

Ampère's Circuital Law：

$$
\boxed{ \oint_C \mathbf B\cdot d\mathbf l = \mu_0 I_{\rm enc} }
$$

先不要被公式吓到。

左边：

$$
\oint_C \mathbf B\cdot d\mathbf l
$$

可以理解成：

> **沿着一个封闭的圆圈，把磁场在这个圆圈方向上的分量全部加起来。**

右边：

$$
\mu_0 I_{\rm enc}
$$

就是：

> **这个圈里面穿过了多少电流。**

所以最简单地说：

$$
\boxed{ \text{磁场绕着电流转} }
$$

---

# 2. 最经典的例子：一根通电导线

想象一根无限长的导线：

```text
             ↑
             │ I
             │
             │
             │
```

电流向上。

那么导线周围会产生环形磁场：

```text
              ↺
          ↺         ↺
       ↺      │ I      ↺
              │
       ↺      │        ↺
          ↺         ↺
              ↺
```

也就是说：

$$
\boxed{ I\rightarrow B }
$$

磁场不是从导线向外直线跑，而是**绕着导线转圈**。

---

# 3. 右手定则

这个方向可以用 **Right-hand rule（右手定则）**判断。

右手：

- 大拇指指向电流 $I$

- 四根手指弯曲的方向，就是磁场 $B$ 的方向

例如电流向上：

```text
          ↑
          │
          │ I
          │
```

右手大拇指向上，手指就会绕着导线：

```text
          ↺
       ↺  │  ↻
          │
       ↻  │  ↺
          │
```

这在 RF、电机、电磁铁里面非常常见。

---

# 4. 用它计算磁场

假设有一根无限长直导线，电流为：

$$
I
$$

距离导线：

$$
r
$$

画一个半径 $r$ 的圆：

```text
             B
          ↺──────↻
        ↺          ↻
       │      ●      │
        ↻          ↺
          ↺──────↻
              ↑
              I
```

由于对称性：

$$
B
$$

在整个圆上大小都一样。

所以：

$$
\oint B\,dl = B(2\pi r)
$$

根据 Ampère's Law：

$$
B(2\pi r)=\mu_0I
$$

于是：

$$
\boxed{ B=\frac{\mu_0 I}{2\pi r} }
$$

这就是无限长直导线周围的磁场。

你可以看到：

$$
\boxed{B\propto\frac1r}
$$

离导线越远，磁场越弱。

---

# 5. 为什么叫 Circuital Law？

因为它研究的是一个**闭合路径（closed loop）**。

比如：

```text
       ┌─────────┐
       │         │
       │    ●    │
       │         │
       └─────────┘
```

你沿着这个闭合路径走一圈：

$$
\oint\mathbf B\cdot d\mathbf l
$$

然后看：

> 这个路径里面包了多少电流？

如果里面有：

$$
I
$$

那么：

$$
\oint\mathbf B\cdot d\mathbf l = \mu_0I
$$

---

# 6. 但是这里有一个非常重要的问题

原来的 Ampère Law：

$$
\boxed{ \nabla\times B=\mu_0J }
$$

只在**稳恒电流**情况下成立。

也就是说：

> 电流不随时间变化。

但是后来 Maxwell 发现了一个问题。

假设一个电容器正在充电：

```text
       导线
         │
         │ I
         ↓
      ───────
       电容
      ───────
```

两块极板之间：

```text
       + + + + +
       ────────
          ↑
       电场 E
          ↑
       ────────
       - - - - -
```

这里：

**导线上明明有电流，但是电容两块极板之间没有真正的导电电流。**

那么按照原来的 Ampère Law：

$$
\oint B\cdot dl=\mu_0I
$$

你会发现一个矛盾。

---

# 7. Maxwell 给它补了一项

Maxwell 发现：

> **变化的电场也会产生磁场。**

于是把 Ampère Law 修改成：

$$
\boxed{ \oint_C\mathbf B\cdot d\mathbf l = \mu_0I_{\rm enc} + \mu_0\epsilon_0 \frac{d\Phi_E}{dt} }
$$

这就是完整的：

**Ampère-Maxwell Law**

微分形式：

$$
\boxed{ \nabla\times\mathbf B = \mu_0\mathbf J + \mu_0\epsilon_0 \frac{\partial\mathbf E}{\partial t} }
$$

这里第二项：

$$
\boxed{ \mu_0\epsilon_0 \frac{\partial E}{\partial t} }
$$

就是非常著名的：

**Displacement Current（位移电流）**

---

# 8. 现在把 Faraday 和 Ampère 放一起

你刚刚问过 Faraday：

$$
\boxed{ \nabla\times E = -\frac{\partial B}{\partial t} }
$$

Ampère-Maxwell：

$$
\boxed{ \nabla\times B = \mu_0J+ \mu_0\epsilon_0 \frac{\partial E}{\partial t} }
$$

如果没有真实电流：

$$
J=0
$$

那么：

$$
\boxed{ \nabla\times B = \mu_0\epsilon_0 \frac{\partial E}{\partial t} }
$$

于是就出现了一个非常漂亮的关系：

```text
       变化的 E
          │
          ↓
       产生 B
          │
          ↓
       变化的 B
          │
          ↓
       产生 E
          │
          ↓
       变化的 E
          │
          ↓
          ...
```

这就是**电磁波传播的核心机制**。

---

## 9. 四个 Maxwell 方程现在可以串起来了

你刚才连续问的三个，加上最后一个：

| 方程                          | 核心意思                |
| ----------------------------- | ----------------------- |
| **Gauss's Law**               | 电荷产生电场            |
| **Gauss's Law for Magnetism** | 没有磁单极子            |
| **Faraday's Law**             | 变化的磁场产生电场      |
| **Ampère-Maxwell Law**        | 电流/变化的电场产生磁场 |

可以把它们粗略记成：

$$
\boxed{ Q\rightarrow E } \qquad
\boxed{ \frac{\partial B}{\partial t}\rightarrow E } \qquad
\boxed{ I\rightarrow B } \qquad
\boxed{ \frac{\partial E}{\partial t}\rightarrow B }
$$

其中最后两个：

$$
\boxed{ \frac{\partial B}{\partial t}\rightarrow E \qquad \frac{\partial E}{\partial t}\rightarrow B }
$$

互相“喂”对方，这就是你之后理解 **RF → 天线 → 电磁波 → 无线通信** 时非常核心的一条线。
