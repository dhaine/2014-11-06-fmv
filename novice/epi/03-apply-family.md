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
   3.95   22.20   28.10   28.20   34.20   52.30 
-------------------------------------------------------- 
cow[, "parity"]: 2
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   20.7    27.5    30.5    32.2    36.7    50.1 
-------------------------------------------------------- 
cow[, "parity"]: 2+
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   6.16   23.80   30.40   30.00   36.90   56.10 
</code></pre></div>



<pre class='in'><code>by(cow$milk, cow[, "parity"], sum)</code></pre>



<div class='out'><pre class='out'><code>cow[, "parity"]: 1
[1] 957.8
-------------------------------------------------------- 
cow[, "parity"]: 2
[1] 999.7
-------------------------------------------------------- 
cow[, "parity"]: 2+
[1] 1051
</code></pre></div>


## tapply

`tapply` applies a function to subsets of a vector.


<pre class='in'><code>tapply(cow$milk, cow$parity, mean)</code></pre>



<div class='out'><pre class='out'><code>    1     2    2+ 
28.17 32.25 30.03 
</code></pre></div>



<pre class='in'><code>kg_to_lb <- function(x){mean(x * 2.205)}
tapply(cow$milk, cow$parity, kg_to_lb)</code></pre>



<div class='out'><pre class='out'><code>    1     2    2+ 
62.11 71.11 66.21 
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



<div class='out'><pre class='out'><code>         [,1]    [,2]     [,3]      [,4]    [,5]     [,6]    [,7]     [,8]
 [1,]  0.7668  0.7497  0.46629 -1.589614 -0.5636 -0.03401  1.1123 -1.36711
 [2,] -0.8961  0.1564 -0.32791 -0.572453 -0.7462 -0.83412  1.4378 -0.48625
 [3,]  0.1358 -0.7115  1.05918 -0.116828  0.1663  0.22193  2.2237 -0.70090
 [4,]  1.0672  0.4363 -0.07396 -1.005235  1.3831 -1.03748  1.5176  1.09699
 [5,] -0.3989 -0.6100  0.25825 -0.007575  1.0929  1.11583 -0.4568 -0.92108
 [6,]  0.2214  1.2120 -2.05653  0.117709  0.6476  0.10659 -0.5188  1.22469
 [7,] -2.3706  0.2391  1.06364  0.179200 -0.2394  0.12782 -0.0933 -0.02047
 [8,]  1.5642  1.3914  0.61901 -0.653233  1.9759 -1.73107  1.0620  1.79092
 [9,]  0.3659 -0.2139  0.20855  1.103183  2.1063 -1.14629  0.7767  0.21098
[10,]  0.8129 -1.8563  0.40717  0.164047 -1.6752  1.03052  0.4641  2.59760
          [,9]    [,10]
 [1,]  1.38690  0.54253
 [2,]  1.78799  1.05938
 [3,] -0.62669  0.09466
 [4,] -0.76544 -1.18642
 [5,] -2.63058 -0.37934
 [6,] -0.02062 -0.10775
 [7,]  1.06557  0.13955
 [8,] -0.10041 -0.63278
 [9,]  0.94612 -0.93532
[10,]  0.68802  1.24092
</code></pre></div>



<pre class='in'><code>replicate(10, rnorm(10), simplify = TRUE)</code></pre>



<div class='out'><pre class='out'><code>          [,1]     [,2]    [,3]     [,4]     [,5]     [,6]     [,7]
 [1,]  0.65227  1.47664  0.8779  0.18688 -0.80794 -0.17088  0.60280
 [2,] -0.17260  0.33333 -1.1624  0.08734 -1.12366 -0.01645  0.06405
 [3,]  0.61335 -0.49099 -0.3785 -0.79292 -0.54918 -0.35244  0.14892
 [4,] -2.21235 -2.45890  1.1668 -0.20793  0.45460  0.57520  0.14274
 [5,] -2.42446  0.82666  0.7346 -0.12014 -0.97467  0.99389 -1.52465
 [6,]  0.85184 -1.10431  0.2007  0.74339  0.14953 -0.09164 -0.19681
 [7,]  0.06051 -0.41689 -1.6005  0.32177 -1.35215 -1.36906 -0.45747
 [8,] -0.20095 -0.94232  0.9857  0.05320  1.06610 -2.51200  0.07430
 [9,] -0.28616 -0.07315  2.8053  1.40341 -0.03351  0.96139 -0.67381
