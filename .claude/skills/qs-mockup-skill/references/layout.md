# QuickSight 布局规范

本文档定义 QuickSight 原型中的布局系统和排版规则。

---

## 一、Section 概念

### 1.1 定义
- Section 是指一个横向区域，内部组件采用左右结构
- Section 本身不需要整体标题
- 每个组件独立显示在自己的卡片中

### 1.2 组件独立性
- 每个图表/表格必须独立在自己的 visual-card 中
- 组件标题显示在各自卡片内部
- 使用 `.row` 和 `.col` 实现横向布局

---

## 二、布局助手

### 2.1 单列布局
直接使用 `<div class="visual-card">组件</div>`

```html
<div class="visual-card">
    <div class="chart-header">
        <div class="chart-title">全宽图表</div>
    </div>
    <canvas id="chart1"></canvas>
</div>
```

### 2.2 两列布局
```html
<div class="row">
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">图表1</div>
            <canvas id="chart1"></canvas>
        </div>
    </div>
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">图表2</div>
            <canvas id="chart2"></canvas>
        </div>
    </div>
</div>
```

### 2.3 三列布局
```html
<div class="row">
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">图表1</div>
            <canvas id="chart1"></canvas>
        </div>
    </div>
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">图表2</div>
            <canvas id="chart2"></canvas>
        </div>
    </div>
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">图表3</div>
            <canvas id="chart3"></canvas>
        </div>
    </div>
</div>
```

---

## 三、非等宽布局

### 3.1 使用场景
当某个图表需要占据特定宽度时，使用 `flex` 和 `max-width` 控制。

### 3.2 示例：1/3 宽度
```html
<div class="row">
    <div class="col" style="flex: 0 0 33.33%; max-width: 33.33%;">
        <div class="visual-card">
            <div class="chart-title">占1/3宽度的图表</div>
            <canvas id="chart1"></canvas>
        </div>
    </div>
</div>
```

### 3.3 示例：2/3 宽度
```html
<div class="row">
    <div class="col" style="flex: 0 0 66.66%; max-width: 66.66%;">
        <div class="visual-card">
            <div class="chart-title">占2/3宽度的图表</div>
            <canvas id="chart1"></canvas>
        </div>
    </div>
</div>
```

---

## 四、完整示例

### 4.1 混合布局示例
```html
<!-- Section 1: 3个KPI横排 -->
<div class="row">
    <div class="col">
        <div class="visual-card kpi-card">
            <div class="kpi-value">$125,430</div>
            <div class="kpi-label">Total Sales</div>
        </div>
    </div>
    <div class="col">
        <div class="visual-card kpi-card">
            <div class="kpi-value">3,245</div>
            <div class="kpi-label">Orders</div>
        </div>
    </div>
    <div class="col">
        <div class="visual-card kpi-card">
            <div class="kpi-value">$38.65</div>
            <div class="kpi-label">AOV</div>
        </div>
    </div>
</div>

<!-- Section 2: 分隔标题 -->
<div class="visual-card section-divider">Daily Trends</div>

<!-- Section 3: 3个趋势图横排 -->
<div class="row">
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">Orders Daily Trend</div>
            <canvas id="ordersChart"></canvas>
        </div>
    </div>
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">Sales Daily Trend</div>
            <canvas id="salesChart"></canvas>
        </div>
    </div>
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">AOV Daily Trend</div>
            <canvas id="aovChart"></canvas>
        </div>
    </div>
</div>

<!-- Section 4: 全宽表格 -->
<div class="visual-card">
    <div class="chart-title">Product Performance</div>
    <div class="table-scroll-container">
        <table class="qs-table">
            <thead><tr><th>Product</th><th>Sales</th></tr></thead>
            <tbody><tr><td>Product A</td><td>$1,234</td></tr></tbody>
        </table>
    </div>
</div>
```
