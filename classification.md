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

## Recognizing a road sign with kNN


```r
# Load the 'class' package
library(class)

# load datasets
signs<-read.csv("signs.csv")
next_sign<-read.csv("next_sign.csv")

str(signs)
```

```
## 'data.frame':	146 obs. of  49 variables:
##  $ sign_type: chr  "pedestrian" "pedestrian" "pedestrian" "pedestrian" ...
##  $ r1       : int  155 142 57 22 169 75 136 149 13 123 ...
##  $ g1       : int  228 217 54 35 179 67 149 225 34 124 ...
##  $ b1       : int  251 242 50 41 170 60 157 241 28 107 ...
##  $ r2       : int  135 166 187 171 231 131 200 34 5 83 ...
##  $ g2       : int  188 204 201 178 254 89 203 45 21 61 ...
##  $ b2       : int  101 44 68 26 27 53 107 1 11 26 ...
##  $ r3       : int  156 142 51 19 97 214 150 155 123 116 ...
##  $ g3       : int  227 217 51 27 107 144 167 226 154 124 ...
##  $ b3       : int  245 242 45 29 99 75 134 238 140 115 ...
##  $ r4       : int  145 147 59 19 123 156 171 147 21 67 ...
##  $ g4       : int  211 219 62 27 147 169 218 222 46 67 ...
##  $ b4       : int  228 242 65 29 152 190 252 242 41 52 ...
##  $ r5       : int  166 164 156 42 221 67 171 170 36 70 ...
##  $ g5       : int  233 228 171 37 236 50 158 191 60 53 ...
##  $ b5       : int  245 229 50 3 117 36 108 113 26 26 ...
##  $ r6       : int  212 84 254 217 205 37 157 26 75 26 ...
##  $ g6       : int  254 116 255 228 225 36 186 37 108 26 ...
##  $ b6       : int  52 17 36 19 80 42 11 12 44 21 ...
##  $ r7       : int  212 217 211 221 235 44 26 34 13 52 ...
##  $ g7       : int  254 254 226 235 254 42 35 45 27 45 ...
##  $ b7       : int  11 26 70 20 60 44 10 19 25 27 ...
##  $ r8       : int  188 155 78 181 90 192 180 221 133 117 ...
##  $ g8       : int  229 203 73 183 110 131 211 249 163 109 ...
##  $ b8       : int  117 128 64 73 9 73 236 184 126 83 ...
##  $ r9       : int  170 213 220 237 216 123 129 226 83 110 ...
##  $ g9       : int  216 253 234 234 236 74 109 246 125 74 ...
##  $ b9       : int  120 51 59 44 66 22 73 59 19 12 ...
##  $ r10      : int  211 217 254 251 229 36 161 30 13 98 ...
##  $ g10      : int  254 255 255 254 255 34 190 40 27 70 ...
##  $ b10      : int  3 21 51 2 12 37 10 34 25 26 ...
##  $ r11      : int  212 217 253 235 235 44 161 34 9 20 ...
##  $ g11      : int  254 255 255 243 254 42 190 44 23 21 ...
##  $ b11      : int  19 21 44 12 60 44 6 35 18 20 ...
##  $ r12      : int  172 158 66 19 163 197 187 241 85 113 ...
##  $ g12      : int  235 225 68 27 168 114 215 255 128 76 ...
##  $ b12      : int  244 237 68 29 152 21 236 54 21 14 ...
##  $ r13      : int  172 164 69 20 124 171 141 205 83 106 ...
##  $ g13      : int  235 227 65 29 117 102 142 229 125 69 ...
##  $ b13      : int  244 237 59 34 91 26 140 46 19 9 ...
##  $ r14      : int  172 182 76 64 188 197 189 226 85 102 ...
##  $ g14      : int  228 228 84 61 205 114 171 246 128 67 ...
##  $ b14      : int  235 143 22 4 78 21 140 59 21 6 ...
##  $ r15      : int  177 171 82 211 125 123 214 235 85 106 ...
##  $ g15      : int  235 228 93 222 147 74 221 252 128 69 ...
##  $ b15      : int  244 196 17 78 20 22 201 67 21 9 ...
##  $ r16      : int  22 164 58 19 160 180 188 237 83 43 ...
##  $ g16      : int  52 227 60 27 183 107 211 254 125 29 ...
##  $ b16      : int  53 237 60 29 187 26 227 53 19 11 ...
```

```r
str(next_sign)
```

