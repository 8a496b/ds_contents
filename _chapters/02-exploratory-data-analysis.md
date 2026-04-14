---
title: "Exploratory Data Analysis (EDA)"
description: "데이터 탐색적 분석의 핵심 기법과 시각화 방법을 학습합니다"
order: 2
---


# Chapter 2: Exploratory Data Analysis (EDA)

## What is EDA?

EDA는 데이터를 **시각화하고 요약 통계를 통해 이해**하는 과정입니다. 모델링 전에 데이터의 구조, 패턴, 이상치를 파악하는 것이 목표입니다.

> John Tukey (1977): "The greatest value of a picture is when it forces us to notice what we never expected to see."

## Descriptive Statistics (기술 통계)

### Central Tendency (중심 경향)

| Measure | Formula | Use Case |
|---------|---------|----------|
| Mean (평균) | $\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i$ | Symmetric data |
| Median (중앙값) | Middle value when sorted | Skewed data |
| Mode (최빈값) | Most frequent value | Categorical data |

```python
import numpy as np
import pandas as pd

data = pd.Series([23, 25, 28, 28, 30, 32, 35, 42, 95])

print(f"Mean:   {data.mean():.1f}")   # 37.6 - influenced by outlier (95)
print(f"Median: {data.median():.1f}") # 30.0 - robust to outlier
print(f"Mode:   {data.mode()[0]}")    # 28   - most frequent
```

**핵심 포인트**: 이상치(outlier)가 있을 때는 **median**이 mean보다 더 적합한 대표값입니다.

### Dispersion (산포도)

```python
print(f"Variance: {data.var():.1f}")   # 분산
print(f"Std Dev:  {data.std():.1f}")   # 표준편차
print(f"IQR:      {data.quantile(0.75) - data.quantile(0.25):.1f}")  # 사분위 범위
print(f"Range:    {data.max() - data.min()}")  # 범위
```

## Visualization Techniques

### 1. Distribution (분포 시각화)

```python
import matplotlib.pyplot as plt
import seaborn as sns

fig, axes = plt.subplots(1, 3, figsize=(15, 4))

# Histogram
axes[0].hist(df['age'], bins=20, edgecolor='black')
axes[0].set_title('Histogram')

# KDE Plot
sns.kdeplot(df['age'], ax=axes[1], fill=True)
axes[1].set_title('KDE Plot')

# Box Plot
axes[2].boxplot(df['age'])
axes[2].set_title('Box Plot')

plt.tight_layout()
plt.show()
```

### 2. Relationship (관계 시각화)

**Scatter Plot**은 두 변수 간의 관계를 보여줍니다:

```python
# Scatter plot with regression line
sns.regplot(x='study_hours', y='exam_score', data=df)
plt.title('Study Hours vs Exam Score')
plt.show()
```

**Correlation Matrix**는 모든 변수 쌍의 상관관계를 한눈에 보여줍니다:

```python
# Correlation heatmap
corr = df[['age', 'income', 'spending', 'satisfaction']].corr()
sns.heatmap(corr, annot=True, cmap='coolwarm', center=0)
plt.title('Correlation Matrix')
plt.show()
```

### 3. Composition (구성 시각화)

```python
# Pie chart for categorical data
df['category'].value_counts().plot(kind='pie', autopct='%1.1f%%')
plt.title('Category Distribution')
plt.show()
```

## Interactive EDA Example

아래는 Anscombe's Quartet을 보여주는 인터랙티브 차트입니다. 네 데이터셋 모두 동일한 기술 통계를 가지지만 분포가 전혀 다릅니다:

