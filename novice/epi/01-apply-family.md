---
layout: lesson
root: ../..
---



# Learning the apply family of functions

One of the greatest joys of vectorized operations is being able to use the
entire family of `apply` functions that are available in base `R`.

These include:


<pre class='in'><code>apply
by
lapply
tapply
sapply</code></pre>

## apply

`apply` applies a function to each row or column of a matrix. It's a convenient
way to get marginal values. It follows this syntax: `apply(object, dimension,
function)`.


<pre class='in'><code>m <- matrix(c(1:10, 11:20), nrow = 10, ncol = 2)
m</code></pre>



<div class='out'><pre class='out'><code>      [,1] [,2]
 [1,]    1   11
 [2,]    2   12
 [3,]    3   13
 [4,]    4   14
 [5,]    5   15
 [6,]    6   16
 [7,]    7   17
 [8,]    8   18
 [9,]    9   19
[10,]   10   20
</code></pre></div>



<pre class='in'><code># 1 is the row index
# 2 is the column index
apply(m, 1, sum)  # row totals</code></pre>



<div class='out'><pre class='out'><code> [1] 12 14 16 18 20 22 24 26 28 30
</code></pre></div>



<pre class='in'><code>apply(m, 2, sum)  # column totals</code></pre>



<div class='out'><pre class='out'><code>[1]  55 155
</code></pre></div>



<pre class='in'><code>apply(m, 1, mean)</code></pre>



<div class='out'><pre class='out'><code> [1]  6  7  8  9 10 11 12 13 14 15
</code></pre></div>



<pre class='in'><code>apply(m, 2, mean)</code></pre>



<div class='out'><pre class='out'><code>[1]  5.5 15.5
</code></pre></div>

There are convenience functions based on `apply`: `rowSums(x)`, `colSums(x)`,
`rowMeans(x)`, `colMeans(x)`, `addmargins`.

## by

`by` applies a function to subsets of a data frame.


<pre class='in'><code>cow <- data.frame(cow = 1:100,
                  milk = rnorm(100, mean = 30, sd = 9),
                  parity = sample(c("1", "2", "2+"), 100, replace = TRUE),
                  breed = sample(c("HO", "JE", "AY", "BS"), 100, replace = TRUE))
by(cow$milk, cow[, "parity"], summary)</code></pre>



<div class='out'><pre class='out'><code>cow[, "parity"]: 1
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   9.18   26.20   31.20   29.60   34.20   42.70 
-------------------------------------------------------- 
cow[, "parity"]: 2
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   8.69   25.10   31.00   30.80   37.10   43.70 
-------------------------------------------------------- 
cow[, "parity"]: 2+
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   17.3    24.9    31.2    31.6    37.3    50.2 
</code></pre></div>



<pre class='in'><code>by(cow$milk, cow[, "parity"], sum)</code></pre>



<div class='out'><pre class='out'><code>cow[, "parity"]: 1
[1] 1065
-------------------------------------------------------- 
cow[, "parity"]: 2
[1] 1233
-------------------------------------------------------- 
cow[, "parity"]: 2+
[1] 758.7
</code></pre></div>


## tapply

`tapply` applies a function to subsets of a vector.


<pre class='in'><code>tapply(cow$milk, cow$parity, mean)</code></pre>



<div class='out'><pre class='out'><code>    1     2    2+ 
29.58 30.83 31.61 
</code></pre></div>



<pre class='in'><code>kg_to_lb <- function(x){mean(x * 2.205)}
tapply(cow$milk, cow$parity, kg_to_lb)</code></pre>



<div class='out'><pre class='out'><code>    1     2    2+ 
65.23 67.99 69.71 
</code></pre></div>

`tapply()` returns an array; `by()`returns a list. 

## lapply (and llply)

What it does: Returns a list of same length as the input. 
Each element of the output is a result of applying a function to the
corresponding element.


<pre class='in'><code>my_list <- list(a = 1:10, b = 2:20)
my_list</code></pre>



<div class='out'><pre class='out'><code>$a
 [1]  1  2  3  4  5  6  7  8  9 10

