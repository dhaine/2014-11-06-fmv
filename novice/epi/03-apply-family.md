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



<div class='out'><pre class='out'><code>            [,1]       [,2]       [,3]       [,4]         [,5]       [,6]
 [1,]  1.1039629 -1.8813337 -2.0235359  2.3784165  1.048628257 -0.4727207
 [2,] -0.2670270 -0.3011534  0.3495604  1.7330323  1.369097993  0.7948813
 [3,] -0.2630512 -1.0641961  1.3388861  0.1653704  0.320397381  0.8497575
 [4,] -0.6106868  1.4564086  1.9007590  1.1935286 -0.000953237  1.8906938
 [5,] -0.5022711 -0.3561224 -0.2437827 -0.3434180 -1.809756928 -1.4478100
 [6,]  0.1865916 -0.2887361 -0.8200394  0.1376751  0.317486335 -2.2824146
 [7,]  1.3966022  0.8954594  0.9408750  0.3149835  0.814019888 -1.0756257
 [8,] -0.1201424 -0.8315681 -0.6896835 -0.6787529 -0.631961576 -0.8633955
 [9,] -0.7052724 -0.2699148 -0.5450394 -1.1930843 -0.223987426  2.0623110
[10,] -1.4885941 -1.0403063 -2.3532351  0.3173535  0.621146190  0.1812417
            [,7]        [,8]        [,9]      [,10]
 [1,]  0.8481980  1.46958590  1.91889385 -1.2901398
 [2,]  2.1923333 -0.82429450  2.03210086 -0.4925210
 [3,]  0.5637884  0.01678429  1.12163356  1.8373233
 [4,]  1.0579548  0.78066323 -0.06335956 -0.3473005
 [5,] -0.9092040  0.44446152 -0.07387800  0.7352516
 [6,]  1.7788934  1.30319420  0.38342130 -0.6316330
 [7,]  0.7104044  0.84031826  0.37465708  0.2443987
 [8,] -0.3147852 -0.63572310 -0.43734921  0.6323848
 [9,]  0.1337544  1.92657088  1.44643302 -0.3720186
[10,]  0.3853374  0.20305182 -0.96362423  0.9377529
</code></pre></div>



<pre class='in'><code>replicate(10, rnorm(10), simplify = TRUE)</code></pre>



<div class='out'><pre class='out'><code>            [,1]       [,2]       [,3]       [,4]        [,5]        [,6]
 [1,] -0.1063621 -0.2618855 -1.4913830  0.7762410  0.68813357 -1.44941802
 [2,]  0.5934308 -2.2958683  1.7536770  1.4450837 -0.44266541 -0.25625441
 [3,]  0.8061224 -1.0696297 -0.4704700 -1.5830817 -1.65029697  1.14825024
 [4,]  1.6105597  0.5695069  0.7546455  0.3508451  0.55616695  0.36435439
 [5,] -1.6522040 -0.3565153 -1.0944770 -1.2351141 -0.03938849 -0.09123491
 [6,]  0.6620682 -0.3483298 -1.5916672  0.6925862  0.30546930  0.45600510
 [7,]  0.1427958  0.6243872 -0.6466425  0.1270679 -1.76495373 -1.44956871
 [8,]  1.1560164 -0.3853759  1.0674628  0.4104996  0.42133663 -2.03192457
 [9,]  0.2396682  1.4457541  1.8810551  1.4858665  2.13339654  1.64005819
[10,] -0.1408372  0.4721200 -0.7885266  0.6153045  0.17389458  0.88051884
            [,7]       [,8]       [,9]        [,10]
 [1,]  0.9875313  1.5817264 -1.5688291  1.866241000
 [2,]  0.7370106 -0.4389475 -0.7394995  0.019344567
 [3,] -1.7792206  1.7531708 -0.3173816 -0.533125840
 [4,]  0.5405757 -0.7634928  1.4223852 -0.853650702
 [5,]  0.3438078  1.1917027  1.6222711  1.509361692
 [6,] -1.2938260 -2.1985441  0.3576113  0.008739449
 [7,] -1.2712306 -0.3879228  1.8564542 -0.088151221
 [8,]  0.7367151 -0.4408947  0.7979100  0.363433913
 [9,] -1.5206790 -0.2908593 -1.4445964  0.448856218
[10,] -0.8659685  0.3984848 -0.7826159  1.023057309
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


<div class='out'><pre class='out'><code>              Gloves No gloves
risk       0.6006006 0.7142857
risk.ratio 0.8408408 1.0000000
odds       1.5037594 2.5000000
odds.ratio 0.6015038 1.0000000
</code></pre></div>

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