<div id="plot-anscombe" style="width:100%;height:500px;"></div>
<script>
if (typeof Plotly !== 'undefined') {
    const datasets = {
        'Dataset I':   {x:[10,8,13,9,11,14,6,4,12,7,5],   y:[8.04,6.95,7.58,8.81,8.33,9.96,7.24,4.26,10.84,4.82,5.68]},
        'Dataset II':  {x:[10,8,13,9,11,14,6,4,12,7,5],   y:[9.14,8.14,8.74,8.77,9.26,8.1,6.13,3.1,9.13,7.26,4.74]},
        'Dataset III': {x:[10,8,13,9,11,14,6,4,12,7,5],   y:[7.46,6.77,12.74,7.11,7.81,8.84,6.08,5.39,8.15,6.42,5.73]},
        'Dataset IV':  {x:[8,8,8,8,8,8,8,19,8,8,8],       y:[6.58,5.76,7.71,8.84,8.47,7.04,5.25,12.5,5.56,7.91,6.89]}
    };
    const traces = Object.entries(datasets).map(([name, d], i) => ({
        x: d.x, y: d.y,
        mode: 'markers',
        type: 'scatter',
        name: name,
        xaxis: i < 2 ? (i === 0 ? 'x' : 'x2') : (i === 2 ? 'x3' : 'x4'),
        yaxis: i < 2 ? (i === 0 ? 'y' : 'y2') : (i === 2 ? 'y3' : 'y4'),
        marker: {size: 10}
    }));
    Plotly.newPlot('plot-anscombe', traces, {
        title: "Anscombe's Quartet (Mean≈9, Var≈11, r≈0.816)",
        grid: {rows: 2, columns: 2, pattern: 'independent'},
        paper_bgcolor: 'rgba(0,0,0,0)', plot_bgcolor: 'rgba(0,0,0,0)',
        font: {color: '#e0e0e0'},
        showlegend: false,
        xaxis:  {range:[2,20]}, yaxis:  {range:[2,14]},
        xaxis2: {range:[2,20]}, yaxis2: {range:[2,14]},
        xaxis3: {range:[2,20]}, yaxis3: {range:[2,14]},
        xaxis4: {range:[2,20]}, yaxis4: {range:[2,14]},
    }, {responsive: true});
}
</script>

## Practical EDA Checklist

실제 프로젝트에서 EDA를 수행할 때의 체크리스트:

### Step 1: Data Overview
```python
df.shape          # (rows, columns)
df.info()         # Data types, non-null counts
df.describe()     # Summary statistics
df.head(10)       # First 10 rows
```

### Step 2: Missing Values
```python
# Missing value percentage
missing = (df.isnull().sum() / len(df) * 100).sort_values(ascending=False)
print(missing[missing > 0])

# Visualize missing pattern
import missingno as msno
msno.matrix(df)
```

### Step 3: Outlier Detection
```python
# IQR method
Q1 = df['value'].quantile(0.25)
Q3 = df['value'].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

outliers = df[(df['value'] < lower_bound) | (df['value'] > upper_bound)]
print(f"Found {len(outliers)} outliers ({len(outliers)/len(df)*100:.1f}%)")
```

### Step 4: Feature Relationships
```python
# Pairplot for quick overview
sns.pairplot(df, hue='target', diag_kind='kde')
plt.show()
```

## Common EDA Pitfalls

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Correlation ≠ Causation | 두 변수가 상관되었다고 인과관계는 아님 | 도메인 지식으로 판단 |
| Survivorship Bias | 살아남은 데이터만 분석 | 누락된 데이터 고려 |
| Simpson's Paradox | 전체 트렌드와 그룹별 트렌드가 반대 | 항상 subgroup 분석 수행 |
| Over-aggregation | 과도한 집계로 패턴 소실 | 다양한 granularity로 분석 |

## Summary

1. EDA는 모델링 전 **필수** 단계 — 데이터를 이해해야 올바른 모델을 선택할 수 있다
2. 기술 통계만으로는 부족하다 (Anscombe's Quartet이 증명)
3. 항상 **시각화**와 **통계**를 병행하라
4. Missing values, outliers, feature relationships를 체계적으로 점검하라

---
*다음 챕터: Machine Learning Basics*
