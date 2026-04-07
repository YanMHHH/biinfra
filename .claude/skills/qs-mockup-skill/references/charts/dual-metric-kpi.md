# Dual Metric KPI Card

## Overview
A KPI card displaying two metrics side by side: current period value and comparison period value with growth percentage.

## CSS Styles (Embedded)

```css
.dual-metric-kpi {
    flex: 0 0 33.33%;
    max-width: 33.33%;
}

.dual-metric-kpi .visual-card {
    padding: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 180px;
    background-color: var(--qs-white);
    border: 1px solid var(--qs-border-dark);
    box-shadow: 0 1px 1px 0 rgba(0,28,36,.05);
}

.dual-metric-kpi .kpi-title {
    font-size: 15px;
    font-weight: bold;
    color: var(--qs-text-main);
    margin-bottom: 16px;
}

.dual-metric-kpi .main-value {
    text-align: center;
    margin-bottom: 20px;
    font-size: 32px;
    font-weight: bold;
    color: var(--qs-text-main);
}

.dual-metric-kpi .compare-section {
    display: flex;
    flex-direction: column;
}

.dual-metric-kpi .compare-label {
    font-size: 12px;
    color: var(--qs-text-muted);
    margin-bottom: 6px;
}

.dual-metric-kpi .compare-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.dual-metric-kpi .compare-value {
    font-size: 18px;
    font-weight: bold;
    color: var(--qs-text-main);
}

.dual-metric-kpi .change-percent {
    font-size: 18px;
    font-weight: bold;
}

.dual-metric-kpi .change-percent.positive {
    color: #1d8102;
}

.dual-metric-kpi .change-percent.negative {
    color: #d32f2f;
}
```

## HTML Structure

```html
<div class="dual-metric-kpi">
    <div class="visual-card">
        <div class="kpi-title">Sales</div>
        <div class="main-value" id="mainSalesValue">$3,847,290</div>
        <div class="compare-section">
            <div class="compare-label">Compare to (Sales)</div>
            <div class="compare-row">
                <div class="compare-value" id="compareSalesValue">$3,124,560</div>
                <div class="change-percent positive" id="salesChangePercent">+23.1%</div>
            </div>
        </div>
    </div>
</div>
```

## Layout Details

- **Width**: 33.33% (1/3 of section)
- **Title**: 15px, bold, top of card
- **Main Value**: 32px, bold, centered
- **Compare Label**: 12px, gray text
- **Compare Value**: 18px, bold, left aligned
- **Change Percent**: 18px, bold, right aligned
  - Green (#1d8102) for positive
  - Red (#d32f2f) for negative

## JavaScript Function

```javascript
function updateKPICard() {
    const mainSales = 3847290;
    const compareSales = 3124560;
    const changePercent = ((mainSales - compareSales) / compareSales * 100).toFixed(1);
    const changeColor = changePercent >= 0 ? '#1d8102' : '#d32f2f';
    const changeSign = changePercent >= 0 ? '+' : '';

    document.getElementById('mainSalesValue').textContent = '$' + mainSales.toLocaleString();
    document.getElementById('compareSalesValue').textContent = '$' + compareSales.toLocaleString();

    const changeElem = document.getElementById('salesChangePercent');
    changeElem.textContent = changeSign + changePercent + '%';
    changeElem.style.color = changeColor;
}

updateKPICard();
```

## Color Logic
- **Positive change (≥ 0%)**: Green (#1d8102)
- **Negative change (< 0%)**: Red (#d32f2f)
- The percentage element dynamically updates its color based on the calculated change value

## Usage
Include this card in Section 1 of tab1 (Sales Analysis) to display sales metrics with period comparison.
