# 🚲 共享单车用户行为分析 (Bike Sharing User Behavior Analysis)

> **团队项目** — 基于上海摩拜单车 2016 年 8 月数据的全链路分析
>
> 项目已按业务拆分为 **5 个 Notebook**，每个 Notebook 有明确的业务目标和输出，团队成员可认领分工。

---

## 📋 数据说明

### 原始骑行数据 (`上海共享单车数据.csv`)

| 字段(单位) | 类型 | 说明 |
|-----------|:----:|------|
| `orderid` | int64 | 订单 ID（唯一标识） |
| `bikeid` | int64 | 单车 ID |
| `userid` | int64 | 用户 ID（匿名化，范围 1~17753） |
| `start_time` | datetime | 骑行开始时间 `YYYY-MM-DD HH:mm` |
| `end_time` | datetime | 骑行结束时间 `YYYY-MM-DD HH:mm` |
| `start_location_x(°)` | float64 | 起点经度（WGS84，≈121.4°E） |
| `start_location_y(°)` | float64 | 起点纬度（WGS84，≈31.2°N） |
| `end_location_x(°)` | float64 | 终点经度 |
| `end_location_y(°)` | float64 | 终点纬度 |
| `track` | string | GPS 轨迹 `经度,纬度#经度,纬度#...` |

**数据规模**: 102,361 条 · 16,887 名用户 · 79,062 辆单车 · 2016 年 8 月（上海）

### 原始天气数据 (`上海天气.xlsx`)

| 字段(单位) | 类型 | 说明 |
|-----------|:----:|------|
| 日期 | string | `YYYY-MM-DD 周X`（清洗时提取纯日期） |
| 最高温(℃) | string | 当日最高气温（含℃符号，需清洗） |
| 最低温(℃) | string | 当日最低气温（含℃符号，需清洗） |
| 天气 | string | 原始描述（晴/雨/多云/阴，**不简化**） |
| 风力风向 | string | 如"东南风3级" |
| 日照时长(h) | string | 当日日照时长（含"小时"，需清洗） |

**数据规模**: 31 条 · 2016年8月每日一条

### 清洗后数据 (`共享单车_清洗后.csv` — Notebook A 生成)

| 字段(单位) | 类型 | 说明 | 来源 |
|-----------|:----:|------|:----:|
| `orderid` | int64 | 订单 ID | 原始 |
| `bikeid` | int64 | 单车 ID | 原始 |
| `userid` | int64 | 用户 ID | 原始 |
| `start_time` | datetime | 骑行开始时间 | 原始 |
| `end_time` | datetime | 骑行结束时间 | 原始 |
| `start_location_x(°)` | float64 | 起点经度 | 原始 |
| `start_location_y(°)` | float64 | 起点纬度 | 原始 |
| `end_location_x(°)` | float64 | 终点经度 | 原始 |
| `end_location_y(°)` | float64 | 终点纬度 | 原始 |
| `track` | string | GPS 轨迹 | 原始 |
| `start_hour(h)` | int64 | 开始小时（0~23） | 衍生 |
| `start_weekday` | int64 | 星期（0=周一~6=周日） | 衍生 |
| `start_day` | int64 | 日期（1~31） | 衍生 |
| `duration_min(min)` | float64 | 骑行时长 | 衍生 |
| `distance_km(km)` | float64 | 骑行距离（经纬度近似） | 衍生 |
| `speed_kmh(km/h)` | float64 | 平均骑行速度 | 衍生 |
| `time_period` | string | 时间段（早/晚高峰/日间/夜间） | 衍生 |
| `is_weekend` | int64 | 是否周末（1=是, 0=否） | 衍生 |
| `天气` | string | 当日天气（原始描述） | 天气合并 |
| `max_temp(℃)` | float64 | 当日最高气温 | 天气清洗 |
| `min_temp(℃)` | float64 | 当日最低气温 | 天气清洗 |
| `avg_temp(℃)` | float64 | 当日平均 `(最高+最低)/2` | 天气清洗 |
| `sunshine_h(h)` | float64 | 当日日照时长 | 天气清洗 |
| `actual_temp(℃)` | float64 | **按时间段取的真实温度** | 衍生 |

### 用户特征数据 (`共享单车_用户特征.csv` — Notebook A 生成)

16,887 条（每个用户一行）

