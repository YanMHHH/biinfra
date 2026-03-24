# 横向堆积条形图 (Horizontal Stacked Bar) 规范

本文档定义 QuickSight 原型中横向堆积条形图的生成规范。

---

## 一、设计要求

### 1.1 图表布局
- 使用横向堆积条形图（`indexAxis: 'y'`）
- 数据按总量从高到低排序
- 图表容器高度固定为 400px
- **推荐使用自定义滚动条**：当数据条目较多时（20+），使用自定义缩放滚动条替代传统滚动条

### 1.2 X轴配置
- 显示整数刻度，不显示小数
- 步长根据数据量级自动调整：
  - 百级别：步长 100 或 200
  - 千级别：步长 500 或 1000
- 无需大面积留白，最大值设置为数据总量的 1.05 倍（可选）

### 1.3 颜色方案
- Priced Highest（最高价）：`#d13212` 红色
- Priced Average（平均价）：`#879596` 灰色
- Priced Lowest（最低价）：`#1d8102` 绿色

---

## 二、HTML 结构

### 2.1 数据较少（6-10条）- 标准布局
```html
<div class="col">
    <div class="visual-card">
        <div class="chart-title">图表标题</div>
        <div style="height: 400px;">
            <canvas id="chartId"></canvas>
        </div>
    </div>
</div>
```

### 2.2 数据较多（20-30条）- 自定义滚动条（推荐）
```html
<div class="col">
    <div class="visual-card">
        <div class="chart-title">图表标题</div>
        <div style="position: relative; height: 400px; padding-right: 16px;" id="chartWrapper">
            <canvas id="chartId"></canvas>
            <div id="customScrollTrack" style="position: absolute; top: 0; right: 0; width: 10px; height: 100%; background: #f1f1f1; border-radius: 5px; box-shadow: inset 0 0 2px rgba(0,0,0,0.1);">
                <div id="customScrollThumb" style="position: absolute; top: 0; left: 0; width: 100%; height: 50%; background: #c1c1c1; border-radius: 5px; cursor: grab; display: flex; flex-direction: column; justify-content: space-between; transition: background 0.2s;">
                    <div id="customScrollTopHandle" style="height: 8px; width: 100%; cursor: ns-resize; display: flex; justify-content: center; align-items: center;">
                        <div style="width: 6px; height: 2px; background: #fff; border-radius: 1px;"></div>
                    </div>
                    <div id="customScrollBottomHandle" style="height: 8px; width: 100%; cursor: ns-resize; display: flex; justify-content: center; align-items: center;">
                        <div style="width: 6px; height: 2px; background: #fff; border-radius: 1px;"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

---

## 三、JavaScript 配置

### 3.1 步长计算函数
```javascript
const getStepSize = (max) => {
    const magnitude = Math.pow(10, Math.floor(Math.log10(max)));
    const normalized = max / magnitude;
    if (normalized <= 2) return magnitude / 2;
    if (normalized <= 5) return magnitude;
    return magnitude * 2;
};
```

### 3.2 数据准备（按总量排序）
```javascript
const chartData = [
    { name: '类目1', highest: 145, average: 280, lowest: 35 },
    { name: '类目2', highest: 132, average: 265, lowest: 32 },
    // ... 更多数据
].map(d => ({ ...d, total: d.highest + d.average + d.lowest }))
  .sort((a, b) => b.total - a.total);

const maxValue = Math.max(...chartData.map(d => d.total));
const stepSize = getStepSize(maxValue);
```

### 3.3 Chart.js 基础配置
```javascript
const chartInstance = new Chart(document.getElementById('chartId'), {
    type: 'bar',
    data: {
        labels: chartData.map(d => d.name),
        datasets: [
            { label: 'Priced Highest', data: chartData.map(d => d.highest), backgroundColor: '#d13212' },
            { label: 'Priced Average', data: chartData.map(d => d.average), backgroundColor: '#879596' },
            { label: 'Priced Lowest', data: chartData.map(d => d.lowest), backgroundColor: '#1d8102' }
        ]
    },
    options: {
        indexAxis: 'y',
        responsive: true,
        maintainAspectRatio: false,
        plugins: { legend: { position: 'top', labels: { boxWidth: 12, font: { size: 11 }}}},
        scales: {
            x: { stacked: true, beginAtZero: true, ticks: { stepSize: stepSize }},
            y: { stacked: true }
        }
    }
});
```

---

## 四、自定义滚动条交互（数据较多时推荐）

### 4.1 交互功能
- **拖拽平移（Pan）**：按住滑块中间区域拖动，平移查看不同数据
- **拖拽缩放（Zoom）**：按住顶部/底部手柄拖动，改变显示的数据条目数量
- **鼠标滚轮**：在图表区域滚轮滚动进行平移

### 4.2 完整交互代码
```javascript
const track = document.getElementById('customScrollTrack');
const thumb = document.getElementById('customScrollThumb');
const topHandle = document.getElementById('customScrollTopHandle');
const bottomHandle = document.getElementById('customScrollBottomHandle');
const wrapper = document.getElementById('chartWrapper');

