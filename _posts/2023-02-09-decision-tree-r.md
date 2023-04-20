---
title: "[R] 3. Decision Tree with Confusion Matrix"
categories:
  - R
  - DecisionTree
  - CART
tags:
  - Decision Tree
  - R
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

# Decision Tree Practice with R

## Performance Evaluation Function 1

```r
perf_eval <- function(cm){
  
  # True positive rate: TPR (Recall)
  TPR <- cm[2,2]/sum(cm[2,])
  # Precision
  PRE <- cm[2,2]/sum(cm[,2])
  # True negative rate: TNR
  TNR <- cm[1,1]/sum(cm[1,])
  # Simple Accuracy
  ACC <- (cm[1,1]+cm[2,2])/sum(cm)
  # Balanced Correction Rate
  BCR <- sqrt(TPR*TNR)
  # F1-Measure
  F1 <- 2*TPR*PRE/(TPR+PRE)
  
  return(c(TPR, PRE, TNR, ACC, BCR, F1))
}

# Performance table
Perf_Table <- matrix(0, nrow = 2, ncol = 6)
rownames(Perf_Table) <- c("Post-Pruning", "Pre-Pruning")
colnames(Perf_Table) <- c("TPR", "Precision", "TNR", "Accuracy", "BCR", "F1-Measure")
Perf_Table
```

## Dataset 1. Personal Loan Dataset

<img width="1184" alt="스크린샷 2023-02-09 16 23 29" src="https://user-images.githubusercontent.com/86525868/217745094-943166ef-8642-4544-aeb6-092b03a770d8.png">

* 총 14개의 변수 
* 2개의 불필요한 변수 : `ID, ZIP`
* 종속변수 : `Personal Loan` → `int`이기 때문에 `factor`로 변형

```r
# Load the data & Preprocessing
Ploan <- read.csv("Personal Loan.csv")
input_idx <- c(2,3,4,6,7,8,9,11,12,13,14)
target_idx <- 10

Ploan_input <- Ploan[,input_idx]
Ploan_target <- as.factor(Ploan[,target_idx])

trn_idx <- 1:1500

CART_trn <- data.frame(Ploan_input[trn_idx,], PloanYN = Ploan_target[trn_idx])
CART_tst <- data.frame(Ploan_input[-trn_idx,], PloanYN = Ploan_target[-trn_idx])
```

### CART with Post-Pruning

```r
install.packages("tree")
library(tree)

# Training the tree
CART_post <- tree(PloanYN ~ ., CART_trn)
summary(CART_post)

# Plot the tree
plot(CART_post)
text(CART_post, pretty = 1)

# Find the best tree
set.seed(123)
CART_post_cv <- cv.tree(CART_post, FUN = prune.misclass)

# Plot the pruning result
plot(CART_post_cv$size, CART_post_cv$dev, type = "b")
CART_post_cv

# Select the final model
CART_post_pruned <- prune.misclass(CART_post, best = 6)
plot(CART_post_pruned)
text(CART_post_pruned, pretty = 1)
```

