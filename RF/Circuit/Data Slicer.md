**Data Slicer（数据切片器）**就是把**解调后的模拟波形判成数字 0 和 1**的电路。最简单的实现是一个**比较器**：输入电压高于门限，输出高电平；低于门限，输出低电平。它常见于 ASK/OOK 等接收机。([analog.com](https://www.analog.com/en/resources/technical-articles/2022/07/16/09/27/data-slicing-techniques-for-uhf-ask-receivers.html?utm_source=openai))

接上你前面问的 **Sallen–Key filter**，可以这样看：

**天线 → RF 接收电路 → 解调器 → Sallen–Key 数据滤波器 → Data Slicer → 数字数据输出**

滤波器先让模拟数据波形更干净；Data Slicer 再决定每一段是高电平还是低电平。有些接收芯片的数据手册就是按这两个相邻模块画的。([analog.com](https://www.analog.com/media/en/technical-documentation/data-sheets/MAX7036.pdf?utm_source=openai))

例如，若门限设为 **1.5 V**：

- 解调波形为 **2.0 V** → 输出 **1**
- 解调波形为 **1.0 V** → 输出 **0**

**难点在“门限怎么定”。**收到的信号强弱会变化，所以门限常由 RC 平均电路或峰值检测电路随信号调整；门限不合适，或波形在门限附近受噪声扰动，就可能把位判错。([analog.com](https://www.analog.com/en/resources/technical-articles/2022/07/16/09/27/data-slicing-techniques-for-uhf-ask-receivers.html?utm_source=openai))

它和 **ADC** 不一样：ADC 输出多位数字采样值，保留波形幅度信息；Data Slicer 通常直接输出**一位高/低电平**，相当于回答“现在更像 1 还是 0”。它也不是在 GHz 射频上直接切片，而是在**解调、滤波之后**处理数据波形。([analog.com](https://www.analog.com/media/en/technical-documentation/data-sheets/MAX7036.pdf?utm_source=openai))