| 字段(单位) | 类型 | 说明 |
|-----------|:----:|------|
| `userid` | int64 | 用户 ID |
| `recency(day)` | int64 | 最近骑行距今天数 |
| `frequency(次)` | int64 | 当月骑行总次数 |
| `total_distance(km)` | float64 | 当月骑行总里程 |
| `total_duration(min)` | float64 | 当月骑行总时长 |
| `avg_duration(min)` | float64 | 平均每次骑行时长 |
| `avg_speed(km/h)` | float64 | 平均骑行速度 |

### 单车特征数据 (`共享单车_单车特征.csv` — Notebook A 生成)

79,062 条（每辆单车一行）

| 字段(单位) | 类型 | 说明 |
|-----------|:----:|------|
| `bikeid` | int64 | 单车 ID |
| `ride_count(次)` | int64 | 当月被骑行次数 |
| `total_distance(km)` | float64 | 当月总骑行里程 |
| `avg_distance(km)` | float64 | 平均每次骑行距离 |
| `total_duration(min)` | float64 | 当月总骑行时长 |
| `avg_duration(min)` | float64 | 平均每次骑行时长 |

---

## 📁 项目结构

```
bike-sharing-analysis/
??? ?? data/                                # ????
?   ??? ????????.csv                  # ?????? (102,361?)
?   ??? ????.xlsx                        # ?????? (31?)
?   ??? ????_???.csv                   # A ??: ??+??+????
?   ??? ????_????.csv                 # A ??: ??RFM????
?   ??? ????_????.csv                 # A ??: ??????
?   ??? ????_?????.csv               # A ??: ?????
?   ??? rfm_standardized.csv                # C1-C3 ??: ???RFM
?   ??? rfm_clustered.csv                   # C4-C5 ??: ?cluster??
?   ??? ????_RFM??.csv                 # C6-C10 ??: ??????
??? ?? code/                                # ?? Notebook
?   ??? ?????????????_zfh.ipynb   # A: ????????? (zfh)
?   ??? ????????.ipynb                # ????
?   ??? ????EDA_B1_hls.ipynb             # B1: ?????? (hls)
?   ??? ????EDA_B2_hls.ipynb             # B2: ?????? (hls)
?   ??? ????EDA_B3-B4_ljh.ipynb          # B3-B4: ??+???? (ljh)
?   ??? ????RFM_C1-C3_hls.ipynb          # C1-C3: ???? (hls)
?   ??? ????RFM_C4-C5_ljh.ipynb          # C4-C5: ?? (ljh)
?   ??? ????RFM_C6-C10_wl.ipynb          # C6-C10: ????? (wl)
??? README.md                               # ????main        ← 稳定版本（保护分支，需 PR 合并）
└── dev     ← 开发主线
    ├── feat/data-cleaning      ← 数据清洗分支
    ├── feat/eda                ← EDA分析分支
    │   ├── feat/eda-time       ← B1时间维度（子任务）
    │   ├── feat/eda-user       ← B2用户行为（子任务）
    │   ├── feat/eda-weather    ← B3天气影响（子任务）
    │   └── feat/eda-spatial    ← B4空间分布（子任务）
    └── feat/rfm                ← RFM分层分支
```

### 认领任务流程

```bash
# 1. 从 dev 拉最新代码
git checkout dev
git pull origin dev

# 2. 创建自己的功能分支
git checkout -b feat/eda-time

# 3. 写代码 → 提交 → 推送
git add .
git commit -m "feat: 添加各小时骑行量分布分析"
git push -u origin feat/eda-time

# 4. 在 GitHub 上提 Pull Request → 合并到 dev
```

### Commit 规范

```
feat:      新功能/分析任务
fix:       修复 bug / 数据异常
docs:      文档更新（README等）
refactor:  重构代码逻辑
style:     格式化、调整样式
chore:     杂项（.gitignore、配置等）
```

### 大体任务

| 任务 | 
|------|
| A - 数据清洗与特征工程 |
| B1 - 时间维度分析 | 
| B2 - 用户行为分析 | 
| B3 - 天气影响分析 | 
| B4 - 空间分布分析 |
| C - RFM用户分层 | 

---

## 🚀 环境要求

```bash
pip install numpy pandas matplotlib seaborn scikit-learn openpyxl
```

## 🔧 运行顺序

```
Notebook A (数据清洗) → Notebook B (EDA) → Notebook C (RFM分层)
```

## 📝 License

MIT
