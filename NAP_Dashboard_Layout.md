# Net-a-porter toB 数据看板布局设计

---

## 看板结构

```
Tab 1: 销售对比
Tab 2: 商品定价
Tab 3: 商品库存
```

---

## Tab 1: 销售对比

### Section 1: KPI 对比表

**组件类型：** Table

**列定义：**
| Store Name | Orders | Sales | AOV | Impressions | Clicks | Conversion Rate | Return Rate |
|-----------|--------|-------|-----|-------------|--------|----------------|-------------|
| NET-A-PORTER | 1,234 | $2.5M | $2,025 | 125K | 38.5K | 3.2% | 8.5% |
| Competitor MAX | 1,456 | $2.8M | $2,100 | 145K | 42K | 3.8% | 9.1% |
| Competitor AVG | 1,180 | $2.1M | $1,780 | 118K | 35K | 3.4% | 7.8% |
| Competitor MIN | 890 | $1.5M | $1,336 | 95K | 28K | 2.9% | 6.8% |

**说明：**
- Competitor MAX: 各竞品中该指标的最大值
- Competitor AVG: 各竞品该指标的平均值
- Competitor MIN: 各竞品中该指标的最小值

---

### Section 2: 订单与销售趋势

**布局：** 3个趋势图横向排列

#### 图表 1: Orders Daily Trend
- **图表类型：** 折线图
- **X轴：** 日期
- **Y轴：** Orders
- **折线：**
  - NET-A-PORTER（蓝色实线）
  - Competitor MAX（红色虚线）
  - Competitor AVG（灰色实线）
  - Competitor MIN（绿色虚线）

#### 图表 2: Sales Daily Trend
- **图表类型：** 折线图
- **X轴：** 日期
- **Y轴：** Sales ($)
- **折线：**
  - NET-A-PORTER（蓝色实线）
  - Competitor MAX（红色虚线）
  - Competitor AVG（灰色实线）
  - Competitor MIN（绿色虚线）

#### 图表 3: AOV Daily Trend
- **图表类型：** 折线图
- **X轴：** 日期
- **Y轴：** AOV ($)
- **折线：**
  - NET-A-PORTER（蓝色实线）
  - Competitor MAX（红色虚线）
  - Competitor AVG（灰色实线）
  - Competitor MIN（绿色虚线）

---

### Section 3: 流量与转化趋势

**布局：** 3个趋势图横向排列

#### 图表 4: Impressions Daily Trend
- **图表类型：** 折线图
- **X轴：** 日期
- **Y轴：** Impressions
- **折线：**
  - NET-A-PORTER（蓝色实线）
  - Competitor MAX（红色虚线）
  - Competitor AVG（灰色实线）
  - Competitor MIN（绿色虚线）

#### 图表 5: Clicks Daily Trend
- **图表类型：** 折线图
- **X轴：** 日期
- **Y轴：** Clicks
- **折线：**
  - NET-A-PORTER（蓝色实线）
  - Competitor MAX（红色虚线）
  - Competitor AVG（灰色实线）
  - Competitor MIN（绿色虚线）

#### 图表 6: Conversion Rate Daily Trend
- **图表类型：** 折线图
- **X轴：** 日期
- **Y轴：** Conversion Rate (%)
- **折线：**
  - NET-A-PORTER（蓝色实线）
  - Competitor MAX（红色虚线）
  - Competitor AVG（灰色实线）
  - Competitor MIN（绿色虚线）

---

### Section 4: 退货率趋势

**布局：** 1个趋势图，宽度与上述折线图相同

#### 图表 7: Return Rate Daily Trend
- **图表类型：** 折线图
- **X轴：** 日期
- **Y轴：** Return Rate (%)
- **折线：**
  - NET-A-PORTER（蓝色实线）
  - Competitor MAX（红色虚线）
  - Competitor AVG（灰色实线）
  - Competitor MIN（绿色虚线）

---

## Tab 2: 商品定价

### Section 1: 价格定位 KPI

**组件类型：** KPI Cards（4个卡片横向排列）

| KPI | 值 | 说明 |
|-----|---|------|
| NAP Highest | 1,234 (13.5%) | NAP 价格最高的商品数及占比 |
| NAP Average | 5,678 (62.1%) | NAP 价格居中的商品数及占比 |
| NAP Lowest | 2,345 (24.4%) | NAP 价格最低的商品数及占比 |
| Price Win Rate | 45% | NAP 价格最低商品占比（价格胜率）|

---

### Section 2: 价格定位分布

**布局：** 2个图表横向排列

#### 图表 1: Price Position by Category
- **图表类型：** 堆叠柱状图
- **X轴：** 品类（Category）
- **Y轴：** 商品数量
- **堆叠维度：**
  - NAP Highest（红色）
  - NAP Average（黄色）
  - NAP Lowest（绿色）
- **交互：** 点击柱状图钻取到商品明细

