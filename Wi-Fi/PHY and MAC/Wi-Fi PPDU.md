# Wi-Fi PPDU：无线帧的基本结构

**PPDU（Physical Layer Protocol Data Unit，物理层协议数据单元）是 PHY 层在空口发送的一次物理层传输。**它包含帮助接收机同步和解码的 PHY 字段，以及本次要传输的内容。

先把层次记住：**PPDU 是空口的物理层传输；MPDU 是其中承载的 MAC 帧；PSDU 是 MAC 交给 PHY 传输的内容。**

## 一张图：从 PPDU 到 MAC 帧

下面以常见的**单用户、携带一个 MAC 帧**的情况为例；方框表示逻辑层次，宽度不代表真实时间或长度：

```text
空口传输时间  ─────────────────────────────────────────────────────→

┌──────────────────────────── 一次 PPDU ────────────────────────────┐
│                                                                  │
│  PHY 前导 / 信令字段                  PHY Data 字段               │
│  ┌───────────────────────────┐   ┌────────────────────────────┐  │
│  │ 训练字段 STF、LTF │ SIG 等 │   │ SERVICE │ PSDU │ Tail/Pad* │  │
│  └───────────────────────────┘   └───────────┬────────────────┘  │
└──────────────────────────────────────────────┼───────────────────┘
                                               │ 本例中 PSDU 是一个 MPDU
                                               ▼
                              ┌───────────────────────────────────┐
                              │ MAC Header │ Frame Body │   FCS   │
                              └───────────────────────────────────┘
                                  MAC 帧（MPDU）的简化结构
```

图中的 `SERVICE / PSDU / Tail / Pad` 是 OFDM PHY Data 的**逻辑组成示意**，不是说空口中这些比特未经处理就原样排列：实际会按 PHY 规定加扰、纠错编码、映射到子载波等。`*` Tail 的存在和处理取决于 PHY 与编码方式；Pad 长度也随配置变化，不能把这个 Data 结构套到所有 PHY。

**PSDU 不一定只有一个 MPDU**：在支持的聚合模式下，可以是由多个 MPDU 子帧组成的 A-MPDU。多个 MPDU 共享一次 PPDU 的 PHY 前导和信令，不需要每个 MPDU 都重新发送一套 PHY 前导。

| 部分 | 主要作用 |
|---|---|
| **STF / LTF** | 检测信号、同步、调整接收增益和估计信道；详见 [[Wi-Fi PHY Training and Signaling]] |
| **SIG** | 告诉接收机本次传输的参数，例如速率、长度、MCS 或 STBC；具体内容取决于 PHY 格式 |
| **PSDU** | PHY 要传送的 MAC 内容；可为单个 MPDU，也可为支持的聚合形式 |
| **MAC Header / Frame Body / FCS** | 帧类型、地址等控制信息 / 帧内容 / 用于差错检测的帧校验序列 |

MAC 帧可以是 **Beacon 等管理帧、ACK 等控制帧或数据帧**；图中的 Header/Body 只是通用概念，不同帧类型的具体字段、长度和是否有帧体都不同。Beacon 的 SSID 和能力信息属于 MAC 帧体，不属于 PHY 的 SIG。

## 具体例子：Wi-Fi 4 的 HT Mixed PPDU

通用图中的训练和信令字段不一定各自集中排列。下面是 **802.11n HT Mixed** 的简化顺序；`L-` 表示 legacy（传统兼容部分）：

```text
时间 ───────────────────────────────────────────────────────────────→

┌───────┬───────┬───────┬────────┬────────┬────────────┬──────────┐
│ L-STF │ L-LTF │ L-SIG │ HT-SIG │ HT-STF │ HT-LTF(s)  │   Data   │
└───────┴───────┴───────┴────────┴────────┴────────────┴──────────┘
  检测/     初始    旧格式    HT 参数   HT 接收    HT 信道       承载
  粗同步   信道估计  信令     MCS/STBC  增益调整    估计          PSDU
```

这是帮助建立概念的简化图：实际配置还可能有可选的 HT extension LTF，训练字段数量也会变化；不要将它当成所有 Wi-Fi 代际通用的精确帧格式。特别注意，**HT-STF / HT-LTF 出现在 HT-SIG 后面**，并不是所有训练字段都排在所有 SIG 前面。

## 读图时注意

1. **前导码与 SIG 的名称有不同用法**：有的资料把 Data 前的整个区域宽泛称为 PHY preamble；要区分已知训练序列和携带配置比特的 SIG。参见 [[Wi-Fi PHY Training and Signaling#3. SIG 算不算前导码？|训练字段与 SIG]]。
2. **Wi-Fi 版本和格式不同，字段不同**：HT、VHT、HE、EHT 以及 SU/MU 格式不能共用一张精确字段图；多用户 PPDU 可同时承载面向不同用户的内容。
3. **Radiotap 不是空口 PHY 头**：普通 Wireshark 抓包通常显示网卡/驱动提供的 Radiotap 元数据和解出的 MAC 帧，而非完整原始 PHY 波形或全部 SIG 位。未显示的字段不等于不存在。

## 来源与相关笔记

- [OpenOFDM：SIGNAL and HT-SIG](https://openofdm.readthedocs.io/en/latest/sig.html)：HT Mixed PPDU 与 HT-SIG。
- [ShareTechnote：Wi-Fi PHY Frame](https://www.sharetechnote.com/html/WLAN_PHY_Frames.html)：不同 PHY 帧结构示意。
- [ShareTechnote：Wi-Fi Frame Structure](https://www.sharetechnote.com/html/WLAN_FrameStructure.html)：MAC 帧、Beacon 与 PHY/MAC 层次示意；具体字段应以对应标准和帧类型为准。
- [[OFDM]]：子载波、循环前缀和导频。
- [[Wi-Fi STBC]]：STBC 与 SIG、空间流的关系。