```
## 'data.frame':	1 obs. of  48 variables:
##  $ r1 : int 204
##  $ g1 : int 227
##  $ b1 : int 220
##  $ r2 : int 196
##  $ g2 : int 59
##  $ b2 : int 51
##  $ r3 : int 202
##  $ g3 : int 67
##  $ b3 : int 59
##  $ r4 : int 204
##  $ g4 : int 227
##  $ b4 : int 220
##  $ r5 : int 236
##  $ g5 : int 250
##  $ b5 : int 234
##  $ r6 : int 242
##  $ g6 : int 252
##  $ b6 : int 235
##  $ r7 : int 205
##  $ g7 : int 148
##  $ b7 : int 131
##  $ r8 : int 190
##  $ g8 : int 50
##  $ b8 : int 43
##  $ r9 : int 179
##  $ g9 : int 70
##  $ b9 : int 57
##  $ r10: int 242
##  $ g10: int 229
##  $ b10: int 212
##  $ r11: int 190
##  $ g11: int 50
##  $ b11: int 43
##  $ r12: int 193
##  $ g12: int 51
##  $ b12: int 44
##  $ r13: int 170
##  $ g13: int 197
##  $ b13: int 196
##  $ r14: int 190
##  $ g14: int 50
##  $ b14: int 43
##  $ r15: int 190
##  $ g15: int 47
##  $ b15: int 41
##  $ r16: int 165
##  $ g16: int 195
##  $ b16: int 196
```

```r
# Create a vector of labels
sign_types <- signs$sign_type

# Classify the next sign observed
# Note that we need to remove the first column from the "signs" dataset 
# because this columns is "sign_types".
# If we do not get rid of the column we run into coercion problems
knn(train = signs[-1], test = next_sign, cl = sign_types)
```

```
## [1] stop
## Levels: pedestrian speed stop
```

## Exploring the traffic sign dataset


```r
# Examine the structure of the signs dataset
str(signs)
```

```
## 'data.frame':	146 obs. of  49 variables:
##  $ sign_type: chr  "pedestrian" "pedestrian" "pedestrian" "pedestrian" ...
##  $ r1       : int  155 142 57 22 169 75 136 149 13 123 ...
##  $ g1       : int  228 217 54 35 179 67 149 225 34 124 ...
##  $ b1       : int  251 242 50 41 170 60 157 241 28 107 ...
##  $ r2       : int  135 166 187 171 231 131 200 34 5 83 ...
##  $ g2       : int  188 204 201 178 254 89 203 45 21 61 ...
##  $ b2       : int  101 44 68 26 27 53 107 1 11 26 ...
##  $ r3       : int  156 142 51 19 97 214 150 155 123 116 ...
##  $ g3       : int  227 217 51 27 107 144 167 226 154 124 ...
##  $ b3       : int  245 242 45 29 99 75 134 238 140 115 ...
##  $ r4       : int  145 147 59 19 123 156 171 147 21 67 ...
##  $ g4       : int  211 219 62 27 147 169 218 222 46 67 ...
##  $ b4       : int  228 242 65 29 152 190 252 242 41 52 ...
##  $ r5       : int  166 164 156 42 221 67 171 170 36 70 ...
##  $ g5       : int  233 228 171 37 236 50 158 191 60 53 ...
##  $ b5       : int  245 229 50 3 117 36 108 113 26 26 ...
##  $ r6       : int  212 84 254 217 205 37 157 26 75 26 ...
##  $ g6       : int  254 116 255 228 225 36 186 37 108 26 ...
##  $ b6       : int  52 17 36 19 80 42 11 12 44 21 ...
##  $ r7       : int  212 217 211 221 235 44 26 34 13 52 ...
##  $ g7       : int  254 254 226 235 254 42 35 45 27 45 ...
##  $ b7       : int  11 26 70 20 60 44 10 19 25 27 ...
##  $ r8       : int  188 155 78 181 90 192 180 221 133 117 ...
##  $ g8       : int  229 203 73 183 110 131 211 249 163 109 ...
##  $ b8       : int  117 128 64 73 9 73 236 184 126 83 ...
##  $ r9       : int  170 213 220 237 216 123 129 226 83 110 ...
##  $ g9       : int  216 253 234 234 236 74 109 246 125 74 ...
##  $ b9       : int  120 51 59 44 66 22 73 59 19 12 ...
##  $ r10      : int  211 217 254 251 229 36 161 30 13 98 ...
##  $ g10      : int  254 255 255 254 255 34 190 40 27 70 ...
##  $ b10      : int  3 21 51 2 12 37 10 34 25 26 ...
##  $ r11      : int  212 217 253 235 235 44 161 34 9 20 ...
##  $ g11      : int  254 255 255 243 254 42 190 44 23 21 ...
##  $ b11      : int  19 21 44 12 60 44 6 35 18 20 ...
##  $ r12      : int  172 158 66 19 163 197 187 241 85 113 ...
##  $ g12      : int  235 225 68 27 168 114 215 255 128 76 ...
##  $ b12      : int  244 237 68 29 152 21 236 54 21 14 ...
##  $ r13      : int  172 164 69 20 124 171 141 205 83 106 ...
##  $ g13      : int  235 227 65 29 117 102 142 229 125 69 ...
##  $ b13      : int  244 237 59 34 91 26 140 46 19 9 ...
##  $ r14      : int  172 182 76 64 188 197 189 226 85 102 ...
##  $ g14      : int  228 228 84 61 205 114 171 246 128 67 ...
##  $ b14      : int  235 143 22 4 78 21 140 59 21 6 ...
##  $ r15      : int  177 171 82 211 125 123 214 235 85 106 ...
##  $ g15      : int  235 228 93 222 147 74 221 252 128 69 ...
##  $ b15      : int  244 196 17 78 20 22 201 67 21 9 ...
##  $ r16      : int  22 164 58 19 160 180 188 237 83 43 ...
##  $ g16      : int  52 227 60 27 183 107 211 254 125 29 ...
##  $ b16      : int  53 237 60 29 187 26 227 53 19 11 ...
```

