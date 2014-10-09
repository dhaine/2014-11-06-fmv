---
layout: lesson
root: ../..
----



# Learning the apply family of functions

One of the greatest joys of vectorized operations is being able to use the
entire family of `apply` functions that are available in base `R`.

These include:


```r
apply
by
lapply
tapply
sapply
```

## apply

`apply` applies a function to each row or column of a matrix. It's a convenient
way to get marginal values. It follows this syntax: `apply(object, dimension,
function)`.


```r
m <- matrix(c(1:10, 11:20), nrow = 10, ncol = 2)
m
```

```
##       [,1] [,2]
##  [1,]    1   11
##  [2,]    2   12
##  [3,]    3   13
##  [4,]    4   14
##  [5,]    5   15
##  [6,]    6   16
##  [7,]    7   17
##  [8,]    8   18
##  [9,]    9   19
## [10,]   10   20
```

```r
# 1 is the row index
# 2 is the column index
apply(m, 1, sum)  # row totals
```

```
##  [1] 12 14 16 18 20 22 24 26 28 30
```

```r
apply(m, 2, sum)  # column totals
```

```
## [1]  55 155
```

```r
apply(m, 1, mean)
```

```
##  [1]  6  7  8  9 10 11 12 13 14 15
```

```r
apply(m, 2, mean)
```

```
## [1]  5.5 15.5
```

There are convenience functions based on `apply`: `rowSums(x)`, `colSums(x)`,
`rowMeans(x)`, `colMeans(x)`, `addmargins`.

## by

`by` applies a function to subsets of a data frame.


```r
cow <- data.frame(cow = 1:100,
                  milk = rnorm(100, mean = 30, sd = 9),
                  parity = sample(c("1", "2", "2+"), 100, replace = TRUE),
                  breed = sample(c("HO", "JE", "AY", "BS"), 100, replace = TRUE))
by(cow$milk, cow[, "parity"], summary)
```

```
## cow[, "parity"]: 1
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    6.96   24.00   28.60   28.90   34.50   42.40 
## -------------------------------------------------------- 
## cow[, "parity"]: 2
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    12.5    26.2    31.1    31.6    37.9    52.5 
## -------------------------------------------------------- 
## cow[, "parity"]: 2+
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    12.4    25.0    29.0    31.3    35.4    55.9
```

```r
by(cow$milk, cow[, "parity"], sum)
```

```
## cow[, "parity"]: 1
## [1] 1158
## -------------------------------------------------------- 
## cow[, "parity"]: 2
## [1] 917.2
## -------------------------------------------------------- 
## cow[, "parity"]: 2+
## [1] 971.7
```


## tapply

`tapply` applies a function to subsets of a vector.


```r
tapply(cow$milk, cow$parity, mean)
```

```
##     1     2    2+ 
## 28.95 31.63 31.35
```

```r
kg_to_lb <- function(x){mean(x * 2.205)}
tapply(cow$milk, cow$parity, kg_to_lb)
```

```
##     1     2    2+ 
## 63.82 69.74 69.12
```

`tapply()` returns an array; `by()`returns a list. 

## lapply (and llply)

What it does: Returns a list of same length as the input. 
Each element of the output is a result of applying a function to the
corresponding element.


```r
my_list <- list(a = 1:10, b = 2:20)
my_list
```

```
## $a
##  [1]  1  2  3  4  5  6  7  8  9 10
## 
## $b
##  [1]  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
```

```r
lapply(my_list, mean)
```

```
## $a
## [1] 5.5
## 
## $b
## [1] 11
```


## sapply

`sapply` is a more user friendly version of `lapply` and will return a list of
matrix where appropriate.

Let's work with the same list we just created.


```r
my_list
```

```
## $a
##  [1]  1  2  3  4  5  6  7  8  9 10
## 
## $b
##  [1]  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
```

```r
x <- sapply(my_list, mean)
x
```

```
##    a    b 
##  5.5 11.0
```

```r
class(x)
```

```
## [1] "numeric"
```


## replicate

An extremely useful function to generate datasets for simulation purposes. 


```r
replicate(10, rnorm(10))
```

```
##           [,1]    [,2]     [,3]    [,4]      [,5]    [,6]    [,7]     [,8]
##  [1,]  0.26367 -0.7572  0.28225 -1.2427 -1.000133 -0.4986 -0.6040 -0.90270
##  [2,]  0.05931  1.2615 -0.11739  0.3434 -0.019019  0.8482  0.4927  0.92169
##  [3,]  1.29533  0.3753 -0.08129 -0.1644 -0.550409 -0.7709 -0.9790 -1.63956
##  [4,] -0.86584  1.0676  0.80114 -0.5454 -2.531136  0.4086 -1.4688  0.62332
##  [5,] -0.37248  1.9349  2.60488 -0.5006 -0.002257  0.7314  1.2269  0.31195
##  [6,]  2.55284  0.7749  0.86954 -0.3821  0.572026  0.4874 -0.8391  0.15092
##  [7,] -1.32526 -0.8163  0.37137 -0.8800  0.480251 -0.7651 -1.3515  0.13111
##  [8,] -1.44633  1.0201 -0.72302  0.3242  0.086251 -0.5288 -1.3565 -1.19356
##  [9,]  0.89081 -0.8071 -0.91163 -1.5300 -1.343942 -0.9594  0.4280 -0.02995
## [10,] -2.13737  0.3219 -0.25220 -0.9426 -0.926704  0.9312 -0.2860  3.90706
##           [,9]   [,10]
##  [1,]  0.92098  0.2643
##  [2,] -0.92606  0.1791
##  [3,]  0.64796 -0.7392
##  [4,] -0.35371  0.7625
##  [5,]  1.51145 -1.7302
##  [6,] -0.51577  1.2584
##  [7,] -0.08102 -0.6317
##  [8,]  0.65853  0.3468
##  [9,]  0.69579 -0.3653
## [10,]  0.41961  0.3480
```

