---
layout: lesson
root: ../..
---



## Answers to R lessons

### R Objects / Data Structures


<pre class='in'><code>health <- read.csv(
    file = "health.csv",
    header = TRUE,
    stringsAsFactors = FALSE)

str(health)
health$presence <- factor(health$presence, labels = c("No", "Yes"))
health$parity <- cut(health$lactation,
                     breaks = c(0, 1, max(health$lactation)),
                     labels = c("1", "2+"))</code></pre>


<pre class='in'><code>tab <- rbind(c(200, 133), c(200, 80))
rownames(tab) <- c("Gloves", "No gloves")
colnames(tab) <- c("Mastitis", "No mastitis")
tab
chisq.test(tab)</code></pre>

### Reshaping Data


<pre class='in'><code>prod <- read.csv(
    file = "prod.csv",
    header = TRUE,
    stringsAsFactors = FALSE
    )</code></pre>


<pre class='in'><code>health.wide <- dcast(
    data = health,
    formula = unique + age + lactation + parity ~ disease,
    value.var = "presence"
    )</code></pre>


<pre class='in'><code>rescale <- function(v, lower = 0, upper = 1) {
  # Rescales a vector, v, to lie in the range lower to upper.
  L <- min(v)
  H <- max(v)
  result <- (v - L) / (H - L) * (upper - lower) + lower
  return(result)
}
answer <- rescale(dat[, 4], lower = 2, upper = 5)
min(answer)
max(answer)
answer <- rescale(dat[, 4], lower = -5, upper = -2)
min(answer)
max(answer)</code></pre>

### Apply Family


<pre class='in'><code>by(prod.long$milk, prod.long[, "test"], function(x) mean(x, na.rm = TRUE))</code></pre>


<pre class='in'><code>tab <- rbind(c(200, 133), c(200, 80))
rownames(tab) <- c("Gloves", "No gloves")
colnames(tab) <- c("Mastitis", "No mastitis")

row.tot <- apply(tab, 1, sum)
risk <- tab[, "Mastitis"] / row.tot
risk.ratio <- risk / risk[2]
odds <- risk / (1 - risk)
odds.ratio <- odds / odds[2]
rbind(risk, risk.ratio, odds, odds.ratio)</code></pre>

### Graphics


<pre class='in'><code>ggplot(dairy, aes(mf, milk)) +
    geom_boxplot() +
    geom_jitter(aes(colour = parity), alpha = .5) +
    stat_summary(fun.y = mean, geom = "point", shape = 3, size = 4)</code></pre>
