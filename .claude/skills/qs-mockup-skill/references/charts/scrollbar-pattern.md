# 自定义滚动条实现模式

## 问题

当使用 `new Chart(...)` 直接创建图表实例而不保存为变量时，后续的滚动条交互代码无法引用该实例，导致 `chartInstance.update()` 失败，滚动条无法工作。

## 解决方案

### ✅ 正确模式

```javascript
// 1. 保存 Chart 实例为变量
const chartInstance = new Chart(document.getElementById('myChart'), {
    type: 'bar',
    data: { /* ... */ },
    options: { /* ... */ }
});

// 2. 获取滚动条 DOM 元素
const track = document.getElementById('customScrollTrack');
const thumb = document.getElementById('customScrollThumb');
const topHandle = document.getElementById('customScrollTopHandle');
const bottomHandle = document.getElementById('customScrollBottomHandle');

// 3. 初始化滚动条交互
if (track && thumb && topHandle && bottomHandle) {
    let viewStart = 0;
    let viewEnd = 9;

    function updateChartAndView() {
        // 关键：使用保存的 chartInstance 更新图表
        chartInstance.options.scales.y.min = viewStart;
        chartInstance.options.scales.y.max = viewEnd;
        chartInstance.update();

        // 更新滚动条位置
        const trackHeight = track.clientHeight;
        thumb.style.top = `${(viewStart / totalItems) * trackHeight}px`;
        thumb.style.height = `${((viewEnd - viewStart + 1) / totalItems) * trackHeight}px`;
    }

    // 绑定事件...
}
```

### ❌ 错误模式

```javascript
// 错误：没有保存实例
new Chart(document.getElementById('myChart'), {
    type: 'bar',
    data: { /* ... */ },
    options: { /* ... */ }
});

// 后续代码无法引用 chartInstance
// Chart.instances.find() 在 DOMContentLoaded 中可能返回 undefined
let chartInstance = Chart.instances.find(c => c.canvas.id === 'myChart');
if (!chartInstance) return; // 滚动条初始化失败
```

## 实现步骤

1. **创建 Chart 时立即保存为变量**
   ```javascript
   const myChartInstance = new Chart(...);
   ```

2. **在滚动条初始化前验证 DOM 元素**
   ```javascript
   if (track && thumb && topHandle && bottomHandle) {
       // 初始化滚动条
   }
   ```

3. **在 updateChartAndView 中调用 chartInstance.update()**
   ```javascript
   chartInstance.options.scales.y.min = viewStart;
   chartInstance.options.scales.y.max = viewEnd;
   chartInstance.update();
   ```

4. **绑定鼠标和滚轮事件**
   - thumb.addEventListener('mousedown', ...)
   - topHandle.addEventListener('mousedown', ...)
   - bottomHandle.addEventListener('mousedown', ...)
   - wrapper.addEventListener('wheel', ...)

## 检查清单

- [ ] Chart 实例保存为 `const` 变量
- [ ] 滚动条初始化在 Chart 创建后
- [ ] 所有 DOM 元素存在检查
- [ ] updateChartAndView 中调用 chartInstance.update()
- [ ] 边界值检查：viewStart >= 0，viewEnd < totalItems
- [ ] 鼠标事件正确绑定
- [ ] 滚轮事件添加 preventDefault()
