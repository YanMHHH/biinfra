# Net-a-porter toB 数据服务方案
*基于 Modesens toB Data Service Framework*

---

## 📋 服务参数确认

| 参数 | 值 |
|-----|---|
| **服务对象类型** | Store |
| **客户名称** | Net-a-porter (NAP) |
| **竞品范围** | Mytheresa, Moda Operandi, Neiman Marcus, SSENSE, Farfetch |
| **数据范围** | 包含订单数据 |
| **时间范围** | 建议近 90 天（可调整） |
| **核心 KPI** | Sales, Orders, AOV, Conversion Rate, Return Rate, Impressions, Clicks |

---

## 一、数据逻辑设计

### 1.1 数据主题域架构（Store 客户专用）

```
NAP 竞争力分析体系
│
├── 【主题域 1】竞对对比分析
│   ├── 商品覆盖度对比
│   │   ├── NAP vs 各竞品的 SKU 数量
│   │   ├── 品牌覆盖度对比
│   │   └── 品类覆盖度对比
│   ├── 价格竞争力指数
│   │   ├── 平均价格排名
│   │   ├── 价格优势商品占比
│   │   └── 品牌级价格胜率
│   ├── 库存丰富度对比
│   │   ├── 在售 SKU 数量
│   │   ├── 缺货率对比
│   │   └── 库存深度指数
│   └── 流量转化对比
│       ├── Impressions 对比
│       ├── Click-through Rate
│       ├── Conversion Rate 对比
│       └── 市场份额（GMV）
│
├── 【主题域 2】订单表现分析
│   ├── GMV 趋势
│   │   ├── 日/周/月 Sales 趋势
│   │   ├── NAP vs 竞品趋势对比
│   │   └── 同比/环比增长率
│   ├── 订单量及客单价
│   │   ├── Orders 趋势
│   │   ├── AOV 分布及趋势
│   │   └── AOV vs 竞品对比
│   ├── Top 商品/品牌/品类
│   │   ├── Top 50 畅销商品
│   │   ├── Top 50 品牌 Sales 排行
│   │   └── 品类 Sales 贡献
│   └── 转化与留存
│       ├── Conversion Rate 趋势
│       ├── Return Rate 分析
│       └── 复购率（如有数据）
│
├── 【主题域 3】定价策略分析
│   ├── 同商品多 Store 价格分布
│   │   ├── NAP 价格最高的商品清单
│   │   ├── NAP 价格最低的商品清单
│   │   ├── NAP 价格居中的商品清单
│   │   └── 价格定位分布（高/中/低占比）
│   ├── 价格排名
│   │   ├── 商品级价格排名（1st/2nd/3rd...）
│   │   ├── 价格差异金额/百分比
│   │   └── 价格优化机会识别
│   ├── 品牌级价格竞争力
│   │   ├── NAP 持续价格优势的品牌（Win）
│   │   ├── NAP 持续价格劣势的品牌（Lose）
│   │   ├── 品牌价格胜率排行
│   │   └── 品牌级定价策略建议
│   └── 折扣力度分析
│       ├── 折扣商品占比
│       ├── 平均折扣幅度 vs 竞品
│       └── 折扣对转化率的影响
│
└── 【主题域 4】库存健康度
    ├── 库存状态分布
    │   ├── 有货/缺货/预售商品数
    │   ├── 缺货率趋势
    │   └── 库存状态 vs 竞品对比
    ├── 缺货商品分析
    │   ├── NAP 缺货但竞品有货的商品
    │   ├── 缺货商品的历史 Sales
    │   └── 补货优先级评分
    ├── 商品组合缺口
    │   ├── 竞品有但 NAP 没有的品牌
    │   ├── 竞品有但 NAP 没有的商品
    │   ├── 缺口商品在竞品的表现
    │   └── 品类深度对比（竞品选择更多的品类）
    └── 库存周转
        ├── 滞销商品识别
        ├── 库存周转率
        └── 库存健康度评分
```

---

## 二、关键指标体系

### 2.1 核心 KPI 定义

