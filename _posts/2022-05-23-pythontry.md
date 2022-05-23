---
title: "[Python] 코드 시도"
categories:
  - Python
tags:
  - machineLearning
  - statistics
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

```python
# RandomForestRegressor에 대한 최적의 하이퍼파라미터 조합 탐색
from sklearn.model_selection import GridSearchCV

param_grid = [
              {'n_estimators' : [3, 10, 30],
               'max_features' : [2, 4, 6, 8],
               'bootstrap' : [False],
               'n_estimators' : [3, 10],
               'max_features' : [2, 3, 4]},
]
forest_reg = RandomForestRegressor()

grid_search = GridSearchCV(forest_reg, param_grid, cv = 5, scoring = 'neg_mean_squared_error', return_train_score = True)
grid_search.fit(housing_prepared, housing_labels)
```

```sql
select * from city where CountryCode = 'KOR';
```
