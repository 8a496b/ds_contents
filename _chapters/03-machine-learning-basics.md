---
title: "Machine Learning Basics"
description: "머신러닝의 기본 개념, 주요 알고리즘, 그리고 모델 평가 방법을 학습합니다"
order: 3
---


# Chapter 3: Machine Learning Basics

## What is Machine Learning?

Machine Learning(ML)은 **명시적으로 프로그래밍하지 않고 데이터로부터 학습**하는 알고리즘을 연구하는 분야입니다.

> Arthur Samuel (1959): "Field of study that gives computers the ability to learn without being explicitly programmed."

## Types of Machine Learning

```
Machine Learning
├── Supervised Learning (지도학습)
│   ├── Classification (분류)
│   └── Regression (회귀)
├── Unsupervised Learning (비지도학습)
│   ├── Clustering (군집화)
│   └── Dimensionality Reduction (차원축소)
└── Reinforcement Learning (강화학습)
    └── Agent learns through rewards/penalties
```

### Supervised Learning

레이블(정답)이 있는 데이터로 학습합니다.

**Classification Example** - 이메일 스팸 분류:
```python
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import accuracy_score

# Split data
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Train model
model = MultinomialNB()
model.fit(X_train, y_train)

# Evaluate
y_pred = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, y_pred):.3f}")
```

**Regression Example** - 집값 예측:
```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

model = LinearRegression()
model.fit(X_train, y_train)

y_pred = model.predict(X_test)
print(f"RMSE: {mean_squared_error(y_test, y_pred, squared=False):.2f}")
print(f"R²:   {r2_score(y_test, y_pred):.3f}")
```

### Unsupervised Learning

레이블 없이 데이터의 구조를 발견합니다.

```python
from sklearn.cluster import KMeans

# K-Means clustering
kmeans = KMeans(n_clusters=3, random_state=42)
clusters = kmeans.fit_predict(X)

# Visualize
plt.scatter(X[:, 0], X[:, 1], c=clusters, cmap='viridis')
plt.scatter(kmeans.cluster_centers_[:, 0], kmeans.cluster_centers_[:, 1],
            marker='x', s=200, linewidths=3, color='red')
plt.title('K-Means Clustering')
plt.show()
```

## Key Algorithms Comparison

| Algorithm | Type | Pros | Cons | Use Case |
|-----------|------|------|------|----------|
| Linear Regression | Regression | Simple, interpretable | Linear only | Price prediction |
| Logistic Regression | Classification | Fast, probabilistic | Linear boundary | Binary classification |
| Decision Tree | Both | Interpretable | Overfitting | Feature importance |
| Random Forest | Both | Robust, accurate | Slow, black-box | General purpose |
| K-Nearest Neighbors | Both | Simple, no training | Slow prediction | Small datasets |
| SVM | Both | Effective in high-dim | Memory intensive | Text classification |
| Neural Network | Both | Any pattern | Needs lots of data | Image, NLP |

## The Bias-Variance Tradeoff

모델 복잡도를 선택할 때 가장 중요한 개념입니다:

$$\text{Total Error} = \text{Bias}^2 + \text{Variance} + \text{Irreducible Error}$$

- **High Bias (Underfitting)**: 모델이 너무 단순 → 훈련/테스트 모두 성능 낮음
- **High Variance (Overfitting)**: 모델이 너무 복잡 → 훈련은 좋지만 테스트 성능 낮음

<div id="plot-bias-variance" style="width:100%;height:400px;"></div>
<script>
if (typeof Plotly !== 'undefined') {
    const complexity = Array.from({length: 50}, (_, i) => i * 0.2 + 0.1);
    const bias2 = complexity.map(c => 5 * Math.exp(-0.5 * c) + 0.2);
    const variance = complexity.map(c => 0.1 * Math.exp(0.4 * c));
    const total = complexity.map((c, i) => bias2[i] + variance[i] + 0.5);

    Plotly.newPlot('plot-bias-variance', [
        {x: complexity, y: bias2, name: 'Bias²', line: {color: '#636EFA', width: 3}},
        {x: complexity, y: variance, name: 'Variance', line: {color: '#EF553B', width: 3}},
        {x: complexity, y: total, name: 'Total Error', line: {color: '#00CC96', width: 3, dash: 'dash'}},
        {x: complexity, y: Array(50).fill(0.5), name: 'Irreducible Error', line: {color: '#AB63FA', width: 2, dash: 'dot'}}
    ], {
        title: 'Bias-Variance Tradeoff',
        xaxis: {title: 'Model Complexity'},
        yaxis: {title: 'Error', range: [0, 8]},
        paper_bgcolor: 'rgba(0,0,0,0)', plot_bgcolor: 'rgba(0,0,0,0)',
        font: {color: '#e0e0e0'},
        legend: {x: 0.6, y: 0.95}
    }, {responsive: true});
}
</script>

## Model Evaluation

### Classification Metrics

```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score,
    f1_score, confusion_matrix, classification_report
)

# Confusion Matrix
#                 Predicted
#              |  Pos  |  Neg  |
# Actual  Pos  |  TP   |  FN   |
#         Neg  |  FP   |  TN   |

print(classification_report(y_test, y_pred))
```

| Metric | Formula | When to Use |
|--------|---------|-------------|
| Accuracy | $\frac{TP+TN}{TP+TN+FP+FN}$ | Balanced classes |
| Precision | $\frac{TP}{TP+FP}$ | Cost of FP is high (spam filter) |
| Recall | $\frac{TP}{TP+FN}$ | Cost of FN is high (disease detection) |
| F1-Score | $2 \cdot \frac{P \cdot R}{P + R}$ | Imbalanced classes |

### Cross-Validation

단일 train/test split의 한계를 극복합니다:

```python
from sklearn.model_selection import cross_val_score

# 5-Fold Cross-Validation
scores = cross_val_score(model, X, y, cv=5, scoring='accuracy')
print(f"CV Accuracy: {scores.mean():.3f} ± {scores.std():.3f}")
```

## Practical ML Pipeline

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier

# Build pipeline
pipeline = Pipeline([
    ('scaler', StandardScaler()),          # Step 1: Normalize features
    ('classifier', RandomForestClassifier( # Step 2: Train model
        n_estimators=100,
        max_depth=10,
        random_state=42
    ))
])

# Fit and predict
pipeline.fit(X_train, y_train)
y_pred = pipeline.predict(X_test)

# Evaluate
print(classification_report(y_test, y_pred))
```

## Hyperparameter Tuning

```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    'classifier__n_estimators': [50, 100, 200],
    'classifier__max_depth': [5, 10, 20, None],
    'classifier__min_samples_split': [2, 5, 10]
}

grid_search = GridSearchCV(
    pipeline, param_grid,
    cv=5, scoring='f1', n_jobs=-1
)
grid_search.fit(X_train, y_train)

print(f"Best params: {grid_search.best_params_}")
print(f"Best F1:     {grid_search.best_score_:.3f}")
```

## Summary

1. ML의 세 가지 유형: 지도학습, 비지도학습, 강화학습
2. **Bias-Variance Tradeoff**가 모델 선택의 핵심 원리
3. 문제에 맞는 **evaluation metric** 선택이 중요
4. **Cross-validation**으로 모델 성능을 안정적으로 추정
5. **Pipeline**으로 전처리와 모델을 깔끔하게 관리

---
*축하합니다! 기본 과정을 모두 완료했습니다.*