| 指标 | 计算公式 | 数据源 | 业务含义 |
|-----|---------|-------|---------|
| Sales (GMV) | SUM(订单金额) | order_data | 总成交额 |
| Orders | COUNT(DISTINCT order_id) | order_data | 订单量 |
| AOV | Sales / Orders | 计算字段 | 客单价 |
| Conversion Rate | Orders / Clicks × 100% | order_data + traffic_data | 转化效率 |
| Return Rate | 退货订单数 / 总订单数 × 100% | order_data | 退货率 |
| Impressions | COUNT(impression 事件) | traffic_data | 曝光量 |
| Clicks | COUNT(click 事件) | traffic_data | 点击量 |

### 2.2 定价分析指标

| 指标 | 计算逻辑 | 业务价值 |
|-----|---------|---------|
| 价格定位 | IF(NAP价格=MIN,'Lowest', IF(NAP价格=MAX,'Highest','Average')) | 识别价格竞争力 |
| 价格排名 | RANK() OVER (PARTITION BY product_id ORDER BY price) | 同商品价格排名 |
| 品牌价格胜率 | COUNT(NAP最低价商品) / COUNT(品牌总商品) × 100% | 品牌价格优势频率 |

### 2.3 商品组合指标

| 指标 | 计算逻辑 | 业务价值 |
|-----|---------|---------|
| 品牌覆盖度 | COUNT(DISTINCT NAP品牌) / COUNT(DISTINCT 市场总品牌) × 100% | 品牌覆盖广度 |
| 品类深度指数 | NAP某品类SKU数 / 竞品平均某品类SKU数 | 品类选择丰富度 |
| 商品缺口数 | COUNT(竞品有但NAP无的商品) | 商品组合缺口 |

### 2.4 重叠度指标

| 指标 | 计算逻辑 | 业务价值 |
|-----|---------|---------|
| 商品重叠度 | COUNT(共同商品) / COUNT(NAP总商品) × 100% | 商品重合程度 |
| 重叠商品价格胜率 | COUNT(NAP价格最低的重叠商品) / COUNT(重叠商品) × 100% | 直接竞争价格优势 |
| 重叠商品GMV占比 | 重叠商品Sales / NAP总Sales × 100% | 重叠商品业务重要性 |

---

## 三、看板设计方案

### 3.1 看板层级结构

```
Level 1: Executive Dashboard（高管总览）
    ↓
Level 2: 主题分析页
    ├── Business Performance（业务表现）
    ├── Pricing Intelligence（定价洞察）
    ├── Assortment Analysis（商品组合）
    └── Overlap Analysis（重叠度分析）
    ↓
Level 3: 明细钻取页
```

---

### 3.2 Executive Dashboard（总览页）

**布局设计：**

```
┌─────────────────────────────────────────────────────────────┐
│  NAP Competitive Intelligence Dashboard                      │
│  时间: [Last 30 Days ▼]  竞品: [All ▼]                       │
├─────────────────────────────────────────────────────────────┤
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐   │
│  │ Sales  │ │ Orders │ │  AOV   │ │  Conv  │ │ Return │   │
│  │ $2.5M  │ │ 1,234  │ │ $2,025 │ │  3.2%  │ │  8.5%  │   │
│  │ ↑ 12%  │ │ ↑ 8%   │ │ ↑ 4%   │ │ ↓ 0.3% │ │ ↑ 1.2% │   │
│  └────────┘ └────────┘ └────────┘ └────────┘ └────────┘   │
├──────────────────────────┬──────────────────────────────────┤
│  Sales Trend (折线图)     │  Market Share (饼图)             │
│  NAP vs 竞品时间序列      │  NAP: 28%, Myth: 22%, 其他: 50% │
├──────────────────────────┼──────────────────────────────────┤
│  Pricing Position (柱图)  │  Assortment Gap (条形图)         │
│  Highest/Average/Lowest   │  各竞品缺口商品数                 │
└──────────────────────────┴──────────────────────────────────┘
```

**图表配置：**

| 组件 | 图表类型 | 维度 | 指标 | 交互 |
|-----|---------|-----|------|------|
| KPI Cards | 数字卡片 | - | Sales, Orders, AOV, Conv, Return | 点击钻取 |
| Sales Trend | 多线折线图 | 日期 | Sales | 图例筛选 |
| Market Share | 饼图 | Store | Sales | 点击筛选 |
| Pricing Position | 堆叠柱状图 | 价格定位 | 商品数 | 钻取商品清单 |
| Assortment Gap | 横向条形图 | 竞品 | 缺口数 | 钻取缺口明细 |

