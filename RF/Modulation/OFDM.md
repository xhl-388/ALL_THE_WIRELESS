**OFDM（Orthogonal Frequency-Division Multiplexing，正交频分复用）**是一种**多子载波传输方式**：把数据分给许多频率不同、同时发送的子载波。每条子载波再用 BPSK、QPSK 或 QAM 等方式承载数据。([mathworks.com](https://www.mathworks.com/discovery/ofdm.html?utm_source=openai))

结合刚才的 CCK，可以这样看：

- **CCK**：生成一组规定的码片，按顺序发送；码片的相位模式携带数据。
- **OFDM**：在同一段时间里，让**许多子载波并行发送**；每条子载波上的相位或幅度状态携带数据。([mathworks.com](https://www.mathworks.com/discovery/ofdm.html?utm_source=openai))

举个简化例子：假设有 4 条子载波，频率分别是 `f₁、f₂、f₃、f₄`。同一个 OFDM 符号期间，发送机可以同时在这 4 条子载波上各发送一个 BPSK 符号：

```text
子载波 f₁：+1
子载波 f₂：−1
子载波 f₃：+1
子载波 f₄：+1
```

天线**不是轮流发四段波**，而是发出这四条子载波叠加后的一个波形。接收机再通过 FFT，把叠加的波形分解，读出各条子载波的数据。([mathworks.com](https://www.mathworks.com/help/comm/gs/basic-ofdm-with-no-cyclic-prefix.html?utm_source=openai))

名字里的**“正交”**是关键：子载波的频谱可以彼此重叠，但频率间隔经过设计，使接收机在规定的符号时间内能够把它们区分开。这与我们前面讨论傅里叶变换时“用不同频率的参考波去比较”直接相关。([mathworks.com](https://www.mathworks.com/discovery/ofdm.html?utm_source=openai))

**一句话记忆：BPSK 说明“一条载波上的数据怎样用相位表示”；OFDM 说明“怎样让许多子载波同时承载数据”。**它们可以一起使用，并不是二选一。([mathworks.com](https://www.mathworks.com/discovery/ofdm.html?utm_source=openai))