```r
# Count the number of signs of each type
table(signs$sign_type)
```

```
## 
## pedestrian      speed       stop 
##         46         49         51
```

```r
# Check r10's average red level by sign type
aggregate(r10 ~ sign_type, data = signs, mean)
```

```
##    sign_type       r10
## 1 pedestrian 113.71739
## 2      speed  80.63265
## 3       stop 132.39216
```

As expected, stop signs tend to have a higher average red value. This is how kNN identifies similar signs.

## Classifying a collection of road signs


```r
test_signs<-read.csv("test_signs.csv")

# Use kNN to identify the test road signs
sign_types <- signs$sign_type
signs_pred <- knn(train = signs[-1], test = test_signs[-1], cl = sign_types)

# Create a confusion matrix of the predicted versus actual values
signs_actual <- test_signs$sign_type
table(signs_pred, signs_actual)
```

```
##             signs_actual
## signs_pred   pedestrian speed stop
##   pedestrian         19     2    0
##   speed               0    17    0
##   stop                0     2   19
```

```r
# Compute the accuracy
mean(signs_pred == signs_actual)
```

```
## [1] 0.9322034
```

## The k of knn

With smaller neighborhoods, kNN can identify more subtle patterns in the data.

## Testing other 'k' values


```r
signs_test<-test_signs # They renamed the dataframe on Data Camp

# Compute the accuracy of the baseline model (default k = 1)
k_1 <- knn(train = signs[-1], test = signs_test[-1], cl = sign_types)
mean(k_1 == signs_actual)
```

```
## [1] 0.9322034
```

```r
# Modify the above to set k = 7
k_7 <- knn(train = signs[-1], test = signs_test[-1], cl = sign_types, k = 7)
mean(k_7 == signs_actual)
```

```
## [1] 0.9491525
```

```r
# Set k = 15 and compare to the above
k_15 <- knn(train = signs[-1], test = signs_test[-1], cl = sign_types, k = 15)
mean(k_15 == signs_actual)
```

```
## [1] 0.8813559
```

## Seeing how the neighbors voted


```r
# Use the prob parameter to get the proportion of votes for the winning class
sign_pred <- knn(train = signs[-1], test = signs_test[-1], cl = sign_types, prob = TRUE, k = 7)

# Get the "prob" attribute from the predicted classes
sign_prob <- attr(sign_pred, "prob")

# Examine the first several predictions
head(sign_pred)
```

```
## [1] pedestrian pedestrian pedestrian stop       pedestrian pedestrian
## Levels: pedestrian speed stop
```

```r
# Examine the proportion of votes for the winning class
head(sign_prob)
```

```
## [1] 0.5714286 0.5714286 0.8571429 0.5714286 0.8571429 0.5714286
```

