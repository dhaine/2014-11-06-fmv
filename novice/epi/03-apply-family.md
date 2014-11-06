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


<pre class='in'><code>by(prod.long$milk, prod.long[, "test"], summary)</code></pre>



<div class='out'><pre class='out'><code>prod.long[, "test"]: 1
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
  10.40   26.00   33.00   33.41   40.90   59.20     135 
-------------------------------------------------------- 
prod.long[, "test"]: 10
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   5.20   16.50   21.60   21.78   25.90   45.30     332 
-------------------------------------------------------- 
prod.long[, "test"]: 2
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   9.40   27.70   34.40   35.05   41.90   60.30     141 
-------------------------------------------------------- 
prod.long[, "test"]: 3
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   3.80   25.20   32.00   32.86   39.00   64.50     158 
-------------------------------------------------------- 
prod.long[, "test"]: 4
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
  10.00   24.00   30.50   31.29   38.58   57.60     174 
-------------------------------------------------------- 
prod.long[, "test"]: 5
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
  10.10   22.10   29.80   29.74   36.10   54.20     199 
-------------------------------------------------------- 
prod.long[, "test"]: 6
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
  10.00   22.30   28.40   28.43   34.60   54.80     219 
-------------------------------------------------------- 
prod.long[, "test"]: 7
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   9.70   20.22   26.55   26.71   32.70   52.50     234 
-------------------------------------------------------- 
prod.long[, "test"]: 8
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   4.20   19.08   25.20   24.95   30.13   46.90     252 
-------------------------------------------------------- 
prod.long[, "test"]: 9
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   5.00   18.20   23.00   23.10   27.05   45.50     285 
</code></pre></div>

### Exercise 1
Using `by`, what is the mean milk production for each monthly test? (use the
`prod.long` dataset)

### Solution


<pre class='in'><code>by(prod.long$milk, prod.long[, "test"], function(x) mean(x, na.rm = TRUE))</code></pre>

## tapply

`tapply` applies a function to subsets of a vector.


<pre class='in'><code>tapply(health.wide$age, health.wide$lactation, mean)</code></pre>



<div class='out'><pre class='out'><code>        1         2         3         4         5         6         7 
 2.234899  3.372519  4.447423  5.574000  6.663043  7.457143  8.685714 
        8         9 
 9.825000 10.100000 
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



<div class='out'><pre class='out'><code>             [,1]       [,2]       [,3]        [,4]       [,5]       [,6]
 [1,] -0.95722458 -0.1746410 -1.1212418 -0.03851547 -0.2775677  0.4293113
 [2,] -0.30750311 -1.3474433  1.2989843  0.32907150 -0.1760832 -0.2463999
 [3,]  1.40105966  1.3675893  1.0992285  0.03963650  0.2934295 -1.2741958
 [4,]  0.81664102  0.5767650  1.0596794  0.67641411  0.0250012  1.0092887
 [5,] -0.04353053  0.2644858 -0.2970318  0.45850009 -0.4950472  1.3406497
 [6,]  0.40501250 -2.1605415 -0.3243596  0.43610242 -0.1293929 -0.2167605
 [7,] -0.30039452  0.1303758 -1.5946648 -0.67434235  1.5422946  0.3692004
 [8,]  1.38483291  0.4480906  1.1968644 -2.11796000 -2.9360524  0.4665804
 [9,]  1.45574455 -0.2518135 -0.9202927  0.24272044 -0.2274694  0.5900310
[10,]  0.82043434 -0.4205003  1.2234671  0.05273058  0.6538569 -0.6299088
            [,7]         [,8]         [,9]       [,10]
 [1,] -0.8344161 -0.507476805 -1.856902774 -1.05977417
 [2,]  0.7323461 -0.505876245 -0.032094869 -0.06842195
 [3,]  0.9742495  0.847529043  0.715935576  0.84306642
 [4,]  0.2679581  0.608173525 -1.183088716 -0.09491204
 [5,] -0.4838054 -0.008392126 -0.970888578 -0.37791885
 [6,] -0.9502041 -0.262922336 -0.263149680  0.47644226
 [7,] -0.5574268 -1.279514485 -0.004736906  0.26869358
 [8,] -0.1793472 -0.727688276  0.510964981 -0.95570437
 [9,] -2.1893002 -1.026565725  0.437831680  0.59701257
[10,]  1.0217861  0.927338726 -0.680099727  0.38534926
</code></pre></div>



<pre class='in'><code>replicate(10, rnorm(10), simplify = TRUE)</code></pre>



