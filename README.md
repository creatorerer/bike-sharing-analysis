# 🚲 共享单车用户行为分析 (Bike Sharing User Behavior Analysis)

> 团队项目 — 基于上海摩拜单车 2016 年 8 月数据的用户行为分析与用户价值分层

---

## 📋 项目概述

本项目对上海共享单车（Mobike）2016 年 8 月的骑行数据进行全链路分析，包含：

| 阶段 | 内容 |
|------|------|
| **数据清洗** | 缺失值/异常值/重复值处理、数据类型转换 |
| **特征工程** | 骑行距离/速度/时长计算、时间特征提取、天气数据清洗与合并 |
| **探索性分析 (EDA)** | 时间维度分析、用户行为分析、天气影响分析、空间分布分析 |
| **用户价值分层 (RFM)** | K-Means 聚类实现用户价值分级（S/A/B/C 四级） |

---

## 📁 项目结构

```
bicycle sharing pj/
├── 📂 data/                     # 数据文件
│   ├── 上海共享单车数据.csv        # 原始骑行数据 (10万+条)
│   ├── 上海天气.xlsx             # 原始天气数据
│   ├── 共享单车_清洗后.csv        # 清洗后的骑行数据
│   ├── 共享单车_用户特征.csv       # 用户级别聚合特征
│   ├── 共享单车_单车特征.csv       # 单车级别聚合特征
│   └── 共享单车_天气清洗后.csv     # 清洗后的天气数据
│
├── 📂 code/                     # 分析 Notebook
│   ├── 共享单车数据处理.ipynb              # (旧版) 基础数据加载
│   ├── 共享单车数据处理与特征工程.ipynb     # Step1-3: 清洗+特征工程
│   ├── 共享单车EDA与可视化.ipynb           # Step4: 探索性分析
│   └── 共享单车用户价值RFM分层.ipynb       # K-Means 用户分层
│
├── .gitignore
└── README.md                    # ⟵ 你现在看到的就是这个
```

---

## 🧰 数据说明

### 骑行数据 (`上海共享单车数据.csv`)

| 字段 | 说明 |
|------|------|
| `orderid` | 订单 ID |
| `bikeid` | 单车 ID |
| `userid` | 用户 ID（匿名化） |
| `start_time` | 骑行开始时间 |
| `end_time` | 骑行结束时间 |
| `start_location_x/y` | 起点坐标（经度/纬度） |
| `end_location_x/y` | 终点坐标（经度/纬度） |
| `track` | GPS 轨迹点 |

**规模**: 102,361 条记录 · 16,887 名用户 · 79,062 辆单车 · 2016 年 8 月

### 天气数据 (`上海天气.xlsx`)

| 字段 | 说明 |
|------|------|
| 日期 | 日期 + 星期 |
| 最高温/最低温 | 含 ℃ 符号 |
| 天气 | 原始天气描述 |
| 风力风向 | 风力等级 |
| 日照时长 | 含"小时"单位 |

---

## 🔧 环境要求

```bash
# Python 3.8+
pip install numpy pandas matplotlib seaborn scikit-learn openpyxl
```

Notebook 在 Jupyter / JupyterLab / VS Code 中运行即可。

---

## 🚀 快速开始

```bash
# 1. 克隆仓库
git clone https://github.com/<你的GitHub用户名>/bike-sharing-analysis.git
cd bike-sharing-analysis

# 2. 安装依赖
pip install -r requirements.txt

# 3. 按顺序运行 Notebook
#    数据处理 → EDA → RFM 分层
```

---

## 👥 团队协作

### 分支策略

```
main        ← 稳定版本，保护分支
├── dev     ← 开发主线
├── feat/xxx   ← 功能分支
└── fix/xxx    ← 修复分支
```

### 提交规范

```
<type>: <简短描述>

feat: 添加EDA分析
fix:  修复距离异常过滤逻辑
docs: 更新README
refactor: 重构特征工程代码
```

### 协作流程

1. 从 `dev` 切出功能分支: `git checkout -b feat/eda-analysis dev`
2. 完成开发后提交 PR 到 `dev`
3. 代码 Review 通过后合并
4. 定期从 `dev` merge 到 `main` 发布稳定版

---

## 📊 分析框架（参考）

本项目参考了同仓库另一套分析框架的写法风格，将分析过程按标准数据科学流程组织：

```
Data Loading → Data Cleaning → Feature Engineering → EDA → User Segmentation → Insights
```

---

## ⚠️ 注意事项

- 原始数据文件较大（~45MB），**已在 .gitignore 中排除**，团队成员需自行获取数据
- 天气数据 `℃` 和 `小时` 等单位符号已在清洗流程中去除
- 如需要在本地完整运行，确保 `data/` 目录下有原始数据文件

---

## 📝 License

MIT
