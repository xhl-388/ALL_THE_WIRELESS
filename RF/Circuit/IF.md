**IF 是 Intermediate Frequency，中文叫“中频”。**它指接收信号经过混频、从原来的射频（RF）搬到一个便于后续处理的频段后，得到的**信号及其频率**；通常不是指某个器件。([analog.com](https://www.analog.com/en/resources/glossary/if.html?utm_source=openai))

接着我们前面的例子看：

```text
接收信号 RF：2.4 GHz ──┐
                      混频器 → 选出差频 → IF：100 MHz → 滤波/解调
本振信号 LO：2.3 GHz ──┘
```

这里 **IF = |RF − LO| = 100 MHz**。混频器也会产生和频等其他成分，所以后面要用滤波器选出需要的信号。([analog.com](https://www.analog.com/media/en/training-seminars/design-handbooks/Basic-Linear-Design/Chapter4.pdf?utm_source=openai))

一句话区分：**RF 是天线收到的射频信号，LO 是供混频器使用的本振信号，IF 是混频后选出来、供后级处理的中频信号。**在我们讨论的链路里，频率解调器可以处理这个 IF 信号；具体是否还要再降频，取决于接收机架构。([analog.com](https://www.analog.com/en/resources/glossary/superheterodyne_receiver.html?utm_source=openai))