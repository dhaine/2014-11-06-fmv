

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
##    4.48   21.40   30.00   28.70   35.90   45.50 
## -------------------------------------------------------- 
## cow[, "parity"]: 2
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    13.1    27.1    33.7    32.6    37.9    45.8 
## -------------------------------------------------------- 
## cow[, "parity"]: 2+
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    8.84   24.50   30.30   30.20   37.20   45.20
```

```r
by(cow$milk, cow[, "parity"], sum)
```

```
## cow[, "parity"]: 1
## [1] 947.7
## -------------------------------------------------------- 
## cow[, "parity"]: 2
## [1] 913.6
## -------------------------------------------------------- 
## cow[, "parity"]: 2+
## [1] 1179
```


## tapply

`tapply` applies a function to subsets of a vector.


```r
tapply(cow$milk, cow$parity, mean)
```

```
##     1     2    2+ 
## 28.72 32.63 30.23
```

```r
kg_to_lb <- function(x){mean(x * 2.205)}
tapply(cow$milk, cow$parity, kg_to_lb)
```

```
##     1     2    2+ 
## 63.32 71.95 66.66
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
##          [,1]    [,2]    [,3]     [,4]    [,5]    [,6]     [,7]    [,8]
##  [1,] -0.9721 -2.0075  0.5363 -0.63090 -1.2726 -0.7708  0.69927  0.4856
##  [2,] -0.3701  0.3930  0.8870 -1.58084 -0.7087  0.2422  1.20985 -1.0164
##  [3,] -0.1507 -0.9408 -1.2987  0.11405  0.0656  2.0311  0.22016  0.7385
##  [4,] -1.2217 -0.3863 -0.3755  1.38567  0.1659 -0.6657  0.25342  1.1141
##  [5,] -1.0043 -2.3539  2.0867  0.06108 -0.4098  0.1469 -0.26275  0.2974
##  [6,] -0.8309 -0.6391  0.9890  0.01646  0.1019  0.4107 -0.03719  0.2342
##  [7,] -0.7948 -1.5612 -1.2137 -0.44618  0.5137 -1.2317  0.59925 -1.7113
##  [8,]  2.2666 -0.6664  0.5956 -1.11082  0.2814 -1.0306 -0.56038 -0.2555
##  [9,]  1.1780 -0.6642  0.7552 -0.41356  0.7468 -0.5447 -0.36256  0.3718
## [10,]  1.6610 -1.8468  0.4971 -0.23057 -1.1684 -1.7696  1.98773 -1.1713
##            [,9]     [,10]
##  [1,] -0.976636 -0.006999
##  [2,]  0.432441 -0.731608
##  [3,]  0.552259 -1.174554
##  [4,]  1.549499 -0.747868
##  [5,]  0.530009 -1.550528
##  [6,] -0.115920  0.919790
##  [7,] -0.730796 -0.476830
##  [8,] -0.001207 -1.567318
##  [9,]  2.307913 -0.035715
## [10,] -0.163589 -1.949326
```

```r
replicate(10, rnorm(10), simplify = TRUE)
```

```
##           [,1]     [,2]    [,3]    [,4]     [,5]    [,6]     [,7]    [,8]
##  [1,]  0.24124 -0.04844  0.3912 -1.8947 -0.47482 -0.4869  0.99768  1.0128
##  [2,] -0.97396  1.78536 -1.7777  0.3643  0.05913 -0.9942 -0.82587  0.6342
##  [3,] -0.33518 -0.29640 -0.5232  2.0294  0.59588  1.4052 -0.06947 -0.3397
##  [4,]  0.84847 -0.27373 -0.5393 -0.8234 -0.90478 -1.2672  0.68632 -2.6852
##  [5,]  1.15410 -0.44096  0.7105  1.0083 -0.46472 -2.0194  0.05854  0.3899
##  [6,] -0.86248  1.96386  0.8399 -0.7122  0.27715  0.6917  0.33392  0.6388
##  [7,]  0.09779 -0.61762 -0.5861  1.0451  1.51190  0.1511 -0.11377  0.3646
##  [8,] -1.34776  0.66439  1.8250  1.5527  0.26509 -0.4821 -0.58462 -0.1451
##  [9,] -0.53106  1.23916  0.4172  1.5068  1.19337  0.4188  0.77668  0.5662
## [10,]  1.49550  0.63657 -1.5508  1.4018 -0.04690  1.7356 -3.39576  1.3750
##          [,9]    [,10]
##  [1,]  0.5508  1.85089
##  [2,] -0.7038 -0.97909
##  [3,]  0.3931 -0.49085
##  [4,] -0.7656  1.23721
##  [5,] -0.7413  0.63937
##  [6,]  0.6075  0.97154
##  [7,]  0.2274  0.85230
##  [8,] -1.6221  0.04715
##  [9,]  1.0122  1.11437
## [10,] -0.6154  0.65209
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
## 1       1    AY 27.04
## 2       2    AY 30.99
## 3      2+    AY 32.75
## 4       1    BS 28.58
## 5       2    BS 36.91
## 6      2+    BS 24.96
## 7       1    HO 27.07
## 8       2    HO 39.06
## 9      2+    HO 34.45
## 10      1    JE 31.48
## 11      2    JE 28.14
## 12     2+    JE 30.63
```


## table


```r
t1 <- table(cow$parity)
t1
```

```
## 
##  1  2 2+ 
## 33 28 39
```

```r
t2 <- table(cow$parity, cow$breed)
t2
```

```
##     
##      AY BS HO JE
##   1   9  7  7 10
##   2   8  8  3  9
##   2+ 14 12  6  7
```

```r
prop.table(t1)
```

```
## 
##    1    2   2+ 
## 0.33 0.28 0.39
```

```r
prop.table(t2)
```

```
##     
##        AY   BS   HO   JE
##   1  0.09 0.07 0.07 0.10
##   2  0.08 0.08 0.03 0.09
##   2+ 0.14 0.12 0.06 0.07
```

```r
prop.table(t2, margin = 2)
```

```
##     
##          AY     BS     HO     JE
##   1  0.2903 0.2593 0.4375 0.3846
##   2  0.2581 0.2963 0.1875 0.3462
##   2+ 0.4516 0.4444 0.3750 0.2692
```

See also `xtabs()`, `ftable()` or `CrossTable()` (in library `gmodels`).

