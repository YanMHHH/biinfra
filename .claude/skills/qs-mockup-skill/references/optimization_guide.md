# QuickSight 原型生成补充规范

本文档提供一些补充性的实现细节和最佳实践。

---

## 一、Controls 展开/收起实现细节

### 1.1 HTML 结构
```html
<div class="controls-header" onclick="toggleControls()">
    <span>Controls</span>
    <span class="controls-icon">▲</span>
</div>
<div class="controls-grid" id="controlsGrid">
    <!-- controls items -->
</div>
```

### 1.2 CSS 样式
```css
.controls-icon { transition: transform 0.3s ease; }
.controls-icon.collapsed { transform: rotate(180deg); }
.controls-grid.collapsed { max-height: 0; opacity: 0; }
```

### 1.3 JavaScript 函数
```javascript
function toggleControls() {
    document.getElementById('controlsGrid').classList.toggle('collapsed');
    document.querySelector('.controls-icon').classList.toggle('collapsed');
}
```

---

## 二、生成检查清单

生成 QuickSight 原型时，必须确保：

- [ ] Controls 区域有展开/收起功能
- [ ] 图表高度为宽度的 0.9 倍
- [ ] Legend 图标 12px，字体 11px
- [ ] 日期格式为 YYYY-MM-DD
- [ ] 金额类指标添加 $ 前缀
- [ ] 百分比指标添加 % 后缀
- [ ] 每个图表独立在 visual-card 中
- [ ] Section 无整体标题
- [ ] 特殊宽度图表使用 flex 控制
- [ ] 表格生成 10-15 行数据
- [ ] 表格调用 makeColumnsResizable() 函数
