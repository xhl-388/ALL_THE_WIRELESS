**Frequency Demodulator（频率解调器）**是把信号中**随时间变化的频率**还原成信息的模块，常用于 **FM** 或 **FSK** 接收。它不是一个特定型号的器件，也**不等于 ADC**。([analog.com](https://www.analog.com/media/en/technical-documentation/data-sheets/adf7025.pdf?utm_source=openai))

拿 **FSK** 举例：发送端用两个不同频率代表 0 和 1。接收端的频率解调器识别信号此刻偏向哪个频率，产生对应的解调结果；后面的滤波和 **Data Slicer** 再把结果判成明确的数字 0/1。([analog.com](https://www.analog.com/media/en/technical-documentation/data-sheets/adf7025.pdf?utm_source=openai))

```text
天线 → 滤波/LNA → 混频器 → 中频或基带信号
                              ↓
                        频率解调器 → 滤波 → Data Slicer → 0/1 数据
```

和前面几个模块的区别是：

- **混频器**：借助 LO 把整个信号“搬到”较低频段，方便处理；不负责判断 0/1。
- **频率解调器**：读出信号的*频率变化*所承载的信息。
- **Data Slicer**：根据解调后的结果，作出最终的高/低电平判决。([analog.com](https://www.analog.com/media/en/technical-documentation/data-sheets/adf7025.pdf?utm_source=openai))

有些芯片把频率解调器称为 **FM demodulator** 或 **frequency discriminator（鉴频器）**；实现可以是模拟电路，也可以在数字域完成，因此 **ADC 是否位于它前面，要看具体架构**。([analog.com](https://www.analog.com/en/products/max1471.html?utm_source=openai))