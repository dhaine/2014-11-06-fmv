

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
##    7.31   26.00   32.00   30.60   37.20   48.10 
## -------------------------------------------------------- 
## cow[, "parity"]: 2
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    12.6    23.1    31.0    30.1    37.2    48.4 
## -------------------------------------------------------- 
## cow[, "parity"]: 2+
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    9.88   24.10   30.50   31.00   37.20   46.50
```

```r
by(cow$milk, cow[, "parity"], sum)
```

```
## cow[, "parity"]: 1
## [1] 1132
## -------------------------------------------------------- 
## cow[, "parity"]: 2
## [1] 1083
## -------------------------------------------------------- 
## cow[, "parity"]: 2+
## [1] 837.8
```


## tapply

`tapply` applies a function to subsets of a vector.


```r
tapply(cow$milk, cow$parity, mean)
```

```
##     1     2    2+ 
## 30.59 30.09 31.03
```

```r
kg_to_lb <- function(x){mean(x * 2.205)}
tapply(cow$milk, cow$parity, kg_to_lb)
```

```
##     1     2    2+ 
## 67.45 66.35 68.42
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
##          [,1]    [,2]     [,3]     [,4]    [,5]    [,6]     [,7]    [,8]
##  [1,]  1.3067 -1.4859 -0.09341  0.40131  0.2795  0.4193 -1.47023 -0.8685
##  [2,]  0.7840  0.5213  1.88728  0.03291 -2.9112  0.3836  0.92493 -0.1098
##  [3,] -1.0534  1.0422  0.96799 -2.30792 -0.3295 -0.6209 -2.35937  0.4455
##  [4,]  0.9612  0.2142  0.41810 -0.63224 -1.0479  0.6746 -0.36022  0.5586
##  [5,]  1.2666 -0.9912  1.07020  0.07693 -0.9015  1.0867  0.63337  1.1424
##  [6,]  0.3898  0.1678  0.06752 -1.17034 -2.5005  2.7074  0.15618 -0.9702
##  [7,] -1.2863  2.8508 -0.66749 -0.68467 -0.5046  0.1719  0.25688  1.2149
##  [8,] -0.4309  3.0646 -1.01996  0.02466  2.0738 -0.4259  0.07283  0.8627
##  [9,] -0.4190 -0.2745 -0.62270 -2.00746 -0.1170  0.7088 -1.11930 -1.0794
## [10,]  1.3280  1.3348  0.69296 -0.92890 -1.3085 -0.2281 -1.25472  0.2278
##           [,9]   [,10]
##  [1,] -0.24145  0.5439
##  [2,] -0.41007  0.3763
##  [3,] -0.97838  0.6673
##  [4,]  0.15444 -0.7817
##  [5,] -0.02641 -0.6196
##  [6,] -1.56855 -1.2658
##  [7,]  1.71340  2.0371
##  [8,] -1.36035  1.4133
##  [9,]  0.44262 -1.5840
## [10,] -0.38811 -0.6039
```

```r
replicate(10, rnorm(10), simplify = TRUE)
```

```
##          [,1]      [,2]     [,3]     [,4]    [,5]     [,6]     [,7]
##  [1,]  0.8684  0.008577  0.45000  0.33610 -1.2743  0.51296  0.09192
##  [2,]  2.2061  0.424799  1.12744 -0.03575 -0.9913  0.03438 -0.58055
##  [3,] -0.3677  1.541390  0.87601 -0.43785  0.5632  0.25818  1.21950
##  [4,] -0.6395  0.341695  0.39091 -1.63705  2.5923 -0.12062 -0.59892
##  [5,]  1.4793  0.378538  0.04161 -0.36981 -0.8673  0.39979  0.44335
##  [6,]  1.8813 -0.466872 -0.16777 -0.35628  1.4743 -1.35312  0.14859
##  [7,]  0.1378  2.271415 -0.86289  0.15438  0.8900  0.92211 -0.92721
##  [8,]  0.4875  0.878628 -0.01497  1.66225 -0.3764  1.72749 -1.65882
##  [9,] -1.0308  1.535098 -0.70781  0.16905 -0.2771  0.25023  0.05378
## [10,] -1.0892 -2.455449 -0.91049 -0.10456 -0.5196 -1.71900  0.17112
##          [,8]    [,9]   [,10]
##  [1,]  1.6419 -1.8467 -0.3045
##  [2,] -0.5037 -0.3863  1.0071
##  [3,]  1.5346 -1.5065  0.5493
##  [4,] -0.4469  0.6751  0.1835
##  [5,]  0.7233  0.7981 -0.1838
##  [6,]  0.2436  1.1336 -0.3059
##  [7,]  0.3736 -0.2112 -1.5033
##  [8,]  1.4988 -2.5653  1.6247
##  [9,] -0.0532 -0.8811 -0.2624
## [10,]  0.1800  0.3997  1.2100
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
## 1       1    AY 33.90
## 2       2    AY 25.62
## 3      2+    AY 30.00
## 4       1    BS 19.19
## 5       2    BS 27.12
## 6      2+    BS 31.95
## 7       1    HO 25.42
## 8       2    HO 36.76
## 9      2+    HO 30.18
## 10      1    JE 33.55
## 11      2    JE 34.46
## 12     2+    JE 31.52
```


## table


```r
t1 <- table(cow$parity)
t1
```

```
## 
##  1  2 2+ 
## 37 36 27
```

```r
t2 <- table(cow$parity, cow$breed)
t2
```

```
##     
##      AY BS HO JE
##   1  14  4  7 12
##   2  11 11  9  5
##   2+  8 10  4  5
```

```r
prop.table(t1)
```

```
## 
##    1    2   2+ 
## 0.37 0.36 0.27
```

```r
prop.table(t2)
```

```
##     
##        AY   BS   HO   JE
##   1  0.14 0.04 0.07 0.12
##   2  0.11 0.11 0.09 0.05
##   2+ 0.08 0.10 0.04 0.05
```

```r
prop.table(t2, margin = 2)
```

```
##     
##          AY     BS     HO     JE
##   1  0.4242 0.1600 0.3500 0.5455
##   2  0.3333 0.4400 0.4500 0.2273
##   2+ 0.2424 0.4000 0.2000 0.2273
```

See also `xtabs()`, `ftable()` or `CrossTable()` (in library `gmodels`).

