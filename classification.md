---
title: 'Supervised Learning in R: Classification'
author: "Andi"
Last updated: "30 May, 2021"
output: 
  html_document: 
    keep_md: yes
---




```r
# {r, echo = FALSE, results='hide'}
# if we used both 'echo=TRUE' and 'results=hide' the pipe would not work properly
# if we used 'echo = FALSE' and 'results=hide' we would have only messages (i.e. attaching package) If we don't want them we set 'error = FALSE', 'warning = FALSE', and 'message = FALSE'.
```

Supervised learning: train a machine to learn from prior examples. When the concept to be learned.

## Classification with Nearest Neighbors


```r
# Load the 'class' package
library(class)

# load datasets
signs<-read.csv("signs.csv")
next_sign<-read.csv("next_sign.csv")

# Create a vector of labels
sign_types <- signs$sign_type

# Classify the next sign observed
knn(train = signs[-1], test = next_sign, cl = sign_types)
```

```
## [1] stop
## Levels: pedestrian speed stop
```
