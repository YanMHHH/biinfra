# Dual Metric KPI Card

## Overview
A KPI card displaying two metrics side by side: current period value and comparison period value with growth percentage.

## HTML Structure

```html
<div class="col" style="flex: 0 0 33.33%; max-width: 33.33%;">
    <div class="visual-card" style="padding: 20px; display: flex; flex-direction: column; justify-content: space-between; height: 100%;">
        <div style="font-size: 15px; font-weight: bold; color: var(--qs-text-main); margin-bottom: 16px;">Sales</div>
        <div style="text-align: center; margin-bottom: 20px;">
            <div style="font-size: 32px; font-weight: bold; color: var(--qs-text-main);" id="mainSalesValue">$3,847,290</div>
        </div>
        <div>
            <div style="font-size: 12px; color: var(--qs-text-muted); margin-bottom: 6px;">Compare to (Sales)</div>
            <div style="display: flex; justify-content: space-between; align-items: center;">
                <div style="font-size: 18px; font-weight: bold; color: var(--qs-text-main);" id="compareSalesValue">$3,124,560</div>
                <div style="font-size: 18px; font-weight: bold; color: #1d8102;" id="salesChangePercent">+23.1%</div>
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
