# 数据表格 (Table) 规范

本文档定义 QuickSight 原型中数据表格的生成规范。

---

## 一、完整结构

```html
<div class="visual-card">
    <div class="chart-header">
        <div class="chart-title">表格标题</div>
        <div class="chart-description">表格说明（可选）</div>
    </div>
    <div class="table-scroll-container">
        <table class="qs-table">
            <thead>
                <tr><th>列1</th><th>列2</th></tr>
            </thead>
            <tbody>
                <tr><td>数据1</td><td>数据2</td></tr>
            </tbody>
        </table>
    </div>
</div>
```

---

## 二、核心规范

### 2.1 容器要求
- **必须包裹容器**：所有表格必须嵌套在 `<div class="table-scroll-container">` 内
- 激活 400px 限高和双向滚动逻辑

### 2.2 表头规则
- 表头（th）完整显示不省略
- 超出宽度时自动换行（`word-break: keep-all` 保持单词完整）
- 最小宽度 80px

### 2.3 单元格规则
- 数据单元格（td）使用 `text-overflow: ellipsis` 截断显示
- 超出宽度的文本显示为 `...`

### 2.4 滚动与布局
- 表格不使用 `table-layout: fixed`，宽度由内容决定
- 容器自动出现横向滚动条
- 纵向滚动时表头固定（Sticky Header）
- **CSS 要求**：`table.qs-table th` 和 `table.qs-table td` 必须设置 `position: sticky`
  - `th` 需要 `top: 0; z-index: 10;` 保持表头固定
  - `td` 需要 `position: sticky` 支持横向滚动时列固定

### 2.5 列宽调整
- 模板已内置列宽拖拽功能
- 拖拽时只调整当前列宽度
- 生成表格后需调用 `makeColumnsResizable()` 函数

### 2.6 Mock 数据量
- 必须生成 10-15 行合理的业务数据
- 确保纵向滚动条能够出现
- 展示 Sticky Header (固定表头) 效果

---

## 三、图片列处理

### 3.1 触发条件
当字段名含"图片/Image/Icon/头像"时，执行商品图片自动匹配逻辑。

### 3.2 匹配规则表

| 逻辑条件 (关键字匹配) | 对应图片路径 | Class |
|:---|:---|:---|
| 包含 "箱包"、"包"、"手提袋" | `assets/bag.png` | `qs-product-img` |
| 包含 "裙子"、"女装"、"连衣裙" | `assets/skirt.png` | `qs-product-img` |
| 包含 "T恤"、"男装"、"短袖"、"上衣" | `assets/tshirt.png` | `qs-product-img` |
| 包含 "鞋"、"运动鞋"、"球鞋" | `assets/shoes.png` | `qs-product-img` |
| 包含 "手表"、"腕表" | `assets/watch.png` | `qs-product-img` |
| 包含 "电子"、"手机"、"数码" | `assets/electronics.png` | `qs-product-img` |
| 未指定品类 或 不匹配上述条件 | 从上述路径中随机选择一个 | `qs-product-img` |

### 3.3 生成代码示例
```html
<img src="assets/bag.png" class="qs-product-img" alt="product">
```

### 3.4 降级方案
如果 assets 文件夹中没有对应图片：

**方案1：占位符**
```html
<div class="qs-img-placeholder">IMG</div>
```

**方案2：在线图片服务**
```html
<img src="https://picsum.photos/40/40?random=N" class="qs-product-img" alt="product">
```

---

## 四、完整示例

```html
<div class="visual-card">
    <div class="chart-header">
        <div class="chart-title">热销商品榜单</div>
        <div class="chart-description">按销量降序排列，数据更新至今日</div>
    </div>
    <div class="table-scroll-container">
        <table class="qs-table">
            <thead>
                <tr>
                    <th>商品图片</th>
                    <th>分类</th>
                    <th>商品名称</th>
                    <th>销量</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><img src="assets/bag.png" class="qs-product-img" alt="product"></td>
                    <td>箱包</td>
                    <td>经典款真皮手提包 - 适合商务通勤使用</td>
                    <td>1,234</td>
                </tr>
                <!-- 更多数据行... -->
            </tbody>
        </table>
    </div>
</div>

<script>
// 初始化列宽拖拽功能
makeColumnsResizable();
</script>
```
