# 趋势图 (Line Chart) 规范

本文档定义 QuickSight 原型中趋势图的生成规范。

---

## 一、实现方式

### 1.1 技术选型
优先使用 Chart.js 库（CDN: `https://cdn.jsdelivr.net/npm/chart.js`）

### 1.2 基本结构
```html
<div class="visual-card">
    <div class="chart-header">
        <div class="chart-title">图表标题</div>
        <div class="chart-description">图表说明（可选）</div>
    </div>
    <canvas id="chartId"></canvas>
</div>
```

### 1.3 Canvas自适应
模板已包含以下CSS确保图表随页面宽度自适应：
```css
canvas {
    max-width: 100%;
    height: auto !important;
}
```

---

## 二、Chart.js 标准配置

### 2.1 配置模板
```javascript
const cfg = {
    type: 'line',
    options: {
        responsive: true,
        maintainAspectRatio: true,
        aspectRatio: 1 / 0.9,  // 高度为宽度的0.9倍
        plugins: {
            legend: {
                display: true,
                position: 'top',
                labels: {
                    boxWidth: 12,
                    boxHeight: 12,
                    font: { size: 11 }
                }
            }
        },
        scales: {
            y: { beginAtZero: false }
        }
    }
};
```

---

## 三、X轴标签配置

### 3.1 标签旋转与间隔
为避免日期标签重叠，使用以下配置：
- **旋转角度**：0°（水平显示）
- **标签间隔**：每3个数据点显示一个标签

### 3.2 X轴配置示例
```javascript
scales: {
    x: {
        ticks: {
            maxRotation: 0,
            minRotation: 0,
            callback: function(value, index) {
                return index % 3 === 0 ? this.getLabelForValue(value) : '';
            }
        }
    }
}
```

---

## 四、横轴时间格式

根据时间粒度使用不同格式：

| 粒度 | 格式 | 示例 | 生成方法 |
|:---|:---|:---|:---|
| 天 | `YYYY-MM-DD` | 2026-03-23 | `toISOString().split('T')[0]` |
| 月 | `MMM YYYY` | Jan 2026 | 手动格式化 |
| 季 | `QN YYYY` | Q1 2026 | 手动计算 |
| 年 | `YYYY` | 2026 | `getFullYear()` |

**生成30天日期示例：**
```javascript
const dates = Array.from({length: 30}, (_, i) => {
    const d = new Date();
    d.setDate(d.getDate() - 29 + i);
    return d.toISOString().split('T')[0];
});
```

---

## 五、Y轴单位格式化

### 4.1 格式化规则

| 指标类型 | 单位 | 实现方式 |
|:---|:---|:---|
| 金额类（Sales、AOV） | `$` 前缀 | `callback: v => '$' + v.toLocaleString()` |
| 百分比类（Conversion Rate、Return Rate） | `%` 后缀 | `callback: v => v.toFixed(1) + '%'` |
| 数量类（Orders、Clicks） | 无单位 | 默认 |

### 4.2 金额类图表示例
```javascript
new Chart(document.getElementById('salesChart'), {
    type: 'line',
    data: {
        labels: dates,
        datasets: [{
            label: 'Sales',
            data: [1200, 1350, 1180, ...],
            borderColor: '#0073bb',
            tension: 0.1
        }]
    },
    options: {
        ...cfg.options,
        scales: {
            y: {
                beginAtZero: false,
                ticks: { callback: v => '$' + v.toLocaleString() }
            }
        }
    }
});
```

### 4.3 百分比类图表示例
```javascript
new Chart(document.getElementById('conversionChart'), {
    type: 'line',
    data: {
        labels: dates,
        datasets: [{
            label: 'Conversion Rate',
            data: [2.3, 2.5, 2.1, ...],
            borderColor: '#0073bb',
            tension: 0.1
        }]
    },
    options: {
        ...cfg.options,
        scales: {
            y: {
                beginAtZero: false,
                ticks: { callback: v => v.toFixed(1) + '%' }
            }
        }
    }
});
```

---

## 六、多线图例

### 5.1 颜色方案
- 主色：`#0073bb`（蓝色）
- 次色：`#879596`（灰色）
- 可扩展更多颜色

### 5.2 线段样式
支持指定线段样式：
- 实线：`borderDash: []`（默认）
- 虚线：`borderDash: [5, 5]`

### 5.3 多线示例
```javascript
new Chart(document.getElementById('comparisonChart'), {
    type: 'line',
    data: {
        labels: dates,
        datasets: [
            {
                label: 'Current Year',
                data: [1200, 1350, 1180, ...],
                borderColor: '#0073bb',
                borderDash: [],
                tension: 0.1
            },
            {
                label: 'Last Year',
                data: [1100, 1250, 1080, ...],
                borderColor: '#879596',
                borderDash: [5, 5],
                tension: 0.1
            }
        ]
    },
    options: cfg.options
});
```