#### 图表 2: Price Position by Brand (Top 20)
- **图表类型：** 堆叠柱状图
- **X轴：** 品牌（Brand）- 按总商品数排序 Top 20
- **Y轴：** 商品数量
- **堆叠维度：**
  - NAP Highest（红色）
  - NAP Average（黄色）
  - NAP Lowest（绿色）
- **交互：** 点击柱状图钻取到商品明细

---

### Section 3: NAP 价格最高/最低商品清单

**布局：** 2个表格横向排列

#### 表格 1: NAP Highest Price Products（Top 50）
**列定义：**
| Product Name | Brand | Category | NAP Price | Min Competitor Price | Price Gap | Gap % | Lowest Competitor | Rank |
|--------------|-------|----------|-----------|---------------------|-----------|-------|-------------------|------|
| Product A | Gucci | Bags | $2,500 | $2,200 | $300 | 13.6% | Mytheresa | 5/5 |

**说明：**
- Price Gap = NAP Price - Min Competitor Price
- Gap % = Price Gap / Min Competitor Price × 100%
- Rank = NAP 在所有 Store 中的价格排名

#### 表格 2: NAP Lowest Price Products（Top 50）
**列定义：**
| Product Name | Brand | Category | NAP Price | Max Competitor Price | Price Advantage | Advantage % | Highest Competitor | Sales |
|--------------|-------|----------|-----------|---------------------|----------------|-------------|-------------------|-------|
| Product B | Prada | Shoes | $850 | $950 | $100 | 10.5% | Farfetch | $12.5K |

**说明：**
- Price Advantage = Max Competitor Price - NAP Price
- Advantage % = Price Advantage / Max Competitor Price × 100%
- Sales = 该商品在 NAP 的销售额

---

### Section 4: 品牌价格胜率分析

**布局：** 1个图表 + 2个表格

#### 图表 3: Brand Price Win Rate（Top 30）
- **图表类型：** 横向条形图
- **X轴：** Win Rate (%)
- **Y轴：** 品牌（按 Win Rate 降序排列）
- **颜色编码：**
  - Win Rate ≥ 60%（绿色）
  - 30% < Win Rate < 60%（黄色）
  - Win Rate ≤ 30%（红色）
- **交互：** 点击品牌钻取到该品牌商品明细

#### 表格 3: Brands NAP Wins On（Win Rate ≥ 60%）
**列定义：**
| Brand | Win Rate | Total Products | Avg Advantage | Sales Contribution | Strategy |
|-------|----------|----------------|---------------|-------------------|----------|
| Brand X | 75% | 120 | $85 | $250K (10%) | 保持价格优势，扩大商品组合 |

#### 表格 4: Brands NAP Loses On（Win Rate ≤ 30%）
**列定义：**
| Brand | Lose Rate | Total Products | Avg Gap | Lost Sales Potential | Main Competitor | Strategy |
|-------|-----------|----------------|---------|---------------------|----------------|----------|
| Brand Y | 80% | 95 | $120 | $180K | SSENSE | 考虑降价或突出差异化价值 |

---

---

## Tab 3: 商品库存

（待补充具体布局需求）

---

## 视觉布局示意

```
┌─────────────────────────────────────────────────────────────┐
│  Tab: [销售对比] [商品定价] [商品库存]                        │
├─────────────────────────────────────────────────────────────┤
│  Section 1: KPI 对比表                                       │
│  ┌─────────────────────────────────────────────────────────┐│
│  │ Store Name | Orders | Sales | AOV | Impr | Clicks | ... ││
│  │ NAP        | 1,234  | $2.5M | ... | ...  | ...    | ... ││
│  │ Comp MAX   | 1,456  | $2.8M | ... | ...  | ...    | ... ││
│  │ Comp AVG   | 1,180  | $2.1M | ... | ...  | ...    | ... ││
│  │ Comp MIN   |   890  | $1.5M | ... | ...  | ...    | ... ││
│  └─────────────────────────────────────────────────────────┘│
├─────────────────────────────────────────────────────────────┤
│  Section 2: 订单与销售趋势                                    │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐                    │
│  │ Orders   │ │ Sales    │ │ AOV      │                    │
│  │ Trend    │ │ Trend    │ │ Trend    │                    │
│  │          │ │          │ │          │                    │
│  └──────────┘ └──────────┘ └──────────┘                    │
├─────────────────────────────────────────────────────────────┤
│  Section 3: 流量与转化趋势                                    │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐                    │
│  │Impressio │ │ Clicks   │ │ Conv     │                    │
│  │ns Trend  │ │ Trend    │ │ Rate     │                    │
│  │          │ │          │ │ Trend    │                    │
│  └──────────┘ └──────────┘ └──────────┘                    │
├─────────────────────────────────────────────────────────────┤
│  Section 4: 退货率趋势                                        │
│  ┌─────────────────────────────────────────────────────────┐│
│  │ Return Rate Daily Trend                                 ││
│  │                                                         ││
│  └─────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────┘
```
