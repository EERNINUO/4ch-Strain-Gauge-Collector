<div align="center">

<!-- Assisted-by: DeepSeek - 文档框架 -->

# 4ch-Strain-Gauge-Collector

创新创业实践课程设计 E4 选题 – 4通道应变信号同步采集模块  
基于 STM32G473，内置 ADC + 高速 SRAM 缓冲 + USB 触发同步

<p align="center">
  <a href="https://github.com/EERNINUO/ArbWave30" style="margin: 2px;">
    <img alt="仓库状态" src="https://img.shields.io/badge/status-设计中-blue">
  </a>
  <a href="https://github.com/EERNINUO/ArbWave30" style="margin: 2px;">
    <img alt="GitHub 仓库星标" src="https://img.shields.io/github/stars/EERNINUO/ArbWave30">
  </a>
  <br/>
  <a href="https://raw.githubusercontent.com/EERNINUO/ArbWave30/main/LICENSE" style="margin: 2px;">
    <img src="https://img.shields.io/github/license/EERNINUO/ArbWave30" alt="许可证">
  </a>
  <a href="https://raw.githubusercontent.com/EERNINUO/ArbWave30/main/LICENSE" style="margin: 2px;">
    <img src="https://img.shields.io/badge/License-CC--BY--SA--4.0-green" alt="许可证">
  </a>
    <a href="https://raw.githubusercontent.com/EERNINUO/ArbWave30/main/LICENSE" style="margin: 2px;">
    <img src="https://img.shields.io/badge/License-CERN--OHL--S--2.0-blue" alt="许可证">
  </a>
</p>

</div>
---

## 📌 项目简介

本项目为《创新创业实践课程》课程 E4 选题的硬件与固件设计成果。  
设计基于 **STM32G473**（Arm Cortex-M4F）实现了 **4 通道应变信号同步采集**，支持外部事件触发、高速数据缓存（SRAM）及非易失参数存储（QSPI Flash），并通过 USB/串口实现与 PC 的命令交互与多模块同步。

设计周期：2 周（原理图 + PCB + 仿真 + 文档 + CubeMX工程，无实物焊接与固件代码）  
参考原型：NI‑9237 应变/桥输入模块

**核心指标**：
- 4 通道差分输入（±2.5V，共模 1.65V）
- 12 位内置 ADC，支持硬件过采样
- 4 阶有源低通滤波器（100kHz / 200kHz 可选）
- 最多 4 片 IS61WV204816BLL 高速 SRAM（2M×16bit）
- QSPI Flash 存储固件及标定参数
- USB 2.0 FS + 可线与串口（多模块同步触发）

## 🎯 主要特性

| 模块 | 特性 |
|------|------|
| **模拟前端** | 仪表放大器 (INA) + 1/4 衰减网络 + 4 阶 Sallen-Key 滤波器，净增益 1 |
| **ADC** | 内置 5 个独立 12 位 ADC，支持差分输入，事件触发同步采样 |
| **触发机制** | 片上比较器 (CMP) 整形外部触发信号，直接路由到定时器，响应延迟 < 1µs |
| **数据缓存** | FMC 总线扩展 SRAM（1~4 片），支持连续高速采集 |
| **存储** | QSPI Flash 用于程序 XIP 及标定参数掉电保存 |
| **通信** | USB CDC 虚拟串口 / 可线与硬件串口，支持多机同步 |

---

## 🖥️ 硬件架构概览

```
[应变传感器] → [仪表放大器(4x)] → [1/4衰减] → [4阶LPF] → [STM32G473 ADC]
                                                                   │
外部触发信号 ─→ [CMP整形] ─→ [TIM触发] ─→ [ADC注入组采样] ←─┐
                                                                   │
                                                          [SRAM缓冲]
                                                                   │
USB/串口 ←─────────────────────────────────────────────── [MCU核心]
```

详细原理框图及模块说明见 [硬件设计文档](docs/hardware_design.md)。

## 🛠️ 开发工具与环境

| 用途 | 工具 | 版本 |
|------|------|------|
| 原理图 & PCB |  KiCad | 10.0 |
| 模拟仿真 | LTspice  | 26.0.1 |
| MCU 配置 | STM32CubeMX | 6.13 |
| 代码生成 | Keil MDK | 5.36  |
| 版本管理 | Git + GitHub | - |

## 📁 仓库结构
4ch-Strain-Gauge-Collector/
├── docs/                     # 设计文档
│   ├── pic/                  # 文档中用到的图片
│   ├── hardware_design.md    # 硬件设计文档（原理图、PCB、BOM）
│   └──  LICENSE              # CC BY-SA 4.0 
├── simulation/               # LTspice 仿真文件
├── firmware/                 # CubeMX工程与生成代码（CubeMX + Keil）
│   └── STM32G473.ioc         # CubeMX 配置文件
├── hardware/                 # 硬件设计源文件
|   ├── E4                    # Kicad 原理图与 PCB 文件
│   └── LICENSE # CERN-OHL-S-2.0   
├── README.md                 # 项目说明
└── LICENSE                   # GPL-3.0

## 📜 许可证
Copyright © 2026 EERNINUO

本项目采用**多许可证策略**，不同部分适用不同许可证：

| 组成部分 | 许可证 |
|----------|--------|
| 固件 (MCU/FPGA 代码) | [GNU General Public License v3.0](LICENSE) |
| 硬件设计文件 (原理图、PCB、BOM) | [CERN Open Hardware Licence Version 2 - Strongly Reciprocal](hardware/LICENSE) |
| 文档、图片、README | [Creative Commons Attribution-ShareAlike 4.0 International](docs/LICENSE) |

这意味着：
- 你可以自由使用、修改、分享固件代码，但任何衍生代码也必须以 GPL-3.0 开源。
- 你可以基于硬件设计文件制作、分发物理设备，但必须同时开源你对设计文件的修改，并保留署名。
- 你可以复制和修改文档，但必须同样以 CC BY-SA 4.0 分享，并注明原作者。

## 📧 联系方式

如有问题或建议，请在 GitHub Issues 中提出。


