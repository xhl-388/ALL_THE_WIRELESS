**不是。电荷泵不是电容；它是给电容“充电或放电”的电路。**

在 PLL 里，可以这样区分：

```text
相位/频率比较器 → 电荷泵 → 环路滤波器（含电容）→ VCO
                   推/拉电流      储存电荷、平滑成控制电压
```

比较器发现反馈信号与参考信号没对齐时，会让**电荷泵输出电流脉冲**，向环路滤波器送入电流，或从中抽走电流。环路滤波器里的**电容**因此充电或放电，电压随之变化；这个电压再去调整 VCO 的频率。([analog.com](https://www.analog.com/en/resources/technical-articles/an143-a-simple-method-to-accurately-predict-pll-reference-spur-levels-due-to-leakage-current.html?gated=1749849782289&utm_source=openai))

所以，“电荷泵”这个名字容易让人误会：它是**搬运电荷的电路**，电容则是**储存电荷的元件**。我前面说“电荷泵输出脉冲，Loop Filter 把它变成平滑电压”，说的就是这两个模块的配合。([analog.com](https://www.analog.com/media/en/technical-documentation/data-sheets/ada4625-1.pdf?utm_source=openai))