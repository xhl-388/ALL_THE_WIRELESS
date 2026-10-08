**RF 里的 Analog Front-End（AFE，模拟前端）**，就是天线和数字处理部分之间、负责处理**模拟射频信号**的电路。它在接收时把微弱的无线电信号筛选、放大，并送往模数转换；在发射时则把信号放大后送到天线。具体包含哪些器件，会随射频架构而变。([wiki.analog.com](https://wiki.analog.com/resources/eval/developer-kits/2to24ghz-mxfe-rf-front-end?utm_source=openai))

有，先看这个**简化框图**：

**接收（Rx）**  
📡 天线 → 滤波器（去干扰）→ **LNA**（低噪声放大）→ 混频器（降频）→ **ADC**（转成数字信号）→ 数字处理

**发射（Tx）**  
数字处理 → **DAC**（转成模拟信号）→ 混频器（升频）→ **PA**（功率放大）→ 📡 天线

这里的混频器、ADC/DAC 是否算在“AFE”里面，取决于具体产品怎样划分模块；但**天线附近的滤波、放大和收发切换**是理解 RF 前端的核心。ADI 的[实际 RF 收发链路框图](https://www.analog.com/media/en/news-marketing-collateral/solutions-bulletins-brochures/rf-communications-product-selector-guide.pdf)也画出了 LNA、混频器、PA、ADC/DAC 等模块的位置。([analog.com](https://www.analog.com/media/en/news-marketing-collateral/solutions-bulletins-brochures/rf-communications-product-selector-guide.pdf?utm_source=openai))



把它想成一条**把空气中的无线电波变成可处理的数据**的流水线：

📡 **天线 → 射频滤波器 → LNA → 混频器 → 中频/基带滤波器 → ADC → 数字处理**

我在你原来的链路里补了一个“中频/基带滤波器”：实际接收机通常需要在混频后、ADC 前再做滤波。([analog.com](https://www.analog.com/en/resources/technical-articles/selecting-highlinearity-mixers-for-wireless-base-stations.html?utm_source=openai))

| 环节                                         | 它做什么                                          | 可以怎么理解                                                                                                                                                                                                 |
| ------------------------------------------ | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **天线**                                     | 接收空间中的电磁波，在接收机输入端形成微弱的电信号。                    | “收音的耳朵”。它收到的不只有目标信号，还有其他频段的信号和干扰。                                                                                                                                                                      |
| **射频滤波器（RF Filter）**                       | 让所需频段通过，抑制频段外的强信号。                            | “先筛一遍”。避免不想要的强信号进入后面的放大器、混频器，造成过载或失真。([wiki.analog.com](https://wiki.analog.com/resources/eval/developer-kits/2to24ghz-mxfe-rf-front-end/rx-overview?utm_source=openai))                               |
| **LNA（Low-Noise Amplifier，低噪声放大器）**        | 放大天线收到的微弱信号，同时尽量少引入额外噪声。                      | “轻声说话时用的高质量扩音器”。它不能把已有的噪声去掉，但能让信号在进入后级电路前变得足够强。([analog.com](https://www.analog.com/en/resources/technical-articles/selecting-highlinearity-mixers-for-wireless-base-stations.html?utm_source=openai)) |
| **混频器（Mixer）**                             | 把接收信号与**本振（LO）**信号混合，产生新的频率；选出较低的频率，就叫“降频”。   | “把高频信号搬到更方便处理的频段”，**不是**在这里把它变成数字。([wiki.analog.com](https://wiki.analog.com/university/tools/m2k/tutorials/buildingaradiowithm2k?utm_source=openai))                                                  |
| **中频/基带滤波器**                               | 选出混频后的目标信号，抑制不需要的混频产物、干扰和部分噪声；ADC 前还可能有抗混叠滤波。 | “搬完频率，再精筛一遍”。([analog.com](https://www.analog.com/cn/resources/app-notes/an-2550.html?utm_source=openai))                                                                                              |
| **ADC（Analog-to-Digital Converter，模数转换器）** | 按时钟对模拟信号采样，并把采样值表示成数字。                        | “把连续变化的电信号变成一串数字样本”。**有了数字样本，不等于已经读懂了无线数据。**([analog.com](https://www.analog.com/cn/resources/app-notes/an-2550.html?utm_source=openai))                                                               |
| **数字处理**                                   | 对样本做数字滤波、解调等处理，提取承载的信息。                       | “从数字样本里还原内容”；它是处理环节，不一定是一个单独的器件。([analog.com](https://www.analog.com/cn/resources/analog-dialogue/articles/receiver-design-simplified-by-if-digitizing-subsystem.html?utm_source=openai))              |

**举个降频例子：**如果收到的是 **2.4 GHz** 信号，本振是 **2.3 GHz**，混频后会出现 **0.1 GHz（100 MHz）** 和 **4.7 GHz** 等频率成分；滤波器选出需要的那部分，再交给后级处理。([wiki.analog.com](https://wiki.analog.com/university/tools/m2k/tutorials/buildingaradiowithm2k?utm_source=openai))

最后提醒一下：这只是**一种简化的接收架构**。有的设计把信号直接降到基带，有的会经过多次混频，还有的用高速 ADC 直接采样较高频率的信号；所以实物框图不一定严格按这七格排列。([wiki.analog.com](https://wiki.analog.com/resources/eval/user-guides/ad9361?utm_source=openai))