---

### 3.3 Pricing Intelligence（定价洞察页）

#### Tab 1: Price Position Overview（价格定位总览）

```
┌─────────────────────────────────────────────────────────────┐
│  Pricing Intelligence - Overview                             │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────┬──────────┬──────────┬──────────┐             │
│  │ Highest  │ Average  │ Lowest   │ Win Rate │             │
│  │ 1,234    │ 5,678    │ 1,234    │   45%    │             │
│  └──────────┴──────────┴──────────┴──────────┘             │
├──────────────────────────┬──────────────────────────────────┤
│  Brand Price Win Rate    │  Price Position by Category      │
│  (横向条形图)             │  (热力图)                         │
│  显示 NAP 价格胜率 Top 20 │  品类 × 价格定位                  │
├──────────────────────────┴──────────────────────────────────┤
│  Same Product Price Comparison (表格)                        │
│  Product | NAP | Myth | Farf | SSEN | Moda | Rank          │
└─────────────────────────────────────────────────────────────┘
```

#### Tab 2: NAP Price Position Analysis（NAP 价格定位分析）

**核心问题：**
- Where is NAP priced highest, lowest, and average vs competitors on the same product?
- Which brands does NAP consistently win or lose on price?

```
┌─────────────────────────────────────────────────────────────┐
│  NAP Price Position Analysis                                 │
│  筛选: [品牌 ▼] [品类 ▼] [价格区间 ▼] [时间 ▼]              │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────┬──────────────┬──────────────┐            │
│  │ NAP Highest  │ NAP Average  │ NAP Lowest   │            │
│  │ 1,234 SKUs   │ 5,678 SKUs   │ 2,345 SKUs   │            │
│  │ (13.5%)      │ (62.1%)      │ (24.4%)      │            │
│  └──────────────┴──────────────┴──────────────┘            │
├─────────────────────────────────────────────────────────────┤
│  Price Position Distribution (堆叠柱状图)                    │
│  按品类/品牌显示 NAP 在 Highest/Average/Lowest 的商品占比   │
│  X轴: 品类/品牌  Y轴: 商品数  颜色: Highest/Average/Lowest  │
├──────────────────────────┬──────────────────────────────────┤
│  NAP Highest Price       │  NAP Lowest Price                │
│  Products (表格)         │  Products (表格)                 │
│  ┌─────────────────────┐│  ┌─────────────────────┐         │
│  │Product│Brand│NAP    ││  │Product│Brand│NAP    │         │
│  │       │     │Price  ││  │       │     │Price  │         │
│  │       │     │vs Min ││  │       │     │vs Max │         │
│  │       │     │Gap    ││  │       │     │Gap    │         │
│  └─────────────────────┘│  └─────────────────────┘         │
│  点击可钻取查看竞品价格  │  点击可钻取查看竞品价格           │
└──────────────────────────┴──────────────────────────────────┘
```

**图表 1: Price Position Distribution（价格定位分布）**

| 组件 | 图表类型 | 维度 | 指标 | 交互 |
|-----|---------|-----|------|------|
| Price Position | 堆叠柱状图 | 品类/品牌 | 商品数 | 筛选器切换 |
| 颜色编码 | - | Highest(红), Average(黄), Lowest(绿) | - | 图例筛选 |

**图表 2: NAP Highest Price Products（NAP 价格最高商品清单）**

| 字段 | 说明 | 计算逻辑 |
|-----|------|---------|
| Product Name | 商品名称 | - |
| Brand | 品牌 | - |
| Category | 品类 | - |
| NAP Price | NAP 售价 | - |
| Min Competitor Price | 竞品最低价 | MIN(竞品价格) |
| Price Gap | 价格差 | NAP Price - Min Price |
| Gap % | 价格差百分比 | (NAP Price - Min Price) / Min Price × 100% |
| Lowest Competitor | 最低价竞品 | 价格最低的 Store 名称 |
| Rank | NAP 价格排名 | RANK() 在所有 Store 中的排名 |

**图表 3: NAP Lowest Price Products（NAP 价格最低商品清单）**