$b
 [1]  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
</code></pre></div>



<pre class='in'><code>lapply(my_list, mean)</code></pre>



<div class='out'><pre class='out'><code>$a
[1] 5.5

$b
[1] 11
</code></pre></div>


## sapply

`sapply` is a more user friendly version of `lapply` and will return a list of
matrix where appropriate.

Let's work with the same list we just created.


<pre class='in'><code>my_list</code></pre>



<div class='out'><pre class='out'><code>$a
 [1]  1  2  3  4  5  6  7  8  9 10

$b
 [1]  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
</code></pre></div>



<pre class='in'><code>x <- sapply(my_list, mean)
x</code></pre>



<div class='out'><pre class='out'><code>   a    b 
 5.5 11.0 
</code></pre></div>



<pre class='in'><code>class(x)</code></pre>



<div class='out'><pre class='out'><code>[1] "numeric"
</code></pre></div>


## replicate

An extremely useful function to generate datasets for simulation purposes. 


<pre class='in'><code>replicate(10, rnorm(10))</code></pre>



<div class='out'><pre class='out'><code>         [,1]    [,2]     [,3]    [,4]    [,5]     [,6]     [,7]      [,8]
 [1,] -0.8077 -0.4946  1.26891  1.2535  0.1755 -0.06703 -0.95751 -1.324438
 [2,] -0.1443  0.1759  0.48662 -0.3923 -1.0600  1.86823 -0.75198  0.242978
 [3,] -2.1187  0.5076 -1.10846  2.0835 -1.6788  0.40975  0.15141 -0.070791
 [4,] -1.5420  1.5990 -1.87422  1.1255 -0.6371 -1.15368 -0.04314 -0.163756
 [5,] -0.3513  0.7307  0.48194 -0.9479  0.1287  0.42514 -0.68336 -2.059926
 [6,] -0.1291 -0.1086 -0.63883  0.8143 -1.1216 -1.93080  0.99431 -0.239709
 [7,]  1.3918  0.2053  0.41917  1.0055  0.2461  1.00383 -0.56776 -0.101047
 [8,] -0.6181  1.4724 -1.19785  0.3212  0.5668 -0.76010 -0.98955 -0.249446
 [9,]  1.4723 -0.7736  0.57119 -0.2095 -0.2652 -0.23208 -1.38243  0.009525
[10,] -1.2392 -0.1647 -0.09768 -0.1102  0.6126 -0.48011  0.32457  0.305553
         [,9]    [,10]
 [1,] -1.4106 -0.47855
 [2,] -0.8428 -0.71226
 [3,]  1.1376 -0.07234
 [4,]  0.4627 -0.02320
 [5,] -1.8838  0.03751
 [6,] -0.2554  1.17454
 [7,]  1.0365 -0.22534
 [8,] -0.1884  0.06404
 [9,] -2.0567  1.16794
[10,]  0.8474  0.04898
</code></pre></div>



<pre class='in'><code>replicate(10, rnorm(10), simplify = TRUE)</code></pre>



<div class='out'><pre class='out'><code>         [,1]    [,2]     [,3]     [,4]    [,5]     [,6]    [,7]    [,8]
 [1,] -1.0000 -0.6429  1.06603 -0.73188 -0.9558 -0.07603 -1.7120 -0.7634
 [2,] -0.7014 -1.3609 -0.54228 -0.09548  1.5042  1.08486  0.7819 -1.6702
 [3,] -0.4812  2.2644  0.09932  0.78816  1.3441  0.97322 -0.2703  1.7311
 [4,]  0.2008 -0.1232  2.05821 -0.85978 -0.4539 -0.79406  1.0282 -0.6484
 [5,]  1.2043  0.5410  1.13018 -1.71038 -1.9845 -0.38350 -0.8091 -1.0008
 [6,] -1.5963  0.9894 -0.41716  1.75136  1.3440  0.51070  1.8031 -1.3375
 [7,] -0.1726 -0.3871 -0.79946 -0.73367 -1.2473  0.45196 -0.6829  1.7772
 [8,] -0.9345 -0.2063 -0.72468  1.31068  0.6183 -1.54435  0.1354  2.5357
 [9,]  0.3276  1.1283  0.06735  0.14916  1.4436  1.02558 -1.3674 -3.3806