if (track && thumb && topHandle && bottomHandle) {
    let totalItems = chartData.length;
    let viewStart = 0;
    let viewEnd = 9;
    let isDragging = false;
    let dragType = '';
    let startY = 0;
    let initialStart = 0;
    let initialEnd = 0;

    function updateChartAndView() {
        if (viewStart < 0) viewStart = 0;
        if (viewEnd >= totalItems) viewEnd = totalItems - 1;
        if (viewStart > viewEnd - 1) viewStart = viewEnd - 1;

        chartInstance.options.scales.y.min = viewStart;
        chartInstance.options.scales.y.max = viewEnd;
        chartInstance.update();

        const trackHeight = track.clientHeight;
        const thumbTop = (viewStart / totalItems) * trackHeight;
        const thumbHeight = ((viewEnd - viewStart + 1) / totalItems) * trackHeight;

        thumb.style.top = \`\${thumbTop}px\`;
        thumb.style.height = \`\${thumbHeight}px\`;
    }

    updateChartAndView();

    function onMouseDown(e, type) {
        e.preventDefault();
        e.stopPropagation();
        isDragging = true;
        dragType = type;
        startY = e.clientY;
        initialStart = viewStart;
        initialEnd = viewEnd;
        document.addEventListener('mousemove', onMouseMove);
        document.addEventListener('mouseup', onMouseUp);
    }

    thumb.addEventListener('mousedown', (e) => onMouseDown(e, 'pan'));
    topHandle.addEventListener('mousedown', (e) => onMouseDown(e, 'zoomTop'));
    bottomHandle.addEventListener('mousedown', (e) => onMouseDown(e, 'zoomBottom'));

    function onMouseMove(e) {
        if (!isDragging) return;
        const deltaY = e.clientY - startY;
        const trackHeight = track.clientHeight;
        const deltaItems = (deltaY / trackHeight) * totalItems;

        if (dragType === 'pan') {
            const shift = Math.round(deltaItems);
            let newStart = initialStart + shift;
            let newEnd = initialEnd + shift;

            if (newStart < 0) {
                newStart = 0;
                newEnd = initialEnd - initialStart;
            }
            if (newEnd >= totalItems) {
                newEnd = totalItems - 1;
                newStart = totalItems - 1 - (initialEnd - initialStart);
            }
            viewStart = newStart;
            viewEnd = newEnd;
        } else if (dragType === 'zoomTop') {
            viewStart = Math.round(initialStart + deltaItems);
            if (viewStart > viewEnd - 1) viewStart = viewEnd - 1;
            if (viewStart < 0) viewStart = 0;
        } else if (dragType === 'zoomBottom') {
            viewEnd = Math.round(initialEnd + deltaItems);
            if (viewEnd < viewStart + 1) viewEnd = viewStart + 1;
            if (viewEnd >= totalItems) viewEnd = totalItems - 1;
        }
        updateChartAndView();
    }

    function onMouseUp() {
        isDragging = false;
        document.removeEventListener('mousemove', onMouseMove);
        document.removeEventListener('mouseup', onMouseUp);
    }

    wrapper.addEventListener('wheel', (e) => {
        e.preventDefault();
        const shift = e.deltaY > 0 ? 1 : -1;
        if (viewStart + shift >= 0 && viewEnd + shift < totalItems) {
            viewStart += shift;
            viewEnd += shift;
            updateChartAndView();
        }
    });
}
```

---

## 五、使用场景

### 5.1 场景1：数据较少（6-10条）
- 容器高度：400px
- Canvas 不设置固定高度
- maintainAspectRatio: true
- aspectRatio: 1.5

### 5.2 场景2：数据较多（20-30条）- 推荐自定义滚动条
- 容器高度：400px，使用相对定位
- 添加自定义滚动条组件
- maintainAspectRatio: false
- 支持拖拽缩放和平移交互

---

## 六、注意事项

1. 数据必须先计算总量，再按总量降序排序
2. X轴步长使用 getStepSize 函数自动计算
3. 颜色方案保持一致：红色（高）、灰色（中）、绿色（低）
4. 图表高度统一为 400px，保持视觉一致性
5. 自定义滚动条需要将Chart实例保存为变量以支持动态更新
```