| 字段 | 说明 | 计算逻辑 |
|-----|------|---------|
| Product Name | 商品名称 | - |
| Brand | 品牌 | - |
| Category | 品类 | - |
| NAP Price | NAP 售价 | - |
| Max Competitor Price | 竞品最高价 | MAX(竞品价格) |
| Price Advantage | 价格优势 | Max Price - NAP Price |
| Advantage % | 优势百分比 | (Max Price - NAP Price) / Max Price × 100% |
| Highest Competitor | 最高价竞品 | 价格最高的 Store 名称 |
| Sales Impact | 销售影响 | 该商品的 Sales 数据 |

```
┌─────────────────────────────────────────────────────────────┐
│  Brand-Level Price Win/Loss Analysis                         │
├─────────────────────────────────────────────────────────────┤
│  Brand Price Win Rate (横向条形图)                           │
│  Top 30 品牌按价格胜率排序                                    │
│  X轴: Win Rate (%)  Y轴: 品牌  颜色: Win(绿) / Lose(红)     │
├──────────────────────────┬──────────────────────────────────┤
│  Brands NAP Wins On      │  Brands NAP Loses On             │
│  (表格)                   │  (表格)                           │
│  ┌─────────────────────┐│  ┌─────────────────────┐         │
│  │Brand│Win  │Total    ││  │Brand│Lose │Total    │         │
│  │     │Rate │Products ││  │     │Rate │Products │         │
│  │     │     │Sales    ││  │     │     │Sales    │         │
│  └─────────────────────┘│  └─────────────────────┘         │
└──────────────────────────┴──────────────────────────────────┘
```

**图表 4: Brand Price Win Rate（品牌价格胜率）**

| 字段 | 说明 | 计算逻辑 |
|-----|------|---------|
| Brand | 品牌名称 | - |
| Total Products | 该品牌总商品数 | COUNT(DISTINCT product_id) |
| NAP Lowest Count | NAP 最低价商品数 | COUNT(WHERE NAP价格=MIN价格) |
| Win Rate | 价格胜率 | NAP Lowest Count / Total Products × 100% |
| Avg Price Gap | 平均价格差 | AVG(NAP Price - Min Competitor Price) |
| Total Sales | 该品牌总销售额 | SUM(Sales) |

**图表 5: Brands NAP Consistently Wins（NAP 持续价格优势品牌）**

筛选条件：Win Rate ≥ 60%

| 字段 | 说明 | 业务价值 |
|-----|------|---------|
| Brand | 品牌名称 | - |
| Win Rate | 价格胜率 | NAP 在该品牌的价格竞争力 |
| Total Products | 商品数量 | 品牌规模 |
| Avg Advantage | 平均价格优势 | 平均低于竞品的金额 |
| Sales Contribution | 销售贡献 | 该品牌对 NAP 总 Sales 的贡献 |
| Strategy | 策略建议 | "保持价格优势，扩大商品组合" |

**图表 6: Brands NAP Consistently Loses（NAP 持续价格劣势品牌）**

筛选条件：Win Rate ≤ 30%

| 字段 | 说明 | 业务价值 |
|-----|------|---------|
| Brand | 品牌名称 | - |
| Lose Rate | 价格劣势率 | 100% - Win Rate |
| Total Products | 商品数量 | 品牌规模 |
| Avg Gap | 平均价格差 | 平均高于竞品的金额 |
| Lost Sales Potential | 潜在流失销售 | 基于转化率估算的机会成本 |
| Main Competitor | 主要竞争对手 | 该品牌价格最低的主要 Store |
| Strategy | 策略建议 | "考虑降价或突出差异化价值" |

**核心功能：**
- 识别 NAP 价格最高/最低/平均的商品分布
- 量化 NAP 与竞品的价格差距（金额 & 百分比）
- 品牌级价格胜率分析（Win/Lose）
- 识别 NAP 持续价格优势/劣势的品牌
- 提供品牌级定价策略建议

---

### 3.4 Assortment Analysis（商品组合页）