```r
replicate(10, rnorm(10), simplify = TRUE)
```

```
##          [,1]    [,2]     [,3]    [,4]     [,5]     [,6]    [,7]     [,8]
##  [1,] -2.3215 -0.7379  0.15749  1.4292  0.20481  0.37084 -1.4496  0.23210
##  [2,]  0.3732  0.7258 -0.09936 -0.3242 -1.16784  0.89219 -0.8178  1.94437
##  [3,]  1.0665 -1.6290  0.35547 -0.3430  0.91766  0.96475 -0.4612 -0.63287
##  [4,]  0.3923  1.4424  1.14262  0.4816 -1.39088 -2.07817 -0.8526 -0.07084
##  [5,]  0.6020 -0.9337 -0.49230 -0.4709 -0.16034 -2.28502 -1.3145 -0.26178
##  [6,]  0.5232  0.3755 -1.57563  0.7555  1.50269  0.54157 -0.5091 -1.82875
##  [7,] -0.7483 -0.7933 -0.70217  0.1709  0.38688 -1.75335  0.6770  0.33862
##  [8,] -0.1727  2.1254 -1.29838  1.5271 -0.79128 -0.98534 -0.2339  0.41611
##  [9,]  0.5463 -0.7988  0.36929 -0.9927 -0.05279 -0.05837 -0.7291  0.18747
## [10,] -0.1532 -0.7632 -0.59781  0.4964 -1.08642  1.39833 -0.3261 -0.82269
##          [,9]   [,10]
##  [1,]  1.3260  1.8741
##  [2,] -0.9035  0.9769
##  [3,]  1.0349  2.2842
##  [4,] -0.4038  0.7580
##  [5,]  0.5230  0.1775
##  [6,] -0.1383 -0.1139
##  [7,] -0.6680  1.2358
##  [8,]  0.3158  1.7765
##  [9,]  0.8574  0.9604
## [10,]  0.7529  0.3948
```

The final arguments turns the result into a vector or matrix if possible.


## mapply
Its more or less a multivariate version of `sapply`. It applies a function to
all corresponding elements of each argument. 

example:


```r
list_1 <- list(a = c(1:10), b = c(11:20))
list_1
```

```
## $a
##  [1]  1  2  3  4  5  6  7  8  9 10
## 
## $b
##  [1] 11 12 13 14 15 16 17 18 19 20
```

```r
list_2 <- list(c = c(21:30), d = c(31:40))
list_2
```

```
## $c
##  [1] 21 22 23 24 25 26 27 28 29 30
## 
## $d
##  [1] 31 32 33 34 35 36 37 38 39 40
```

```r
mapply(sum, list_1$a, list_1$b, list_2$c, list_2$d)
```

```
##  [1]  64  68  72  76  80  84  88  92  96 100
```


---

* `apply` functions are more computationally efficient than loops
* you could also use `reshape` and
  [`plyr`](http://plyr.had.co.nz/)/[`dplyr`](http://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html)
  packages


---

## aggregate


```r
aggregate(milk ~ parity + breed, data = cow, FUN = mean)
```

```
##    parity breed  milk
## 1       1    AY 30.46
## 2       2    AY 31.72
## 3      2+    AY 31.21
## 4       1    BS 26.55
## 5       2    BS 31.46
## 6      2+    BS 42.14
## 7       1    HO 33.92
## 8       2    HO 37.96
## 9      2+    HO 25.40
## 10      1    JE 24.00
## 11      2    JE 25.51
## 12     2+    JE 28.64
```


## table


```r
t1 <- table(cow$parity)
t1
```

```
## 
##  1  2 2+ 
## 40 29 31
```

```r
t2 <- table(cow$parity, cow$breed)
t2
```

```
##     
##      AY BS HO JE
##   1  14 11  8  7
##   2   8 11  5  5
##   2+  6  7  8 10
```

```r
prop.table(t1)
```

```
## 
##    1    2   2+ 
## 0.40 0.29 0.31
```

```r
prop.table(t2)
```

```
##     
##        AY   BS   HO   JE
##   1  0.14 0.11 0.08 0.07
##   2  0.08 0.11 0.05 0.05
##   2+ 0.06 0.07 0.08 0.10
```

```r
prop.table(t2, margin = 2)
```

```
##     
##          AY     BS     HO     JE
##   1  0.5000 0.3793 0.3810 0.3182
##   2  0.2857 0.3793 0.2381 0.2273
##   2+ 0.2143 0.2414 0.3810 0.4545
```

See also `xtabs()`, `ftable()` or `CrossTable()` (in library `gmodels`).