[10,] -0.5358 -0.4258 -0.50536 -1.31044  1.6728  0.22932  0.6169  0.6753
          [,9]   [,10]
 [1,]  0.89007 -0.8771
 [2,]  0.04724 -1.9064
 [3,]  0.01105 -0.6476
 [4,]  1.36643  1.8120
 [5,]  0.11875 -2.9438
 [6,]  1.18917  0.2740
 [7,] -0.27572  1.8116
 [8,] -0.40395  1.1300
 [9,] -0.65940  0.2898
[10,] -0.37488  1.8176
</code></pre></div>

The final arguments turns the result into a vector or matrix if possible.


## mapply
Its more or less a multivariate version of `sapply`. It applies a function to
all corresponding elements of each argument. 

example:


<pre class='in'><code>list_1 <- list(a = c(1:10), b = c(11:20))
list_1</code></pre>



<div class='out'><pre class='out'><code>$a
 [1]  1  2  3  4  5  6  7  8  9 10

$b
 [1] 11 12 13 14 15 16 17 18 19 20
</code></pre></div>



<pre class='in'><code>list_2 <- list(c = c(21:30), d = c(31:40))
list_2</code></pre>



<div class='out'><pre class='out'><code>$c
 [1] 21 22 23 24 25 26 27 28 29 30

$d
 [1] 31 32 33 34 35 36 37 38 39 40
</code></pre></div>



<pre class='in'><code>mapply(sum, list_1$a, list_1$b, list_2$c, list_2$d)</code></pre>



<div class='out'><pre class='out'><code> [1]  64  68  72  76  80  84  88  92  96 100
</code></pre></div>


---

* `apply` functions are more computationally efficient than loops
* you could also use `reshape` and
  [`plyr`](http://plyr.had.co.nz/)/[`dplyr`](http://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html)
  packages


---

## aggregate


<pre class='in'><code>aggregate(milk ~ parity + breed, data = cow, FUN = mean)</code></pre>



<div class='out'><pre class='out'><code>   parity breed  milk
1       1    AY 27.15
2       2    AY 32.90
3      2+    AY 37.87
4       1    BS 32.69
5       2    BS 28.50
6      2+    BS 30.40
7       1    HO 29.36
8       2    HO 29.58
9      2+    HO 27.49
10      1    JE 29.58
11      2    JE 30.88
12     2+    JE 33.20
</code></pre></div>


## table


<pre class='in'><code>t1 <- table(cow$parity)
t1</code></pre>



<div class='out'><pre class='out'><code>
 1  2 2+ 
36 40 24 
</code></pre></div>



<pre class='in'><code>t2 <- table(cow$parity, cow$breed)
t2</code></pre>



<div class='out'><pre class='out'><code>    
     AY BS HO JE
  1  12 10  8  6
  2  15  8 10  7
  2+  4  6  7  7
</code></pre></div>



<pre class='in'><code>prop.table(t1)</code></pre>



<div class='out'><pre class='out'><code>
   1    2   2+ 
0.36 0.40 0.24 
</code></pre></div>



<pre class='in'><code>prop.table(t2)</code></pre>



<div class='out'><pre class='out'><code>    
       AY   BS   HO   JE
  1  0.12 0.10 0.08 0.06
  2  0.15 0.08 0.10 0.07
  2+ 0.04 0.06 0.07 0.07
</code></pre></div>



<pre class='in'><code>prop.table(t2, margin = 2)</code></pre>



<div class='out'><pre class='out'><code>    
         AY     BS     HO     JE
  1  0.3871 0.4167 0.3200 0.3000
  2  0.4839 0.3333 0.4000 0.3500
  2+ 0.1290 0.2500 0.2800 0.3500
</code></pre></div>

See also `xtabs()`, `ftable()` or `CrossTable()` (in library `gmodels`).