```
┌─────────────────────────────────────────────────────────────┐
│  Assortment Analysis                                         │
├─────────────────────────────────────────────────────────────┤
│  Top 50 Brands Comparison (气泡图)                           │
│  X轴: NAP SKU数  Y轴: 竞品平均SKU数  气泡: Sales            │
├──────────────────────────┬──────────────────────────────────┤
│  Category Depth (雷达图)  │  Missing Products (表格)         │
│  各品类 NAP vs 竞品深度   │  竞品有但 NAP 缺失的高潜力商品    │
└──────────────────────────┴──────────────────────────────────┘
```

**核心功能：**
- NAP Top 50 品牌 vs 竞品对比
- 竞品选择更多的品类识别
- 缺口商品及市场潜力分析

---

### 3.5 Overlap Analysis（重叠度分析页）

```
┌─────────────────────────────────────────────────────────────┐
│  Overlap Analysis                                            │
├─────────────────────────────────────────────────────────────┤
│  Overlap Metrics by Competitor (表格)                        │
│  Store | Overlap Products | Price Win Rate | Overlap Sales  │
├──────────────────────────┬──────────────────────────────────┤
│  Overlap vs Unique Sales │  Price Advantage Impact (散点图) │
│  (堆叠柱状图)             │  X: 价格优势  Y: 转化率           │
└──────────────────────────┴──────────────────────────────────┘
```

**核心洞察：**
- NAP 与各竞品的共同商品数量
- 在共同商品上的价格胜率
- 价格优势对转化率的影响

---

## 四、数据需求清单

### 4.1 核心数据表

**表1: product_inventory（商品库存表）**
```
- product_id: 商品ID
- store_name: 电商名称
- brand: 品牌
- category: 品类
- product_name: 商品名
- price: 售价
- stock_status: 库存状态
- last_updated: 更新时间
```

**表2: order_data（订单表）**
```
- order_id: 订单ID
- product_id: 商品ID
- store_name: 电商名称
- order_amount: 订单金额
- order_date: 订单日期
- is_returned: 是否退货
```

**表3: traffic_data（流量表）**
```
- event_id: 事件ID
- product_id: 商品ID
- store_name: 电商名称
- event_type: impression/click
- event_date: 事件日期
```

---

### 4.2 数据加工逻辑

**商品匹配表：**
```sql
CREATE TABLE product_mapping AS
SELECT 
    product_group_id,
    product_id,
    store_name,
    brand,
    product_name
FROM product_inventory
WHERE 品牌+商品名匹配
GROUP BY product_group_id;
```

**价格对比宽表：**
```sql
CREATE TABLE price_comparison AS
SELECT
    product_group_id,
    MAX(CASE WHEN store_name='NAP' THEN price END) AS nap_price,
    MAX(CASE WHEN store_name='Mytheresa' THEN price END) AS myth_price,
    MAX(CASE WHEN store_name='Farfetch' THEN price END) AS farf_price,
    MIN(price) AS min_price,
    MAX(price) AS max_price
FROM product_inventory
GROUP BY product_group_id;
```

---

## 五、实施建议

### 5.1 开发优先级

**Phase 1: MVP（2-3周）**
- Executive Dashboard
- 核心 KPI + Sales Trend
- Pricing Position 基础分析

**Phase 2: 核心功能（3-4周）**
- Pricing Intelligence 完整页
- Assortment Analysis 页

**Phase 3: 完整版（2-3周）**
- Overlap Analysis 页
- 明细钻取功能

### 5.2 技术选型

- BI 工具：Tableau / QuickSight / Power BI
- 数据仓库：Snowflake / BigQuery
- 更新频率：库存日更新，订单准实时

### 5.3 关键成功因素

1. 商品匹配准确性（同商品识别）
2. 数据时效性（价格/库存高频更新）
3. 看板性能（加载 <3秒）
4. 可操作性（洞察转化为决策）

---

## 六、方案总结

为 Net-a-porter 设计的竞争力分析体系包括：

- **4大主题域**：竞对对比、订单表现、定价策略、库存健康
- **20+核心指标**：覆盖 Sales、价格、商品组合、重叠度
- **5个看板页面**：总览 + 4个主题分析页
- **3阶段实施**：8-10周完整交付

**核心价值：**
- 实时监控 5 家竞品竞争态势
- 识别定价优化机会（价格胜率提升）
- 发现商品组合缺口（补货优先级）
- 量化重叠商品竞争效果

建议先实施 MVP 验证数据质量和业务价值，再逐步完善。
