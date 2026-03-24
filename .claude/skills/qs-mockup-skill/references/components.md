# QuickSight 组件规范

本文档定义所有 QuickSight 原型中可用的组件及其生成规范。

---

## 一、Controls 组件

### 1.1 基本结构
Controls 区域填充至模板中的 `<!-- CONTROLS_PLACEHOLDER -->`，必须支持展开/收起功能。

**重要：多Tab场景**
- 当页面包含多个Tab且每个Tab都有Controls时，必须为每个Tab的Controls使用唯一ID
- 示例：Tab1使用 `controls-grid`，Tab2使用 `controls-grid-price`，Tab3使用 `controls-grid-inventory`
- 同时需要为每个Tab创建独立的toggle函数（如 `toggleControlsPrice()`）

### 1.2 组件类型

**下拉框类型：**
```html
<div class="control-item">
    <label>字段名</label>
    <select>
        <option>All</option>
    </select>
</div>
```

**日期类型**（字段名含 "Date" 字样）：
```html
<div class="control-item">
    <label>字段名</label>
    <input type="date">
    <div class="control-hint">YYYY-MM-DD</div>
</div>
```

### 1.3 预设字段下拉选项

| 字段名              | 默认值                     | 下拉选项                                               |
|:-----------------|:------------------------|:---------------------------------------------------|
| Gender           | All                     | Female, Male, Unisex, Kids                         |
| Category         | All                     | Clothing, Shoes, Bag, Home, Beauty                 |
| Conditions       | All                     | New, Pre-owned                                     |
| Country          | US                      | US, CA, AU, CN                                     |
| Comparison Type  | Competitor AVG          | Competitor MAX, Competitor MIN                     |
| Comparison Scope | Across Competitor Store | Across All Stores                                  |
| Brand            | All                     | GUCCI, PRADA, Belenciaga, Valentino, Burberry, ... |

### 1.4 展开/收起实现
- 默认保持展开状态
- 用户可以通过点击 Header 触发模板中已有的折叠 JS
- 详细实现参考 `optimization_guide.md` 第一章节

---

## 二、组件标题规范

### 2.1 适用范围
所有组件（表格、图表）默认需要添加标题和说明结构，**KPI、Text Box、Section Divider 除外**。

### 2.2 标题结构
```html
<div class="chart-header">
    <div class="chart-title">标题</div>
    <div class="chart-description">说明</div>
</div>
```

- **标题**：16px 黑色加粗字体，如果用户未指定标题，显示"图表标题待定义"
- **说明**：12px 灰色字体，仅在用户明确提供说明时显示，超出宽度自动折行

---

## 三、基础组件

### 3.1 Text Box（文本块）
```html
<div class="visual-card text-box">内容</div>
```
**注意**：不需要标题结构

### 3.2 Section Divider（分隔标题）
```html
<div class="visual-card section-divider">标题</div>
```
**注意**：不需要标题结构

### 3.3 KPI 卡片
KPI 卡片的详细规范请参考：`#[[file:charts/kpi-card.md]]`

---

## 四、图表占位符

当不需要生成真实图表数据时，使用占位符：

```html
<div class="visual-card">
    <div class="chart-header">
        <div class="chart-title">图表标题</div>
    </div>
    <div class="chart-placeholder" style="height: 300px;">图表名称</div>
</div>
```

---

## 五、数据表格

数据表格的详细规范请参考：`#[[file:charts/table.md]]`
