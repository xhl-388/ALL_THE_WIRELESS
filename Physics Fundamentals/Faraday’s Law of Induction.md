**Faraday’s Law of Induction（法拉第电磁感应定律）**是 Maxwell 方程组里的第三个方程。它描述的是：

> **变化的磁场，会产生电场。**

这句话非常重要，因为它解释了**发电机、变压器、无线充电、天线、电磁波**等大量东西。

---

## 1. 先看公式

积分形式：

$$
\boxed{ \oint_C \mathbf E\cdot d\mathbf l = -\frac{d}{dt} \int_S \mathbf B\cdot d\mathbf A }
$$

右边这一坨：

$$
\int_S \mathbf B\cdot d\mathbf A
$$

叫做 **磁通量（Magnetic Flux）**：

$$
\boxed{\Phi_B=\int_S\mathbf B\cdot d\mathbf A}
$$

所以可以简单写成：

$$
\boxed{ \mathcal E=-\frac{d\Phi_B}{dt} }
$$

其中 $\mathcal E$ 是**感应电动势（EMF）**。

最重要的就是：

$$
\boxed{\text{磁通量发生变化}\Rightarrow\text{产生感应电动势}}
$$

---

# 2. 最经典的例子：磁铁 + 线圈

想象一个线圈：

```text
          线圈
       ┌────────┐
       │        │
       │        │
       └────────┘
             ↑
             │
             │
            N S
           磁铁
```

现在把磁铁向线圈移动：

```text
磁铁靠近线圈

N S  → → →  [ 线圈  ]
```

磁铁产生的磁场穿过线圈。

当磁铁移动的时候：

$$
\Phi_B
$$

发生变化。

于是：

$$
\boxed{\mathcal E=-\frac{d\Phi_B}{dt}}
$$

线圈里面就会产生**感应电压**。

如果线圈接上灯泡：

```text
        ┌────💡────┐
        │          │
        └──────────┘
              ↑
             N S
```

磁铁运动时，灯泡就可能亮。

---

# 3. 为什么一定强调“变化”？

这是 Faraday's Law 最容易理解错的地方。

假设磁铁静静地放在线圈旁边：

```text
N S       [线圈]
```

虽然这里存在磁场：

$$
B\neq0
$$

但是如果磁场不发生变化：

$$
\frac{d\Phi_B}{dt}=0
$$

那么：

$$
\boxed{\mathcal E=0}
$$

所以：

> **有磁场 ≠ 一定产生感应电压**

而是：

> **磁通量变化 → 感应电压**

---

# 4. 怎么让磁通量变化？

磁通量：

$$
\Phi_B=\int B\cdot dA
$$

对于一个简单的均匀磁场，可以近似成：

$$
\boxed{\Phi_B=BA\cos\theta}
$$

所以只要下面三个东西中的任何一个变化，都可以产生感应电压：

### ① 磁场 $B$ 变化

例如：

```text
B小 → B大
```

### ② 面积 $A$ 变化

例如一个金属框在磁场里运动：

```text
┌───────┐
│       │ → →
│       │
└───────┘
```

### ③ 角度 $\theta$ 变化

比如转动线圈：

```text
      ↻
   ┌──────┐
   │线圈  │
   └──────┘
```

这就是**发电机**的基本原理。

---

# 5. 那个负号是什么意思？

公式：

$$
\boxed{ \mathcal E=-\frac{d\Phi_B}{dt} }
$$

这个 **−** 非常重要。

它代表：

> **感应出来的电流产生的磁场，会阻碍原来磁通量的变化。**

这叫 **Lenz's Law（楞次定律）**。

例如：

```text
      磁铁
       N
       ↓
       ↓ 运动
       ↓
    ┌───────┐
    │ 线圈  │
    └───────┘
```

N 极正在靠近线圈。

线圈会产生一个磁场，使靠近磁铁的一侧也变成 **N 极**：

```text
      N
      ↓
      ↓
     N │ 线圈
       │
```

两个 N 极互相排斥。

也就是说：

> **线圈产生的磁场在“反抗”磁铁靠近造成的磁通量增加。**

这就是负号的物理意义。

---

# 6. 这和发电机有什么关系？

发电机本质上就是：

> **让磁通量不断变化 → 不断产生感应电压。**

比如转动一个线圈：

```text
       磁场 B
     → → → → →

          ↻
       ┌─────┐
       │线圈 │
       └─────┘

     → → → → →
```

线圈不断旋转：

$$
\theta(t)
$$

不断变化：

$$
\Phi_B=BA\cos\theta
$$

所以：

$$
\frac{d\Phi_B}{dt}\neq0
$$

于是产生：

$$
\mathcal E\neq0
$$

这就是发电。

---

# 7. 最关键的一点：它甚至不需要“导线运动”

这一点对理解 RF / 天线非常重要。

Faraday 定律真正说的是：

$$
\boxed{\text{变化的磁场产生旋转的电场}}
$$

微分形式：

$$
\boxed{ \nabla\times\mathbf E = -\frac{\partial\mathbf B}{\partial t} }
$$

注意这里出现了：

$$
\boxed{\nabla\times\mathbf E}
$$

也就是 **电场的 curl（旋度）**。

意思是：

如果：

$$
\frac{\partial B}{\partial t}\neq0
$$

那么：

$$
\nabla\times E\neq0
$$

也就是说，电场本身可以形成一种**环形结构**：

```text
          ↗ → → ↘
        ↑         ↓
        ↑    B    ↓
        ↑         ↓
          ↖ ← ← ↙
```

这和我们高中里经常想象的“电场线从正电荷出发，到负电荷结束”有点不一样。

**变化的磁场可以直接产生这种闭合的电场。**

---

# 8. 这就开始和 RF 联系起来了

你之前问的 RF、EVM、IQ imbalance 等东西，往后学到**天线和电磁波**时，这个定律会变得非常重要。

Maxwell 方程组的逻辑可以先记成：

$$
\boxed{ \text{变化的磁场} \rightarrow \text{电场} }
$$

而另一个方程：

$$
\boxed{ \nabla\times B = \mu_0J+ \mu_0\epsilon_0\frac{\partial E}{\partial t} }
$$

告诉你：

$$
\boxed{ \text{电流/变化的电场} \rightarrow \text{磁场} }
$$

于是就形成：

```text
变化的 E
   ↓
产生 B
   ↓
变化的 B
   ↓
产生 E
   ↓
变化的 E
   ↓
产生 B
   ↓
……
```

这就是**电磁波能够自己传播出去**的核心。

所以从学习路线来说，你现在这几个定律其实正好是在逐渐拼出 Maxwell 方程组：

$$
\boxed{ \begin{array}{c} \text{Gauss's Law} \\ \text{Gauss's Law for Magnetism}\\ \text{Faraday's Law}\\ \text{Ampère-Maxwell Law} \end{array} }
$$

而下一步最值得搞懂的是 **Ampère-Maxwell Law**，因为把它和 Faraday's Law 放在一起，你就能真正理解“**电磁波为什么会自己跑起来**”。