[10,] -1.15769 -1.00542 -0.4868  2.19842  0.89798 -0.25782  0.39470
         [,8]    [,9]    [,10]
 [1,] -1.0936 -0.5278  0.25093
 [2,]  0.8529 -1.6037  1.03137
 [3,]  1.4678  0.4896  0.13329
 [4,] -0.3701  0.8981 -0.38644
 [5,] -0.3514 -1.0902 -1.95764
 [6,]  1.9312  0.3368 -0.04482
 [7,]  0.7469 -0.5293 -0.26333
 [8,]  0.6686  1.2163 -0.85742
 [9,] -1.1088  1.0104 -0.46143
[10,]  0.1623  0.1168  0.23603
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
* you could also use `reshape2` and
  [`plyr`](http://plyr.had.co.nz/)/[`dplyr`](http://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html)
  packages


---

## aggregate


<pre class='in'><code>aggregate(milk ~ parity + breed, data = cow, FUN = mean)</code></pre>



<div class='out'><pre class='out'><code>   parity breed  milk
1       1    AY 26.42
2       2    AY 32.12
3      2+    AY 28.00
4       1    BS 27.80
5       2    BS 31.60
6      2+    BS 28.22
7       1    HO 26.76
8       2    HO 31.65
9      2+    HO 32.58
10      1    JE 30.86
11      2    JE 33.35
12     2+    JE 31.27
</code></pre></div>


## table


<pre class='in'><code>t1 <- table(cow$parity)
t1</code></pre>



<div class='out'><pre class='out'><code>
 1  2 2+ 
34 31 35 
</code></pre></div>



<pre class='in'><code>t2 <- table(cow$parity, cow$breed)
t2</code></pre>



<div class='out'><pre class='out'><code>    
     AY BS HO JE
  1   7  9  8 10
  2   4  8  9 10
  2+  8 10 10  7
</code></pre></div>



<pre class='in'><code>prop.table(t1)</code></pre>



<div class='out'><pre class='out'><code>
   1    2   2+ 
0.34 0.31 0.35 
</code></pre></div>



<pre class='in'><code>prop.table(t2)</code></pre>



<div class='out'><pre class='out'><code>    
       AY   BS   HO   JE
  1  0.07 0.09 0.08 0.10
  2  0.04 0.08 0.09 0.10
  2+ 0.08 0.10 0.10 0.07
</code></pre></div>



<pre class='in'><code>prop.table(t2, margin = 2)</code></pre>



<div class='out'><pre class='out'><code>    
         AY     BS     HO     JE
  1  0.3684 0.3333 0.2963 0.3704
  2  0.2105 0.2963 0.3333 0.3704
  2+ 0.4211 0.3704 0.3704 0.2593
</code></pre></div>

See also `xtabs()`, `ftable()` or `CrossTable()` (in library `gmodels`).

### Exercise

With the wide format `health` dataset you created, what is the median cow age in
each herd?


### Solution


<pre class='in'><code>aggregate(age ~ herd, data = health.wide, FUN = median)</code></pre>



<div class='out'><pre class='out'><code>   herd  age
1    K1 3.50
2   K10 4.30
3  K100 4.45
4  K101 2.20
5  K102 3.30
6  K103 3.40
7  K104 3.20
8  K105 4.70
9  K106 4.25
10 K107 3.90
11 K108 6.40
12  K11 3.90
13 K110 3.20
14 K111 3.15
15 K112 4.75
16 K113 3.40
17 K114 5.10
18 K115 3.60
19 K116 3.20
20 K117 3.60
21 K118 3.20
22 K119 4.80
23  K12 3.40
24 K120 3.15
25 K121 4.60
26 K122 2.25
27 K123 3.55
28 K124 4.25
29 K125 3.70
30 K126 3.50
31 K128 3.40
32 K129 6.75
33  K13 3.30
34 K130 4.05
</code></pre></div>

---

## Split-Apply-Combine in Action

The following figure can give you a sense of what is split-apply-combine.

![](split.png)

`plyr` package cna be used to apply the split-apply-combine strategy. You need
to provide the follwoing information:

1. The data structure of the input
2. The dataset being worked on
3. The variable to split the dataset on.
4. The function to apply to each split piece.
5. The data structure of the output to combine pieces.

In short, `plyr` synthetizes the entire `*apply` family. For example, the mean
age by herd and parity could be computed and saved into a data frame:


<pre class='in'><code>lact <- ddply(health.wide,
              .(herd, parity),
              summarize,
              mean.age = mean(age)
              )</code></pre>

Two other packages can also be helpful in applying this strategyL `dplyr` and
`data.table`.