<div class='out'><pre class='out'><code>            [,1]       [,2]        [,3]        [,4]        [,5]       [,6]
 [1,] -0.1221847  0.7260033  0.19991247 -1.17037381 -0.49317426 -1.2580341
 [2,]  0.7743752 -0.3811260  0.60631568 -1.74038715  0.08716704 -0.4681509
 [3,] -1.2343826 -1.2342070  0.65678457  1.21378692  0.44838335  0.6942896
 [4,]  0.9114560  0.7113268 -0.06922040  0.05059820 -1.56661644 -0.7709096
 [5,]  1.4352579  1.0185572 -1.90553346  1.53980125  2.23690837 -1.6101425
 [6,]  2.0818898 -1.2056564 -1.13217187  1.48808113 -0.27715487  0.4932312
 [7,] -0.4896037 -0.7631312 -0.16406809  0.26236069 -0.84160384  0.4270507
 [8,]  0.7316817 -0.4279365 -0.91174798 -0.05756756  1.12052784  1.0623881
 [9,] -0.3364964 -0.7986739 -0.89418678 -0.09806616  0.17870598  1.3159870
[10,] -0.8646764  2.2787989  0.01699213 -0.52690732 -2.08281237  1.2076389
            [,7]       [,8]       [,9]       [,10]
 [1,] -0.1821960 -1.9238387  1.2047836 -0.24665761
 [2,]  1.5664676  0.6156399 -0.1113554 -0.69073383
 [3,] -0.3857960  2.0666271  0.8415383 -0.14537979
 [4,] -1.2562010 -1.1565143 -0.8794926 -0.44068511
 [5,] -0.4637832 -1.2059193  0.5020955  1.07799034
 [6,] -1.9004877  0.1720717 -0.7351375 -0.92866159
 [7,]  0.3050111  0.7304313  1.2149550  1.00414054
 [8,] -0.3968534  0.5721941  2.0768743  0.09785746
 [9,]  0.5419702 -1.0746962  0.1252881 -0.97874062
[10,] -0.1742323 -0.9406756  0.2771703  1.29709105
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


<pre class='in'><code>aggregate(age ~ lactation + da, data = health.wide, FUN = mean)</code></pre>



<div class='out'><pre class='out'><code>   lactation  da       age
1          1  No  2.235862
2          2  No  3.366667
3          3  No  4.417582
4          4  No  5.572917
5          5  No  6.668889
6          6  No  7.457143
7          7  No  8.816667
8          8  No  9.825000
9          9  No 10.100000
10         1 Yes  2.200000
11         2 Yes  3.750000
12         3 Yes  4.900000
13         4 Yes  5.600000
14         5 Yes  6.400000
15         7 Yes  7.900000
</code></pre></div>


## table


<pre class='in'><code>t1 <- table(health.wide$parity)
t1</code></pre>



<div class='out'><pre class='out'><code>
  1  2+ 
149 351 
</code></pre></div>



<pre class='in'><code>t2 <- table(health.wide$parity, health.wide$da)
t2</code></pre>



<div class='out'><pre class='out'><code>    
      No Yes
  1  145   4
  2+ 339  12
</code></pre></div>



<pre class='in'><code>prop.table(t1)</code></pre>



<div class='out'><pre class='out'><code>
    1    2+ 
0.298 0.702 
</code></pre></div>



<pre class='in'><code>prop.table(t2)</code></pre>



<div class='out'><pre class='out'><code>    
        No   Yes
  1  0.290 0.008
  2+ 0.678 0.024
</code></pre></div>



<pre class='in'><code>prop.table(t2, margin = 2)</code></pre>



<div class='out'><pre class='out'><code>    
            No       Yes
  1  0.2995868 0.2500000
  2+ 0.7004132 0.7500000
</code></pre></div>

See also `xtabs()`, `ftable()` or `CrossTable()` (in library `gmodels`).

### Exercise 2

From the lesson on `R` objects, you created a matrix based on the following:
Three hundred and thirty-three cows are milked by a milker wearing gloves and
280 by a milker not wearing gloves. Two hundred cows in each group developed a
mastitis. Using the `apply` family of functions, find out the risk ratio and
odds ratio for mastitis according to wearing gloves.

|           | Mastitis | No mastitis |
|:---------:|:--------:|:-----------:|
| Gloves    |: 200    :|: 133       :|
| No gloves |: 200    :|: 80        :|

### Solution



---

## Split-Apply-Combine in Action

The following figure can give you a sense of what is split-apply-combine.

![](split.png)

`plyr` package can be used to apply the split-apply-combine strategy. You need
to provide the following information:

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
