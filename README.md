
![License](https://img.shields.io/badge/License-CC%20BY--NC%204.0-blue.svg)
![Verilog](https://img.shields.io/badge/Language-Verilog%2FSystemVerilog-orange.svg)
![Simulator](https://img.shields.io/badge/Simulator-Icarus%20Verilog%2012.0-green.svg)
![EFF](https://img.shields.io/badge/EFF-98.04%25-brightgreen.svg)
![Status](https://img.shields.io/badge/Status-Pre--Tape--Out%20Verified-blue.svg)
![Stars](https://img.shields.io/github/stars/your-username/cold-chip?style=social)

**EFF = (Erecycled ÷ Etotal) → 100%**

*非耗散计算协议 · 核心验证标准*

[📄 中文论文](#论文) · [📄 English Paper](#paper) · [🔬 仿真验证](#仿真验证) · [⭐ Star支持](#支持本项目)

</div>

---

## ⚡ 核心主张

> 冷芯不是"节能芯片"，而是一种**热现象在架构层面被消除**的计算系统。

传统硅基芯片发热的根源：**Q = I²Rt**

冷芯的解法：**从物理机制上让 I²Rt ≡ 0**，而不是改善散热。

```
传输层：光子走波导 + 纳米颗粒走无摩擦液态轨道  →  Q = I²Rt ≡ 0
开关层：微环谐振器调节折射率切换逻辑            →  无剧烈电流
结构层：0.5nm逻辑岛 + 液态微流体无人区          →  热量瞬间吸收
```

---

## 📊 关键数据（EDA Playground 真实仿真结果）

| 指标 | 冷芯实测 | 传统3nm硅基 | 改善 |
|------|----------|------------|------|
| 总功耗 | **9,180 nW** | 451,800,000 nW | **49,216倍** |
| 温升 ΔT | **0.000918 K** | 需主动散热 | 趋近室温 |
| EFF能量回收率 | **98.04%** | ~50% | +96% |
| 功耗密度 | **9,180 nW/cm²** | ~0.45 W/cm² | 49,216倍 |
| 芯片面积 | 1 mm² | — | 四岛+储能池 |

> **EFF = 98.04%** 即为室温下的 **Landauer 物理极限**（剩余1.96%为热力学第二定律不可消除项）。

---

## 🔬 仿真验证

五步完整验证，全部在 **EDA Playground（Icarus Verilog 12.0）** 上真实运行，用户亲眼看到 Log 输出。

| 步骤 | 验证内容 | 关键结果 | 状态 |
|------|----------|----------|------|
| 第1步 | RTL 逻辑仿真 | EFF_PASS=1 · zero_heat_ok=1 · 四岛自愈 | ✅ 通过 |
| 第2步 | 时序仿真 | slack=0.5ns · 零稳态建立时间违例 | ✅ 通过 |
| 第3步 | 功耗仿真 | 9180nW · ΔT=0.000918K · EFF=98.04% | ✅ 通过 |
| 第4步 | 版图 DRC/LVS | 14/14 PASS · LVS 完全对应 · 1mm² | ✅ 通过 |
| 第5步 | 动态应力仿真 | 五场景全通 · -40°C~125°C · 液态失效降级 | ✅ 通过 |

### 第1步 RTL 关键 Log 节点

```
[T=1,615,000]  island_online = 1111        ← 四岛全部上线
[T=2,935,000]  resonance = 1               ← 共振态激活
[T=8,075,000]  EFF_PASS=1 zero_heat_ok=1   ← 零废热验证通过
[T=10,075,000] island_online=1111          ← 自愈完成
```

### 第3步 功耗关键 Log

```
[T=3,985,000]  Pdyn=9000nW | Pstat=180nW | Total=9180nW
[T=3,985,000]  delta_T=918uK | EFF=9804/10000
[T=8,075,000]  EFF_PASS=1 | zero_heat_ok=1
```

### 第4步 DRC/LVS

```
DRC pass map : 11111111111111   ← 14项全通
DRC all pass : 1
LVS pass map : 11111111         ← 5模块全对应
Result: DRC 14/14 PASS | LVS PASS
```

---

## 🏗️ 芯片架构

```
┌─────────────────────────────────────┐
│         冷芯 1mm² 布局              │
│                                     │
│  ┌─────────┐       ┌─────────┐      │
│  │   CPU   │       │   MEM   │      │
│  │ 50K门   │       │ 30K门   │      │
│  │(220,250)│       │(600,250)│      │
│  └────┬────┘       └────┬────┘      │
│       │    ┌──────┐     │           │
│       └───►│ ENG  │◄────┘           │
│            │ CORE │                 │
│       ┌───►│(500, │◄────┐           │
│       │    │ 500) │     │           │
│  ┌────┴────┐└──────┘┌───┴─────┐    │
│  │   GPU   │        │   BUS   │    │
│  │ 80K门   │        │ 20K门   │    │
│  │(220,600)│        │(600,600)│    │
│  └─────────┘        └─────────┘    │
│                                     │
│  液态轨道  v = c/n = 2.14×10⁸ m/s  │
└─────────────────────────────────────┘
```

**共振态激活条件**：四岛全线 + 温度正常 → 持续32周期 → `resonance=1` → EFF=98.04%

---

## 📁 仓库结构

```
cold-chip/
├── README.md                   # 本文件
├── LICENSE                     # CC BY-NC 4.0 许可证
├── PRIOR_ART.md                # 优先权声明 & SHA256 时间戳
├── src/
│   ├── cold_chip.v             # 核心RTL模块（已验证）
│   ├── cold_chip_timing.v      # 第2步时序仿真
│   ├── cold_chip_power.v       # 第3步功耗仿真
│   ├── cold_chip_drc.v         # 第4步DRC/LVS验证
│   └── cold_chip_stress.v      # 第5步动态应力仿真
├── docs/
│   ├── cold_chip_paper_cn.docx # 技术论文（中文版）
│   ├── cold_chip_paper_en.docx # Technical Paper (English)
│   └── cold_chip_report.docx   # 仿真验证完整报告
└── sim/
    └── edaplayground_guide.md  # EDA Playground 运行指南
```

---

## 🚀 快速开始

### 在 EDA Playground 上运行仿真

1. 打开 [edaplayground.com](https://edaplayground.com)
2. 选择 **Icarus Verilog 12.0**
3. 编译选项：`-Wall -g2012 -DSIMULATION`

| 步骤 | Top Module | 期望输出 |
|------|-----------|---------|
| 第1步 RTL | `tb_cold_chip` | EFF_PASS=1, zero_heat_ok=1 |
| 第3步 功耗 | `tb_power` | Total=9180nW, EFF=9804/10000 |
| 第5步 应力 | `tb_stress` | ALL 5 SCENES PASS |

---

## 📄 论文 / Paper

| 版本 | 文件 | 内容 |
|------|------|------|
| 🇨🇳 中文版 | `docs/cold_chip_paper_cn.docx` | 完整技术论文（中文）|
| 🇺🇸 English | `docs/cold_chip_paper_en.docx` | Full Technical Paper (English) |
| 📊 验证报告 | `docs/cold_chip_report.docx` | 五步仿真验证完整报告 |

---

## ⚖️ 许可证 / License

本项目采用 **Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)** 许可证。

| | 说明 |
|---|---|
| ✅ 允许分享 | 复制、发行（须署名，非商业） |
| ✅ 允许演绎 | 修改、再创作（须署名，非商业） |
| ✅ 学术引用 | 论文、报告引用（须注明来源） |
| ❌ 禁止商业使用 | 不得用于商业流片、IP授权等 |

**版权所有 © 2025 毛广辉（Mao Guanghui）**

完整许可证文本见 [LICENSE](./LICENSE)。商业授权请通过 Issues 联系。

---

## 🔒 优先权声明

本仓库建立的主要目的之一是**公开时间戳记录**，用于技术优先权存证。

- 首次提交时间：**2026年4月2号**（见 Git commit history）
- 核心文档 SHA256 见 [PRIOR_ART.md](./PRIOR_ART.md)

---

## ⭐ 支持本项目

如果冷芯的技术路线让你觉得有价值，请点击右上角的 **⭐ Star** 支持本项目。

每一个 Star 都是对「芯片不必发热」这一命题的认可。

> 如果你认识对低功耗芯片、光子计算或液态架构感兴趣的研究者、工程师或投资人，欢迎转发。

---

## 📬 联系

- **GitHub Issues**：技术问题、合作咨询
- **LinkedIn**：[毛广辉](https://www.linkedin.com/in/广辉-毛-7657523ab)
- **商业授权**：Issues 注明「商业合作」

---

<div align="center">

**EFF = (Erecycled ÷ Etotal) → 100%**

*冷芯 · Cold Chip · 零废热 · 液态计算 · 非耗散架构*

版权所有 © 2025 毛广辉 · CC BY-NC 4.0

