---
layout: lesson
root: ../..
---





# Reshaping Data

Sometimes you have multiple observations per subject, across time or for
different chacteristics of the subject. It is said the data is in "wide" format
if there's only one observation/row per subject with each
measurement/characteristic set as a different variable. It is in "long" format
if there's one observation/row per measurement (i.e. multiple rows per
subject). Reshaping data is like transposing rows and columns, switching from
wide to long format and vice versa.

### Exercise

Using the `prod` dataset which gives the milk production at each monthly tests
(`dim` = days in milk), what is its format?


<pre class='in'><code>prod <- read.csv(
    file = "prod.csv",
    header = TRUE,
    stringsAsFactors = FALSE
    )</code></pre>

We can go from wide to long format with `melt` function in the
`reshape2` package. You could do it with base `R` functions but `reshape2` has
simpler and more consistent functions for these operations.


<pre class='in'><code>library(reshape2)
prod.long <- melt(
    data = prod,
    id = "unique",
    measure.vars = 2:11,
    variable.name = "test",
    value.name = "milk"
    )

prod.long2 <- melt(
    data = prod,
    id = "unique",
    measure.vars = 12:21,
    variable.name = "test",
    value.name = "dim"
    )

# combining melt and dcast
prod.l1 <- melt(prod, id = "unique")
library(stringr)
prod.l1 <- transform(prod.l1, month = str_replace(variable, "^.*\\.", ""),
                    variable = str_replace(variable, "\\..*$", ""))
prod.l2 <- dcast(prod.l1, unique + month ~ variable)

# in 1 line with base::reshape
prod.l3 <- reshape(prod, dir = "long", varying = 2:21, sep = ".") </code></pre>

We can go from long to wide format with the `dcast` function.

### Exercise

Transform the health `dataset` from long to wide format.

### Solution


<pre class='in'><code>health.wide <- dcast(
    data = health,
    formula = unique + age + lactation + parity ~ disease,
    value.var = "presence"
    )</code></pre>

# String Manipulation

We can use the `stringr` package to manipulate strings. For example, `unique` is
made of the herd id and the cow id separated by a dash. If you want to break it
in two to get back each pieces of id you could:


<pre class='in'><code>library(stringr)
health.wide$id <- str_split_fixed(health.wide$unique, "-", 2)
health.wide$herd <- health.wide$id[, 1]
health.wide$cow <- health.wide$id[, 2]</code></pre>

See help for `stringr` to get all the possibilities. Note that you could also
use base `R` function to do this.


# Join

You might want to join the two datasets so that we could link diseases and milk
production. Base `R` has the `merge` function for this.


<pre class='in'><code>health.prod <- merge(health.wide, prod, by = "unique")</code></pre>

`plyr` library has also the `join` function to merge together data
frames. `plyr` has other interesting functions as well. For example, when you
`melt` the `prod` dataset, you noticed that `month` could be better
formatted. `plyr` has the `mutate` function to add or replace existing columns,
and it would be more interesting to order columns with `arrange` function:


<pre class='in'><code>library(plyr)
prod.long <- mutate(prod.long, test = sub("milk.", "", test))
prod.long2 <- mutate(prod.long2, test = sub("dim.", "", test))

prod.long <- join(prod.long, prod.long2, by = c("unique", "test"))

prod.long <- arrange(prod.long, unique, test)</code></pre>

## Exercise

Is it ordered by `id` and `month`? Why not?