![image](https://user-images.githubusercontent.com/86525868/217746368-f8b3854f-f9c4-4456-8cbe-f4e868f87a8a.png)

* 대출 받은 고객이 왜 대출을 받았을까? 란 질문에 대한 답을 할 수 있다 
  * `Income`은 98.5 보다 작고 `CCAvg, 신용카드평균사용량`도 2.95 보다 작고 `CDAccount`가 있는 고객은 대출을 받음  

```r
# Prediction
CART_post_prey <- predict(CART_post_pruned, CART_tst, type = "class")
CART_post_cm <- table(CART_tst$PloanYN, CART_post_prey)
CART_post_cm

Perf_Table[1,] <- perf_eval(CART_post_cm)
Perf_Table

# CART with Pre-Pruning -------------------------------
# For CART
install.packages("matrixStats")
install.packages("party")
library(party)

# For AUROC
install.packages("ROCR")
library(ROCR)

# Divide the dataset into training/validation/test datasets
trn_idx <- 1:1000
val_idx <- 1001:1500
tst_idx <- 1501:2500

CART_trn <- data.frame(Ploan_input[trn_idx,], PloanYN = Ploan_target[trn_idx])
CART_val <- data.frame(Ploan_input[val_idx,], PloanYN = Ploan_target[val_idx])
CART_tst <- data.frame(Ploan_input[tst_idx,], PloanYN = Ploan_target[tst_idx])

# Construct single tree and evaluation
# tree parameter settings
min_criterion = c(0.9, 0.95, 0.99)
min_split = c(10, 30, 50, 100)
max_depth = c(0, 10, 5)
CART_pre_search_result = matrix(0,length(min_criterion)*length(min_split)*length(max_depth),11)
colnames(CART_pre_search_result) <- c("min_criterion", "min_split", "max_depth", 
                                      "TPR", "Precision", "TNR", "ACC", "BCR", "F1", "AUROC", "N_leaves")

iter_cnt = 1

for (i in 1:length(min_criterion)){
  for ( j in 1:length(min_split)){
    for ( k in 1:length(max_depth)){
      
      cat("CART Min criterion:", min_criterion[i], ", Min split:", min_split[j], ", Max depth:", max_depth[k], "\n")
      
      tmp_control = ctree_control(mincriterion = min_criterion[i], minsplit = min_split[j], maxdepth = max_depth[k])
      tmp_tree <- ctree(PloanYN ~ ., data = CART_trn, controls = tmp_control)
      tmp_tree_val_prediction <- predict(tmp_tree, newdata = CART_val)
      
      tmp_tree_val_response <- treeresponse(tmp_tree, newdata = CART_val)
      tmp_tree_val_prob <- 1-unlist(tmp_tree_val_response, use.names=F)[seq(1,nrow(CART_val)*2,2)]
      tmp_tree_val_rocr <- prediction(tmp_tree_val_prob, CART_val$PloanYN)
      
      # Confusion matrix for the validation dataset
      tmp_tree_val_cm <- table(CART_val$PloanYN, tmp_tree_val_prediction)
      
      # parameters
      CART_pre_search_result[iter_cnt,1] = min_criterion[i]
      CART_pre_search_result[iter_cnt,2] = min_split[j]
      CART_pre_search_result[iter_cnt,3] = max_depth[k]
      # Performances from the confusion matrix
      CART_pre_search_result[iter_cnt,4:9] = perf_eval(tmp_tree_val_cm)
      # AUROC
      CART_pre_search_result[iter_cnt,10] = unlist(performance(tmp_tree_val_rocr, "auc")@y.values)
      # Number of leaf nodes
      CART_pre_search_result[iter_cnt,11] = length(nodes(tmp_tree, unique(where(tmp_tree))))
      iter_cnt = iter_cnt + 1
    }
  }
}

# Find the best set of parameters
CART_pre_search_result <- CART_pre_search_result[order(CART_pre_search_result[,10], decreasing = T),]
CART_pre_search_result
best_criterion <- CART_pre_search_result[1,1]
best_split <- CART_pre_search_result[1,2]
best_depth <- CART_pre_search_result[1,3]

# Construct the best tree
tree_control = ctree_control(mincriterion = best_criterion, minsplit = best_split, maxdepth = best_depth)

# Use the training and validation dataset to train the best tree
CART_trn <- rbind(CART_trn, CART_val)

CART_pre <- ctree(PloanYN ~ ., data = CART_trn, controls = tree_control)
CART_pre_prediction <- predict(CART_pre, newdata = CART_tst)
CART_pre_response <- treeresponse(CART_pre, newdata = CART_tst)

# Performance of the best tree
CART_pre_cm <- table(CART_tst$PloanYN, CART_pre_prediction)
CART_pre_cm

Perf_Table[2,] <- perf_eval(CART_pre_cm)
Perf_Table

# Plot the ROC
CART_pre_prob <- 1-unlist(CART_pre_response, use.names=F)[seq(1,nrow(CART_tst)*2,2)]
CART_pre_rocr <- prediction(CART_pre_prob, CART_tst$PloanYN)
CART_pre_perf <- performance(CART_pre_rocr, "tpr","fpr") 
plot(CART_pre_perf, col=5, lwd = 3)

# Plot the best tree
plot(CART_pre)
plot(CART_pre, type="simple")


```
