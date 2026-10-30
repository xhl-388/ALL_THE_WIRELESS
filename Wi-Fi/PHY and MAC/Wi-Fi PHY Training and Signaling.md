# Wi-Fi PHY：训练序列、前导字段与 SIG

**训练序列是收发双方事先知道的信号。**接收机利用它检测无线信号、确定采样时序、估计频偏和信道；而 **SIG 是承载本次传输配置的信令字段**，告诉接收机后续数据应如何解码。

PPDU、PSDU 与 MAC 帧的整体关系，先看 [[Wi-Fi PPDU]]。

## 1. 训练序列是什么？

先用一个简化的实数序列说明“已知模板匹配”；它**不是 Wi-Fi 实际使用的序列**：

```text
约定的模板：              +1  -1  +1  +1  -1
接收信号：   噪声  噪声  +1  -1  +1  +1  -1  后续信号……
                         ↑
                  匹配程度增加的位置
```

接收机可以用**相关（correlation）**比较接收信号与模板，或比较重复信号的不同部分，找到可能的到达时间。实际接收信号是复数 I/Q 样本，受到噪声、频偏和多径影响；相关峰不一定准确对应第一条物理路径，具体的定时选择由接收机实现决定。

## 2. Wi-Fi 中的 STF、LTF 和 SIG

以 **802.11n 的 HT Mixed PPDU** 为例，时间从左到右：

```text
L-STF | L-LTF | L-SIG | HT-SIG | HT-STF | HT-LTF(s) | Data
  │       │       │        │        │          │        └─ PSDU：MAC 帧等
  │       │       │        │        │          └─ HT 多天线信道估计
  │       │       │        │        └─ 调整 HT 部分的接收增益
  │       │       │        └─ HT 传输参数，如 MCS、STBC
  │       │       └─ 旧设备可识别的信令
  │       └─ 精细定时 / 频偏处理、初始信道估计
  └─ 信号检测、AGC、粗同步与粗频偏估计
```

这只是 HT Mixed 的示意；不同 PHY 和 PPDU 格式的字段顺序、数量及用途可能不同。**训练字段不一定全部在 SIG 前面**：例如上图的 HT-STF 和 HT-LTF 位于 HT-SIG 之后。

### STF：先发现并初步对齐

**STF（Short Training Field，短训练字段）**含有可供检测的重复结构。接收机通常用它检测信号到来、调整 [[Receiver Metrics#自动增益控制（AGC）|AGC]]、进行粗定时与粗频偏估计。具体算法和先后顺序随接收机实现不同。

### LTF：更准确地估计信道

**LTF（Long Training Field，长训练字段）**使用接收机已知的训练信号，可帮助精细定时、频偏估计和初始信道估计。

以一个子载波的简化模型为例，已知发送值为 $X_k$，接收值为 $Y_k$，忽略噪声和残余同步误差时：

$$
Y_k=H_kX_k,\qquad \hat H_k=\frac{Y_k}{X_k}\quad(X_k\ne0)
$$

$H_k$ 是该子载波的复数信道响应，包含幅度和相位变化；$\hat H_k$ 是它的估计。例如 $X_k=1+j0$、$Y_k=0.5+j0.5$ 时，简化估计为 $\hat H_k=0.5+j0.5$。后续数据可以用信道估计进行**均衡**，尽量补偿信道造成的失真。真实接收机还要处理噪声、频偏和信道随时间变化；多天线情况下要估计信道矩阵，而不是直接套用单输入单输出公式。

### SIG：告诉接收机怎么解码

**SIG（Signal field，信令字段）**的格式已由标准规定，但内容会随本次 PPDU 而变化，例如长度、带宽、MCS 或 STBC 信息。接收机在完成必要同步和信道估计后解码 SIG，才能知道怎样处理后续字段。

| 字段 | 接收机是否预先知道具体内容？ | 主要作用 |
|---|---|---|
| STF / LTF | 知道规定的训练信号 | 检测、同步和信道估计 |
| SIG | 知道格式，但本包字段值需要解码 | 告知本次传输的参数 |
| Data | 不预先知道用户数据 | 承载 PSDU 和其中的 MAC 帧 |

## 3. SIG 算不算前导码？

**要先看资料采用的术语范围。**狭义讨论训练序列时，STF/LTF 是已知训练信号，SIG 是承载配置比特的信令字段；有些资料则把 Data 前用于接收准备的整个区域宽泛地称为 *PHY preamble*，其中也包括 SIG。理解时先看具体 PPDU 图，不要把“前导码”和“已知训练序列”当成完全相同的词。

## 4. 训练字段与导频有什么区别？

训练字段通常用于传输开始阶段的检测、同步和初始信道估计；**导频（pilot）**是数据传输期间插在指定子载波上的已知参考，帮助跟踪残余频偏、相位变化等。导频和数据子载波在同一个 OFDM 符号中并行发送，不是每个 QAM 符号前面单独加一段训练序列。参见 [[OFDM#导频（Pilot）|OFDM 的导频部分]]。

## 5. 与 STBC 和抓包的关系

[[Wi-Fi STBC|STBC]] 的实际使用信息位于特定 PHY 格式的 SIG 中：例如 HT Mixed 的 **HT-SIG 第二部分 B4–B5**，以及 VHT 的 **VHT-SIG-A1 B3**。接收机还需要适当的训练字段估计空时流相关的信道，才能正确处理后续符号。

Beacon 里的 STBC 能力字段表示设备**宣告支持什么**；某个 PPDU 的 SIG 表示**这次怎样发送**。普通抓包未必提供完整原始 PHY 头，Wireshark 的 Radiotap 元数据是否包含 STBC 等信息取决于网卡和驱动；字段缺失不能直接等同于未使用。

## 来源与延伸

- [OpenOFDM：Symbol Alignment](https://openofdm.readthedocs.io/en/latest/sync_long.html)：利用已知长训练序列做相关和符号定时。
- [OpenOFDM：SIGNAL and HT-SIG](https://openofdm.readthedocs.io/en/latest/sig.html)：HT Mixed PPDU 与 HT-SIG 字段。
- [ShareTechnote：Wi-Fi PHY Frame](https://www.sharetechnote.com/html/WLAN_PHY_Frames.html)：HT/VHT 字段结构示意。
- [[Linearity and RF Impairments]]：载波频率偏移与相位噪声。
