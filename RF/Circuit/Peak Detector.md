**Peak Detector（峰值检测器）**是用来“记住”一段模拟信号达到过的最高电压的电路。它输出的是一个**随峰值变化的电压**，不是直接输出数字 0/1。([wiki.analog.com](https://wiki.analog.com/university/courses/electronics/text/chapter-7?utm_source=openai))

最简单的正峰值检测器可以这样理解：

```text
输入 ── 二极管 ──●── 输出（峰值电压）
                │
                ├── 电容 ── 地
                │
                └── 电阻 ── 地
```

当输入电压升高到足以使二极管导通时，**电容被充电**；输入电压回落后，二极管截止，电容暂时保持较高的电压，再通过电阻**慢慢放电**。所以输出通常是“上升快、下降慢”。简单二极管电路的输出还会受二极管正向压降影响，并非精确等于输入峰值。([wiki.analog.com](https://wiki.analog.com/university/courses/electronics/text/chapter-7?utm_source=openai))

接到你前面问的 **Data Slicer**：在一些 ASK/FSK 接收机里，峰值检测器会跟踪**解调、滤波后的数据波形**的最高电平；有的还配一个**最低值检测器**。电路可利用最高和最低电平生成切片门限，让 Data Slicer 在信号幅度变化时仍能判断 0/1。这里检测的不是 2.4 GHz 载波的峰值，而是后面那条数据波形的峰值——**具体检测哪一级信号，要看框图连接位置**。([analog.com](https://www.analog.com/en/resources/technical-articles/data-slicing-techniques-for-uhf-ask-receivers.html?utm_source=openai))

一句话区分：**Peak Detector 找并保持峰值；Data Slicer 用门限判 0/1。**