---
title: "Introduction to Data Science"
description: "Data Science의 기본 개념, 워크플로우, 그리고 주요 도구를 소개합니다"
order: 1
---


# Chapter 1: Introduction to Data Science

## What is Data Science?

Data Science는 **데이터로부터 의미 있는 인사이트를 추출**하는 학제간 분야입니다. 통계학, 컴퓨터 과학, 도메인 지식이 결합된 영역으로, 대규모 데이터셋에서 패턴을 발견하고 의사결정에 활용합니다.

> "Data is the new oil, but like oil, it must be refined to be useful."

## The Data Science Workflow

데이터 사이언스 프로젝트는 일반적으로 다음 단계를 따릅니다:

```
1. Problem Definition → 2. Data Collection → 3. Data Cleaning
         ↑                                          ↓
    6. Deployment    ←  5. Evaluation  ←  4. Modeling
```

### 1. Problem Definition (문제 정의)
비즈니스 문제를 데이터 문제로 변환하는 단계입니다.

- **분류(Classification)**: 이메일이 스팸인지 아닌지?
- **회귀(Regression)**: 내일 주가는 얼마일까?
- **클러스터링(Clustering)**: 고객을 어떤 그룹으로 나눌 수 있을까?

### 2. Data Collection (데이터 수집)
데이터는 다양한 소스에서 수집할 수 있습니다:

| Source | Example | Format |
|--------|---------|--------|
| Database | PostgreSQL, MongoDB | Structured/Semi-structured |
| API | Twitter API, Weather API | JSON/XML |
| Web Scraping | BeautifulSoup, Scrapy | HTML → Structured |
| Files | CSV, Excel, Parquet | Tabular |

### 3. Data Cleaning (데이터 정제)
실제 데이터의 **60-80%** 시간이 이 단계에 소요됩니다.

```python
import pandas as pd

# Load data
df = pd.read_csv('data.csv')

# Check missing values
print(df.isnull().sum())

# Handle missing values
df['age'].fillna(df['age'].median(), inplace=True)

# Remove duplicates
df.drop_duplicates(inplace=True)

# Check data types
print(df.dtypes)
```

## Key Tools in Data Science

### Python Ecosystem

Python은 Data Science에서 가장 널리 사용되는 언어입니다:

```python
# Core libraries
import numpy as np       # Numerical computing
import pandas as pd      # Data manipulation
import matplotlib.pyplot as plt  # Visualization
import seaborn as sns    # Statistical visualization
from sklearn import *    # Machine Learning
```

### 라이브러리별 역할

**NumPy** - 수치 계산의 기반
```python
import numpy as np

# Array creation and operations
arr = np.array([1, 2, 3, 4, 5])
print(f"Mean: {arr.mean():.2f}")
print(f"Std:  {arr.std():.2f}")

# Matrix operations
matrix = np.random.randn(3, 3)
eigenvalues = np.linalg.eigvals(matrix)
```

**Pandas** - 데이터 조작의 핵심
```python
import pandas as pd

# DataFrame operations
df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'score': [85, 92, 78],
    'grade': ['B', 'A', 'C']
})

# Groupby and aggregation
summary = df.groupby('grade')['score'].agg(['mean', 'count'])
```

## Interactive Visualization Example

아래는 데이터 분포를 시각화한 예시입니다:

<div id="plot-distribution" style="width:100%;height:400px;"></div>
<script>
if (typeof Plotly !== 'undefined') {
    const x = Array.from({length: 1000}, () => {
        let u = 0, v = 0;
        while(u === 0) u = Math.random();
        while(v === 0) v = Math.random();
        return Math.sqrt(-2.0 * Math.log(u)) * Math.cos(2.0 * Math.PI * v);
    });
    Plotly.newPlot('plot-distribution', [{
        x: x,
        type: 'histogram',
        marker: { color: 'rgba(99, 110, 250, 0.7)' },
        nbinsx: 40
    }], {
        title: 'Normal Distribution (n=1000)',
        xaxis: { title: 'Value' },
        yaxis: { title: 'Frequency' },
        paper_bgcolor: 'rgba(0,0,0,0)',
        plot_bgcolor: 'rgba(0,0,0,0)',
        font: { color: '#e0e0e0' }
    }, {responsive: true});
}
</script>

## Summary

이 챕터에서 배운 핵심 개념:

1. Data Science는 통계학 + CS + 도메인 지식의 교차점
2. 표준 워크플로우: 문제 정의 → 수집 → 정제 → 모델링 → 평가 → 배포
3. Python이 핵심 도구 (NumPy, Pandas, Scikit-learn)
4. 데이터 정제가 전체 작업의 대부분을 차지

---
*다음 챕터: Exploratory Data Analysis (EDA)*
