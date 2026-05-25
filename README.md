# MathModel — 数学建模竞赛 Skill 工具集

面向 **CUMCM、MCM/ICM、HiMCM、GMCM** 等数学建模竞赛的 Claude Code Skill 集合，
按用途分为四个模块，覆盖从问题分析、数值求解到论文生成和格式检查的完整流水线。

---

## 工具概览

```
竞赛题目
    │
    ▼
┌──────────────────────────────────────────────┐
│  1. MathModel — 核心建模引擎                   │
│     问题分析 → 建模方案 → 求解 → 论文生成 → 检查   │
└──────────────────────────────────────────────┘
    │                │              │
    ▼                ▼              ▼
┌──────────┐  ┌──────────┐  ┌──────────────┐
│2. matlab │  │3. doc    │  │4. minimax    │
│  数值计算 │  │  论文排版 │  │  -xlsx       │
│  仿真绘图 │  │  DOCX生成 │  │  数据处理    │
└──────────┘  └──────────┘  │  表格格式化   │
                            └──────────────┘
```

---

## 1. MathModel — 核心建模引擎

**用途**：数学建模全流程自动化的主控 Skill。

启动后自动执行两阶段：
- **Phase 1 — 建模与论文生成**：问题分析 → 建模方案选择 → Python 代码编写与执行 → 论文撰写（python-docx 直接生成 DOCX + Markdown）
- **Phase 2 — 论文格式与内容检查**：5 大类质量检查（公式、代码一致性、图表、排版、f-string 转义），逐项排查并自动修复

**触发方式**：
- `/MathModel` 或描述数学建模问题
- "求解这道 CUMCM 题目：..."

**常见问题类型与推荐模型**：

| 问题类型 | 推荐方法 |
|----------|----------|
| 预测/预报 | ARIMA、LSTM、灰色模型 GM(1,1)、指数平滑 |
| 分类 | 逻辑回归、SVM、随机森林、XGBoost |
| 优化 | 线性/非线性规划、遗传算法、粒子群、模拟退火 |
| 微分方程 | ODE/PDE 数值解（Runge-Kutta、有限差分） |
| 网络/图论 | 最短路径、最大流、PageRank、社区发现 |
| 评价/AHP | TOPSIS、熵权法、模糊综合评价 |
| 空间分析 | Kriging 插值、Voronoi、聚类 |
| 时间序列 | Fourier 变换、小波分析、Kalman 滤波 |

**Phase 2 检查清单亮点**：
- 2.1.1 单位换算验证 — 检测代码数值与论文公式是否一致（如 m/s vs m/min 的 60 倍差异）
- 2.1.2 符号下标一致性 — 检测 Unicode 下标混用（如 Tₐ vs T_A）
- 2.2 代码-论文一致性 — 重新运行代码提取数值，与论文表格逐项比对

---

## 2. matlab — 数值计算与科学可视化

**用途**：MATLAB / GNU Octave 计算引擎，处理矩阵运算、微分方程求解、信号处理、数据可视化。

**在竞赛中的典型场景**：
- 微分方程数值解（ODE/PDE）— 矿井水流漫延、传染病传播、热传导
- 大规模矩阵运算 — 网络流分析、有限元计算
- 信号处理 — 地震波分析、振动信号 FFT
- 优化求解 — 线性规划、非线性约束优化
- 高质量图表 — 300/600 DPI 输出，可直接用于论文插图

**支持 MATLAB 和 GNU Octave**，脚本通用。

**安装**：
```bash
# MATLAB (商业)
# 下载: https://www.mathworks.com/downloads/

# GNU Octave (免费开源)
brew install octave        # macOS
sudo apt install octave    # Ubuntu/Debian
# Windows: https://octave.org/download
```

---

## 3. doc — 论文排版与 DOCX 生成

**用途**：.docx 文档的读取、创建、编辑和可视化检查。

**在竞赛中的典型场景**：
- 使用 python-docx 生成竞赛格式论文（三线表、页眉页码、宋体/黑体）
- DOCX → PDF → PNG 渲染检查，确保排版无误
- 表格、图表、标题层级一致性验证

**论文格式标准**（CUMCM 规范）：
- 三线表：顶线/底线 1.5pt，表头线 0.75pt，无竖线
- 标题：一级 15pt 黑体居中，二级 14pt，三级 12pt
- 正文：宋体 12pt，1.5 倍行距
- 页码：封面无页码，正文从 1 开始

---

## 4. minimax-xlsx — 数据处理与表格格式化

**用途**：Excel 文件（.xlsx/.xlsm/.csv）的创建、读取、编辑、公式验证，零格式损失编辑。

**在竞赛中的典型场景**：
- 读取附件数据（端点坐标、巷道连接关系、参数表）
- 导出计算结果表（水流到达时间、逃生路径结果）
- 生成带公式的汇总表
- 验证计算结果的一致性

**核心优势**：XML 直接编辑技术，不损坏 VBA、数据透视表、条件格式。

---

## 安装方式

将这些 skill 目录复制到 Claude Code 的 skills 路径：

```bash
# Claude Code skills 目录
~/.claude/skills/

# 复制所有 skill
cp -r skills/* ~/.claude/skills/
```

或者逐个安装：
```bash
cp -r skills/MathModel ~/.claude/skills/
cp -r skills/matlab ~/.claude/skills/
cp -r skills/doc ~/.claude/skills/
cp -r skills/minimax-xlsx ~/.claude/skills/
```

重启 Claude Code 后，使用 `/MathModel` 即可启动。

---

## 典型工作流

以 CUMCM 2025 D 题"矿井突水水流漫延模型"为例：

```
1. MathModel 启动
   ├── Phase 1 Step 1: 问题分析 → 识别 4 个子问题
   ├── Phase 1 Step 2: 建模方案 → 图论最短路径 + 时变网络
   ├── Phase 1 Step 3: MATLAB 求解 + Python 插图
   │       ├── matlab skill: 水流模拟 + 逃生路径求解
   │       └── minimax-xlsx skill: 读取附件数据，导出结果
   ├── Phase 1 Step 4: 论文撰写
   │       └── doc skill: python-docx 生成三线表、页眉页码
   └── Phase 2: 质量检查
           ├── [PASS] 单位换算验证
           ├── [FAIL] 符号下标不一致 → 自动修复
           └── [PASS] 数值一致性
```

---

## 目录结构

```
MathModel/
├── README.md              # 本文件
└── skills/
    ├── MathModel/         # 核心建模引擎
    │   └── SKILL.md
    ├── matlab/            # MATLAB/Octave 数值计算
    │   ├── SKILL.md
    │   └── references/    # 详细参考手册 (8 个)
    ├── doc/               # DOCX 文档处理
    │   ├── SKILL.md
    │   └── scripts/
    │       └── render_docx.py
    └── minimax-xlsx/      # Excel 数据处理
        ├── SKILL.md
        ├── scripts/       # 脚本工具集 (10 个)
        ├── templates/     # XML 模板
        └── references/    # 参考手册 (8 个)
```

---

## 许可证

MIT License — 详见各 skill 目录。
