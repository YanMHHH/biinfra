# KPI 卡片 (KPI Card) 规范

本文档定义 QuickSight 原型中 KPI 卡片的生成规范。

---

## 一、基本结构

### 1.1 标准 KPI 卡片（带标题）
```html
<div class="visual-card">
    <div class="chart-title">KPI 标题</div>
    <div style="text-align: center;">
        <div style="font-size: 32px; font-weight: bold; color: #0073bb;">$125,430</div>
        <div style="font-size: 11px; color: var(--qs-text-muted); margin-top: 5px;">指标说明</div>
    </div>
</div>
```

### 1.2 带进度条的 KPI 卡片
```html
<div class="visual-card">
    <div class="chart-title">KPI 标题</div>
    <div style="text-align: center;">
        <div style="font-size: 11px; color: var(--qs-text-muted); margin-bottom: 8px;">指标说明</div>
        <div style="font-size: 32px; font-weight: bold; color: #d13212; margin-bottom: 10px;">1,245</div>
        <div style="background: #f0f0f0; height: 20px; border-radius: 3px; overflow: hidden;">
            <div style="background: #d13212; height: 100%; width: 28%; display: flex; align-items: center; justify-content: center; color: white; font-size: 11px; font-weight: bold;">28%</div>
        </div>
    </div>
</div>
```

---

## 二、样式规范

### 2.1 标题位置
- 标题放置在卡片左上角
- 使用 `.chart-title` 类
- 字体大小：15px
- 字体粗细：bold

### 2.2 数值样式
- 字体大小：32px
- 字体粗细：bold
- 颜色根据业务含义选择（见下方颜色方案）

### 2.3 说明文字
- 字体大小：11px
- 颜色：`var(--qs-text-muted)`（灰色）
- 位置：数值上方或下方

### 2.4 进度条样式
- 背景色：`#f0f0f0`（浅灰）
- 高度：20px
- 圆角：3px
- 进度条颜色：根据业务含义选择
- 百分比文字：11px，bold，白色

---

## 三、颜色方案

### 3.1 业务语义颜色

| 业务含义 | 颜色值 | 适用场景 |
|:---|:---|:---|
| 主色调 | `#0073bb` | 常规指标（销售额、订单数等） |
| 高价/警告 | `#d13212` | 高价商品、异常指标 |
| 中性 | `#879596` | 平均值、中性指标 |
| 低价/成功 | `#1d8102` | 低价商品、正向指标 |

---

## 四、布局方式

### 4.1 单个 KPI
```html
<div class="visual-card">
    <div class="chart-title">Total Sales</div>
    <div style="text-align: center;">
        <div style="font-size: 32px; font-weight: bold; color: #0073bb;">$125,430</div>
    </div>
</div>
```

### 4.2 三个 KPI 横排
```html
<div class="row">
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">Total Sales</div>
            <div style="text-align: center;">
                <div style="font-size: 32px; font-weight: bold; color: #0073bb;">$125,430</div>
            </div>
        </div>
    </div>
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">Orders</div>
            <div style="text-align: center;">
                <div style="font-size: 32px; font-weight: bold; color: #0073bb;">3,245</div>
            </div>
        </div>
    </div>
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">AOV</div>
            <div style="text-align: center;">
                <div style="font-size: 32px; font-weight: bold; color: #0073bb;">$38.65</div>
            </div>
        </div>
    </div>
</div>
```

---

## 五、完整示例

### 5.1 价格分析 KPI（带进度条）
```html
<div class="row">
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">Priced Highest</div>
            <div style="text-align: center;">
                <div style="font-size: 32px; font-weight: bold; color: #d13212; margin-bottom: 10px;">1,245</div>
                <div style="background: #f0f0f0; height: 20px; border-radius: 3px; overflow: hidden;">
                    <div style="background: #d13212; height: 100%; width: 28%; display: flex; align-items: center; justify-content: center; color: white; font-size: 11px; font-weight: bold;">28%</div>
                </div>
            </div>
        </div>
    </div>
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">Priced Average</div>
            <div style="text-align: center;">
                <div style="font-size: 32px; font-weight: bold; color: #879596; margin-bottom: 10px;">2,890</div>
                <div style="background: #f0f0f0; height: 20px; border-radius: 3px; overflow: hidden;">
                    <div style="background: #879596; height: 100%; width: 65%; display: flex; align-items: center; justify-content: center; color: white; font-size: 11px; font-weight: bold;">65%</div>
                </div>
            </div>
        </div>
    </div>
    <div class="col">
        <div class="visual-card">
            <div class="chart-title">Priced Lowest</div>
            <div style="text-align: center;">
                <div style="font-size: 32px; font-weight: bold; color: #1d8102; margin-bottom: 10px;">312</div>
                <div style="background: #f0f0f0; height: 20px; border-radius: 3px; overflow: hidden;">
                    <div style="background: #1d8102; height: 100%; width: 7%; display: flex; align-items: center; justify-content: center; color: white; font-size: 11px; font-weight: bold;">7%</div>
                </div>
            </div>
        </div>
    </div>
</div>
```
