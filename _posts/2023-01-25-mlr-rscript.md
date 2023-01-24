---
title: "[R] Multiple Linear Regression"
categories:
  - R
  - R studio
  - Multiple Linear Regression
tags:
  - Multiple Linear Regression
  - R
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

# Multiple Linear Regression Practice with R
``` r
# Performance evaluation function for regression 
perf_eval_reg <- function(tgt_y, pre_y){
  #RMSE
  rmse <- sqrt(mean((tgt_y - pre_y)^2))
  #MAE
  mae <- mean(abs(tgt_y - pre_y))
  #MAPE
  mape <- 100* mean(abs((tgt_y - pre_y)/tgt_y))
  
  return(c(rmse, mae, mape))
}

# Initialize a performance summary 
perf_mat <- matrix(0, nrow = 2, ncol = 3)
rownames(perf_mat) <- c("Toyota Coroola", "Boston Housing")
colnames(perf_mat) <- c("RMSE", "MAE", "MAPE")
perf_mat
```
