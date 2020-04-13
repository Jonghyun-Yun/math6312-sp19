+++
title = "Lab 3: R basics"
author = ["Jonghyun Yun"]
lastmod = 2020-04-04T20:11:05-05:00
tags = ["R tutorial"]
categories = ["teaching", "math6312"]
draft = false
type = "docs"
toc = true
[menu."math6312-sp19"]
  identifier = "lab-3-r-basics"
  parent = "R Lab"
  weight = 30
+++



---


## R as a calculator {#r-as-a-calculator}


### Basic operations {#basic-operations}

<a id="code-snippet--basic op"></a>

```r
2 + 2 # add numbers
```

```
## [1] 4
```

```r
2 * pi # multiply by a constant
```

```
## [1] 6.283185
```

```r
3^4 # powers
```

```
## [1] 81
```

```r
sqrt(4^2) # functions
```

```
## [1] 4
```

```r
log(100, base = 10)
```

```
## [1] 2
```

```r
log(exp(1))
```

```
## [1] 1
```


### Functions in statistics {#functions-in-statistics}


```r
7 + runif(1) # add a Uniform(0,1) random number
```

```
## [1] 7.476541
```

```r
pnorm(1.96,0,1) # standard normal cdf
```

```
## [1] 0.9750021
```

```r
rnorm(5) # generate 5 random numbers from N(0,1)
```

```
## [1] -2.3523905 -1.1861995 -0.9536634  0.7676184 -1.0534510
```


## Assigning values to R objects {#assigning-values-to-r-objects}

A key action in R is to store values in the form of R objects, and to examine the value of R objects.


```r
val = 3
val
```

```
## [1] 3
```

```r
print(val)
```

```
## [1] 3
```

```r
# assign value to two objects
a = b = 3
a
```

```
## [1] 3
```

```r
b
```

```
## [1] 3
```

```r
Val = 7 # case-sensitive!
print(c(val, Val))
```

```
## [1] 3 7
```


```r
mySeq = 1:6
mySeq
```

```
## [1] 1 2 3 4 5 6
```

```r
myOtherSeq = seq(1.1, 11.1, by = 2)
myOtherSeq
```

```
## [1]  1.1  3.1  5.1  7.1  9.1 11.1
```

```r
length(myOtherSeq)
```

```
## [1] 6
```

```r
fours = rep(4, 6)
fours
```

```
## [1] 4 4 4 4 4 4
```

```r
# This is a comment: here is an example of non-npnorm(-1.96,0,1) -umeric data
depts = c('espm', 'pmb', 'stats')
depts
```

```
## [1] "espm"  "pmb"   "stats"
```

saved it for later use. R gives us a lot of flexibility (within certain rules) for assigning to (parts of) objects from (parts of) other objects.


## How to be [lazy](http://dilbert.com/strips/comic/2005-05-29/) {#how-to-be-lazy}

If you're starting to type something you've typed before, or the long name of an R object or function, STOP! You likely don't need to type all of that.

-   Tab completion
-   Command history
    -   up/down arrows
    -   Ctrl-{up arrow} or Command-{up arrow}

-   R Studio: select a line or block for execution
-   Put your code in a file and use `source()`. For example: `source('MyScript.R')`


## Vectors in R {#vectors-in-r}

The most basic form of an R object is a vector. In fact, individual (scalar) values are vectors of length one.

We can concatenate values into a vector with `c()`.


```r
# numeric vector
nums = c(1.1, 3, -5.7)
devs = rnorm(5)
devs
```

```
## [1]  0.17196003 -0.15259904 -0.08815965  1.21777177  1.03509825
```

```r
# logical vector
bools = c(TRUE, FALSE, TRUE)
bools
```

```
## [1]  TRUE FALSE  TRUE
```


## Working with indices and subsets {#working-with-indices-and-subsets}


```r
vals = seq(2, 12, by = 2)
vals
```

```
## [1]  2  4  6  8 10 12
```

```r
vals[3]
```

```
## [1] 6
```

```r
vals[3:5]
```

```
## [1]  6  8 10
```

```r
vals[c(1, 3, 6)]
```

```
## [1]  2  6 12
```

```r
vals[-c(1, 3, 6)]
```

```
## [1]  4  8 10
```

```r
vals[c(rep(TRUE, 3), rep(FALSE, 2), TRUE)]
```

```
## [1]  2  4  6 12
```

```r
# list values >= 5
vals[vals >= 5]
```

```
## [1]  6  8 10 12
```

```r
# list index whose value = 4
which(vals ==  4)
```

```
## [1] 2
```


## Vectorized calculations and comparisons {#vectorized-calculations-and-comparisons}

At the core of R is the idea of doing calculations on entire vectors.


```r
vec1 = sample(1:5, 10, replace = TRUE)
vec2 = sample(1:5, 10, replace = TRUE)
vec1
```

```
##  [1] 1 1 5 4 4 4 5 3 3 2
```

```r
vec2
```

```
##  [1] 1 2 2 5 1 1 2 2 4 4
```

```r
vec1 + vec2
```

```
##  [1] 2 3 7 9 5 5 7 5 7 6
```

```r
vec1^vec2
```

```
##  [1]    1    1   25 1024    4    4   25    9   81   16
```

```r
vec1 >= vec2
```

```
##  [1]  TRUE FALSE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE
```

```r
vec1 <= 3
```

```
##  [1]  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE
```

```r
# using 'or'
vec1 <= 0 | vec1 >= 3
```

```
##  [1] FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
```

```r
# using 'and'
vec1 <= 0 & vec1 >= vec2
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```

```r
vec1 == vec2
```

```
##  [1]  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```

```r
vec1 != vec2
```

```
##  [1] FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
```

```r
# careful:
vec1 = vec2
identical(vec1, vec2)
```

```
## [1] TRUE
```

An important related concept is that of recycling


```r
vec3 = sample(1:5, 5, replace = TRUE)
vec4 = sample(1:5, 3, replace = TRUE)
vec1
```

```
##  [1] 1 2 2 5 1 1 2 2 4 4
```

```r
vec3
```

```
## [1] 3 3 3 3 5
```

```r
vec4
```

```
## [1] 1 2 3
```

```r
vec1 + vec3
```

```
##  [1] 4 5 5 8 6 4 5 5 7 9
```

```r
vec1 + vec4
```

```
##  [1] 2 4 5 6 3 4 3 4 7 5
```

**Question**: Tell me what's going on. What choices were made by the R developers?


## R is a functional language {#r-is-a-functional-language}

-   Operations are carried out with functions. Functions take objects as inputs and return objects as outputs.
-   An analysis can be considered a pipeline of function calls, with output from a function used later in a subsequent operation as input to another function.
-   We can embed function calls:

<!--listend-->


```r
set.seed(1) # set a random seed, so your random numbers won't be different when you generate them next time.
hist(rnorm(1000))
```

<img src="/courses/math6312-sp19/lab3_files/figure-html/unnamed-chunk-8-1.png" width="672" style="display: block; margin: auto;" />


## Getting help about a function {#getting-help-about-a-function}

To get information about a function you know exists, use `help` or `?`, e.g., `?lm`. For information on a general topic, use `apropos` or `??`


## Basic kinds of R objects {#basic-kinds-of-r-objects}

Vectors are not the only kinds of R objects.


#### Vectors {#vectors}

Vectors of various types (numeric (i.e., decimal/floating point/double), integer, boolean, character), all items must be of the same type


#### Matrices {#matrices}

Matrices of various types, all items must be of the same type


```r
mat = matrix(c(1,0,5,2,1,6,3,4,0),3,3)
t(mat) %*% mat
```

```
##      [,1] [,2] [,3]
## [1,]   26   32    3
## [2,]   32   41   10
## [3,]    3   10   25
```

```r
dim(mat)
```

```
## [1] 3 3
```

```r
# * is element-wise product
t(mat) * mat
```

```
##      [,1] [,2] [,3]
## [1,]    1    0   15
## [2,]    0    1   24
## [3,]   15   24    0
```


#### Some linear algera {#some-linear-algera}


```r
# vector without declaration of column/row vector
z = c(1,2,3)

# column vector 3 by 1
x = matrix(c(1,2,3),3,1)
x
```

```
##      [,1]
## [1,]    1
## [2,]    2
## [3,]    3
```

```r
# transpose
t(x)
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
```

```r
# row vector 1 by 3
y = matrix(c(3,2,1),1,3)
y
```

```
##      [,1] [,2] [,3]
## [1,]    3    2    1
```

```r
# sum
c(y %*% rep(1,length(y)))
```

```
## [1] 6
```

```r
sum(y)
```

```
## [1] 6
```

```r
# L-2 norm of a vector
norm(x,"f")
```

```
## [1] 3.741657
```

```r
# inner product
t(x) %*% x
```

```
##      [,1]
## [1,]   14
```

```r
z %*% z
```

```
##      [,1]
## [1,]   14
```

```r
# outer product
x %*% t(x)
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]    2    4    6
## [3,]    3    6    9
```

```r
z %o% z
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]    2    4    6
## [3,]    3    6    9
```

```r
# recycling mat
mat
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]    0    1    4
## [3,]    5    6    0
```

```r
# diagonal of matrix
diag(mat)
```

```
## [1] 1 1 0
```


```r
# determinant of matrix
det(mat)
```

```
## [1] 1
```

```r
# trace of matrix
sum(diag(mat))
```

```
## [1] 2
```

```r
# positive definite symmetric matrix A
A = t(mat) %*% mat
A
```

```
##      [,1] [,2] [,3]
## [1,]   26   32    3
## [2,]   32   41   10
## [3,]    3   10   25
```

```r
# inverse
solve(A)
```

```
##      [,1] [,2] [,3]
## [1,]  925 -770  197
## [2,] -770  641 -164
## [3,]  197 -164   42
```

```r
solve(A) %*% A
```

```
##               [,1]          [,2]          [,3]
## [1,]  1.000000e+00  1.534772e-12 -2.557954e-13
## [2,] -9.947598e-13  1.000000e+00  1.989520e-13
## [3,] -2.131628e-13 -3.126388e-13  1.000000e+00
```

```r
A %*% solve(A)
```

```
##              [,1]          [,2]          [,3]
## [1,] 1.000000e+00 -1.080025e-12 -2.131628e-13
## [2,] 1.818989e-12  1.000000e+00 -2.842171e-14
## [3,] 4.547474e-13 -5.115908e-13  1.000000e+00
```

**Question**: Why `solve(A) %*% A` does not produce the identity matrix?


```r
# eigenvalues and eigenvector
myeigen = eigen(A)

# spectral decomposition
D = diag(myeigen$values)
D
```

```
##          [,1]    [,2]         [,3]
## [1,] 68.53918  0.0000 0.0000000000
## [2,]  0.00000 23.4602 0.0000000000
## [3,]  0.00000  0.0000 0.0006219127
```

```r
C = myeigen$vectors
C %*% D %*% t(C)
```

```
##      [,1] [,2] [,3]
## [1,]   26   32    3
## [2,]   32   41   10
## [3,]    3   10   25
```

```r
# see if A = CDC'
C %*% D %*% t(C) == A
```

```
##       [,1]  [,2]  [,3]
## [1,]  TRUE FALSE FALSE
## [2,] FALSE FALSE FALSE
## [3,] FALSE FALSE FALSE
```

```r
# square root of A
C %*% sqrt(D) %*% t(C)
```

```
##            [,1]     [,2]       [,3]
## [1,]  3.2935597 3.889999 -0.1427347
## [2,]  3.8899989 4.971968  1.0711855
## [3,] -0.1427347 1.071185  4.8818222
```

```r
# it is not sqrt(A)
sqrt(A)
```

```
##          [,1]     [,2]     [,3]
## [1,] 5.099020 5.656854 1.732051
## [2,] 5.656854 6.403124 3.162278
## [3,] 1.732051 3.162278 5.000000
```


#### More linear algebra {#more-linear-algebra}


```r
X = t(C %*% sqrt(D) %*% matrix(rnorm(100*3),3,100))
head(X) # first few rows of X
```

```
##           [,1]      [,2]         [,3]
## [1,] -7.035462 -7.617190   3.12659974
## [2,] -1.160273 -1.344615  -0.06440313
## [3,] -1.593771 -4.522554 -10.38579507
## [4,] -4.244371 -6.197340  -4.32757133
## [5,]  9.861545 10.853130  -3.82817762
## [6,]  1.589210  1.335829  -2.44649186
```

```r
cov(X) # sample covariance matirx
```

```
##           [,1]     [,2]      [,3]
## [1,] 27.308007 33.91697  4.379755
## [2,] 33.916971 43.75008 11.802931
## [3,]  4.379755 11.80293 25.653086
```

```r
# singular value decomposition of X
mysvd <- svd(X)
mysvd$d
```

```
## [1] 85.6217244 48.0160946  0.2709681
```

```r
D = diag(mysvd$d[1:2])
X_recon = mysvd$u[,1:2] %*% D %*% t(mysvd$v[,1:2]) # reconstruct X using 2-dim basis
cov(X_recon)
```

```
##           [,1]     [,2]      [,3]
## [1,] 27.308319 33.91747  4.379772
## [2,] 33.917470 43.74904 11.803077
## [3,]  4.379772 11.80308 25.653080
```


#### Lists {#lists}

Collections of disparate or complicated objects


```r
myList = list(stuff = 3, mat = matrix(1:4, nrow = 2),
   moreStuff = c("china", "japan"), list(5, "bear"))
myList
```

```
## $stuff
## [1] 3
## 
## $mat
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## $moreStuff
## [1] "china" "japan"
## 
## [[4]]
## [[4]][[1]]
## [1] 5
## 
## [[4]][[2]]
## [1] "bear"
```

```r
myList[[1]] # result is not (usually) a list (unless you have nested lists)
```

```
## [1] 3
```

```r
identical(myList[[1]], myList$stuff)
```

```
## [1] TRUE
```

```r
myList$moreStuff[2]
```

```
## [1] "japan"
```

```r
myList[[4]][[2]]
```

```
## [1] "bear"
```

```r
myList[1:3] # subset of a list is a list
```

```
## $stuff
## [1] 3
## 
## $mat
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## $moreStuff
## [1] "china" "japan"
```

```r
myList$newOne = 'more weird stuff'
names(myList)
```

```
## [1] "stuff"     "mat"       "moreStuff" ""          "newOne"
```

Lists can be used as vectors of complicated objects. E.g., suppose you have a linear regression for each value of a stratifying variable. You could have a list of regression fits. Each regression fit will itself be a list, so you'll have a list of lists.


#### R data frame {#r-data-frame}

A data frame is used for storing data tables. It is a list of vectors of equal length. Some R functions work properly only on the data frame.


```r
df = data.frame(X) # turning a matrix into a data frame
colnames(df) = c("y","x1","x2") # assigning variable names (column names)
head(df)
```

```
##           y        x1           x2
## 1 -7.035462 -7.617190   3.12659974
## 2 -1.160273 -1.344615  -0.06440313
## 3 -1.593771 -4.522554 -10.38579507
## 4 -4.244371 -6.197340  -4.32757133
## 5  9.861545 10.853130  -3.82817762
## 6  1.589210  1.335829  -2.44649186
```

```r
head(df$x1) # access variable by name
```

```
## [1] -7.617190 -1.344615 -4.522554 -6.197340 10.853130  1.335829
```

```r
df$group = sample(1:2,100,T) # randomly assign into two groups
aggregate(df[,-4], list(df$group), mean) # group means
```

```
##   Group.1         y         x1          x2
## 1       1  0.241162  0.3170858  0.09947586
## 2       2 -1.271514 -1.5735235 -0.18532985
```

```r
aggregate(df[,-4], list(df$group), var) # group variances
```

```
##   Group.1        y       x1       x2
## 1       1 19.43567 32.53132 24.87290
## 2       2 34.88598 54.48683 26.95799
```

A data frame can contain variables in different types.


```r
age = c(32,34,23)
sex = c("M","F","M")
healthy = c(TRUE, FALSE, FALSE)

dat = data.frame(age,sex,healthy)
dat
```

```
##   age sex healthy
## 1  32   M    TRUE
## 2  34   F   FALSE
## 3  23   M   FALSE
```


## R for programming {#r-for-programming}


### For-loop {#for-loop}

The for-loop iterates commands over a set of indexes. The below is to calculate the sum and product of `x`. The `tot` and `prd` should be properly pre-defined before the for-loop below.


```r
x = rnorm(100)
tot = 0
prd = 1
for (i in 1:length(x)){
  tot = tot + x[i]
  prd = prd * x[i]
}
tot
```

```
## [1] -2.012583
```

```r
prd
```

```
## [1] -7.533576e-26
```

The R for-loop can be iterated over any set of R objects.


```r
for (j in c(1,2,-1,sqrt(2))){
  print(j)
  }
```

```
## [1] 1
## [1] 2
## [1] -1
## [1] 1.414214
```


```r
for (j in c("apple", "banana", "orange")){
  print(j)
  }
```

```
## [1] "apple"
## [1] "banana"
## [1] "orange"
```


### Function {#function}

An R function takes R objects as inputs, and return an R object (vector, matrix, plot, list, etc). Let's see a simple example.


```r
square = function(x){
  y = x * x
  return(y)
}
square(2)
```

```
## [1] 4
```

In the above, local variables `x` and `y` are declared inside a function, and they can be accessed only inside that function.


```r
identical(y, 4)
```

```
## [1] FALSE
```

In the meanwhile, global variables are declared outside any function, and they can be accessed (used) out/inside any function. However, using global variables inside a function may be a bad programming practice since global variables can be changed easily by any function or R console.


```r
g.var = 20
div20 = function(x){
out = x / g.var
return(out)
}
div20(20)
```

```
## [1] 1
```


```r
g.var = 10
div20(20)
```

```
## [1] 2
```

To void this issue, a local `g.var` variable is defined in the function `div20()`. Observe that global and local variables have the same name, but different values.


```r
div20 = function(x){
g.var = 20
out = x / g.var
print(g.var)
return(out)
}
div20(20)
```

```
## [1] 20
```

```
## [1] 1
```


```r
g.var
```

```
## [1] 10
```

You can also return a vector output from a function.


```r
myfun1 = function(x,y) {
  prod = x * y
  sum = x + y
    return(c(prod,sum))
}
myfun1(2,3)
```

```
## [1] 6 5
```

An R function can return a list object. This is very useful when you have different types of variables as output.


```r
myfun2 = function(x,y) {
  prod = x * y
  sum = x + y
  out = list(prod,sum)
  names(out) = c("prod","sum")
  return(out)
}
ans = myfun2(2,3)
ans
```

```
## $prod
## [1] 6
## 
## $sum
## [1] 5
```


## (optional) R data.table {#optional--r-data-dot-table}

<kbd>data.frame</kbd> is part of base R, and <kbd>data.table</kbd> is a package that extends <kbd>data.frame</kbd>. Two of its most notable features are speed and cleaner syntax. However, that syntax is different from the standard R syntax for <kbd>data.frame</kbd>. You may enjoy great performance of <kbd>data.table</kbd> after the relatively steep learning curve.


```r
library(data.table)
dat = data.table(age, sex, healthy)
class(dat)
```

```
## [1] "data.table" "data.frame"
```

```r
dat
```

```
##    age sex healthy
## 1:  32   M    TRUE
## 2:  34   F   FALSE
## 3:  23   M   FALSE
```

The data manipulation using <kbd>data.table</kbd> has a general form: <kbd>DT[i, j, by]</kbd>, which means "Take <kbd>DT</kbd>, subset rows using <kbd>i</kbd>, then calculate <kbd>j</kbd> grouped by <kbd>by</kbd>"


```r
diamondsDT <- data.table(ggplot2::diamonds)
diamondsDT
```

```
##        carat       cut color clarity depth table price    x    y    z
##     1:  0.23     Ideal     E     SI2  61.5    55   326 3.95 3.98 2.43
##     2:  0.21   Premium     E     SI1  59.8    61   326 3.89 3.84 2.31
##     3:  0.23      Good     E     VS1  56.9    65   327 4.05 4.07 2.31
##     4:  0.29   Premium     I     VS2  62.4    58   334 4.20 4.23 2.63
##     5:  0.31      Good     J     SI2  63.3    58   335 4.34 4.35 2.75
##    ---                                                               
## 53936:  0.72     Ideal     D     SI1  60.8    57  2757 5.75 5.76 3.50
## 53937:  0.72      Good     D     SI1  63.1    55  2757 5.69 5.75 3.61
## 53938:  0.70 Very Good     D     SI1  62.8    60  2757 5.66 5.68 3.56
## 53939:  0.86   Premium     H     SI2  61.0    58  2757 6.15 6.12 3.74
## 53940:  0.75     Ideal     D     SI2  62.2    55  2757 5.83 5.87 3.64
```

```r
diamondsDT[
  cut != "Fair", # subset of rows with no Fair cut
  .(AvgPrice = mean(price), # Returns the mean and median of column price and # of cases as data.table
    MedianPrice = as.numeric(median(price)),
    Count = .N
  ),
  by = cut # for every group in cut
][
  order(-Count) # chaining
]
```

```
##          cut AvgPrice MedianPrice Count
## 1:     Ideal 3457.542      1810.0 21551
## 2:   Premium 4584.258      3185.0 13791
## 3: Very Good 3981.760      2648.0 12082
## 4:      Good 3928.864      3050.5  4906
```


## Pipes to chain multiple operations {#pipes-to-chain-multiple-operations}

Pipes are a powerful tool for clearly expressing a sequence of multiple operations. The pipe, <kbd>%>%</kbd>, comes from the <kbd>magrittr</kbd> package. Packages in the <kbd>tidyverse</kbd> load <kbd>%>%</kbd> for you automatically, so you don’t usually load <kbd>magrittr</kbd> explicitly.


```r
library(tidyverse)
```

-   nesting: The below is code to generate 100 standard normal random numbers, arrange them into 50 by 2 matrix, and plot the matrix.

<!--listend-->


```r
plot(matrix(rnorm(100), ncol=2))
```

<img src="/courses/math6312-sp19/lab3_files/figure-html/unnamed-chunk-31-1.png" width="672" style="display: block; margin: auto;" />

-   chaining: The code below produces the same results as the above.

<!--listend-->


```r
rnorm(100) %>%
  matrix(ncol = 2) %>%
  plot
```

<img src="/courses/math6312-sp19/lab3_files/figure-html/unnamed-chunk-32-1.png" width="672" style="display: block; margin: auto;" />

Some people prefer chaining to nesting because the functions applied can be read from left to right rather than from inside out.


### Data processing using `data.frame` vs `data.table` {#data-processing-using-data-dot-frame-vs-data-dot-table}

<kbd>dplyr</kbd> is a grammar of data manipulation (say, you are tidying your data), providing a consistent set of verbs that help you solve the most common data manipulation challenges. The package comes with <kbd>tidyverse</kbd>, so again you don't need to load it explicitly.


```r
diamondsDT <- data.table(ggplot2::diamonds)
diamondsDF <- tbl_df(ggplot2::diamonds)

diamondsDT
```

```
##        carat       cut color clarity depth table price    x    y    z
##     1:  0.23     Ideal     E     SI2  61.5    55   326 3.95 3.98 2.43
##     2:  0.21   Premium     E     SI1  59.8    61   326 3.89 3.84 2.31
##     3:  0.23      Good     E     VS1  56.9    65   327 4.05 4.07 2.31
##     4:  0.29   Premium     I     VS2  62.4    58   334 4.20 4.23 2.63
##     5:  0.31      Good     J     SI2  63.3    58   335 4.34 4.35 2.75
##    ---                                                               
## 53936:  0.72     Ideal     D     SI1  60.8    57  2757 5.75 5.76 3.50
## 53937:  0.72      Good     D     SI1  63.1    55  2757 5.69 5.75 3.61
## 53938:  0.70 Very Good     D     SI1  62.8    60  2757 5.66 5.68 3.56
## 53939:  0.86   Premium     H     SI2  61.0    58  2757 6.15 6.12 3.74
## 53940:  0.75     Ideal     D     SI2  62.2    55  2757 5.83 5.87 3.64
```

```r
diamondsDF
```

```
## # A tibble: 53,940 x 10
##    carat cut       color clarity depth table price     x     y     z
##    <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
##  1 0.23  Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
##  2 0.21  Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
##  3 0.23  Good      E     VS1      56.9    65   327  4.05  4.07  2.31
##  4 0.290 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
##  5 0.31  Good      J     SI2      63.3    58   335  4.34  4.35  2.75
##  6 0.24  Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
##  7 0.24  Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
##  8 0.26  Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
##  9 0.22  Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
## 10 0.23  Very Good H     VS1      59.4    61   338  4     4.05  2.39
## # … with 53,930 more rows
```

```r
diamondsDT[
  cut != "Fair",
  .(AvgPrice = mean(price),
    MedianPrice = as.numeric(median(price)),
    Count = .N
  ),
  by = cut
][
  order(-Count)
]
```

```
##          cut AvgPrice MedianPrice Count
## 1:     Ideal 3457.542      1810.0 21551
## 2:   Premium 4584.258      3185.0 13791
## 3: Very Good 3981.760      2648.0 12082
## 4:      Good 3928.864      3050.5  4906
```

```r
diamondsDF %>%
  filter(cut != "Fair") %>%
  group_by(cut) %>%
  summarize(
    AvgPrice = mean(price),
    MedianPrice = as.numeric(median(price)),
    Count = n()
  ) %>%
  arrange(desc(Count))
```

```
## # A tibble: 4 x 4
##   cut       AvgPrice MedianPrice Count
##   <ord>        <dbl>       <dbl> <int>
## 1 Ideal        3458.       1810  21551
## 2 Premium      4584.       3185  13791
## 3 Very Good    3982.       2648  12082
## 4 Good         3929.       3050.  4906
```

In general, `data.table` is faster and more memory efficient then `dplyr`. However, you may not notice the speed difference unless you are a very large data set. `dplyr` has more accessible syntax (easy to write/read). `data.table` is a comprehensive, efficient, self-contained package. It has its own DSL, it has a very fast file reader, etc. The DSL is succint as well: you can do a lot even in a single line of code. And I believe it is currently the fastest data open source manipulation tool for data that fit in memory (see the benchmark at Rdatatable/data.table, although since then all three packages have implemented some very substantial performance improvements), and toward that goal, it provides functionality that is occasionally incredibly useful (like in-place modification of tables). data.table is also memory-efficient, and works well on very large data sets. data.table is written in C for the most part. It truly is quite an amazing piece of software. It feels exhilaratingly fast.

Conversely, dplyr emphasizes the virtues of function composition and data layer abstraction. Because of composition, it is happy to be complemented by functions provided by other packages (Hadley himself has written readr and tidyr for file reading and long/wide operations, for example). Because of built-in abstraction, dplyr allows you to munge data from several DBs using the same syntax; in this respect it has some similarity to Python's Blaze project. Finally, dplyr's code is made more efficient by the use of C++.


## Do and Don't {#do-and-don-t}


### Read a file in a local directory {#read-a-file-in-a-local-directory}

-   Avoid using an absolute path.


#### Don't {#don-t}

<a id="code-snippet--read1,eval=F"></a>

```r
df = fread("/path/to/local/directory/data/filename.extension")
```


#### Do {#do}

<a id="code-snippet--read2,eval=F"></a>

```r
setwd("/path/to/local/directory/")
df = fread("data/filename.extension")
```


### Accessing variables in list() or data.frame() {#accessing-variables-in-list-or-data-dot-frame}

-   Avoid using <kbd>attach()</kbd>. There are some possibilities for creating errors when using <kbd>attach()</kbd>.


#### Don't {#don-t}


```r
data(iris)
attach(iris)
plot(Sepal.Length, Sepal.Width)
```

<img src="/courses/math6312-sp19/lab3_files/figure-html/unnamed-chunk-34-1.png" width="672" style="display: block; margin: auto;" />

```r
detach(iris)
```


#### Do {#do}


```r
data(iris)
plot(iris$Sepal.Length, iris$Sepal.Width)
```

<img src="/courses/math6312-sp19/lab3_files/figure-html/unnamed-chunk-35-1.png" width="672" style="display: block; margin: auto;" />


```r
data(iris)
with(iris, plot(Sepal.Length, Sepal.Width))
```

<img src="/courses/math6312-sp19/lab3_files/figure-html/unnamed-chunk-36-1.png" width="672" style="display: block; margin: auto;" />


```r
data(iris)
iris %>% with(plot(Sepal.Length, Sepal.Width))
```

<img src="/courses/math6312-sp19/lab3_files/figure-html/unnamed-chunk-37-1.png" width="672" style="display: block; margin: auto;" />


#### Bad use of <kbd>attach()</kbd> {#bad-use-of-attach}


```r
x = c(0,0,0)
df = data.frame(x = c(1,1,1))
x
```

```
## [1] 0 0 0
```

```r
df$x
```

```
## [1] 1 1 1
```

```r
attach(df)
x
```

```
## [1] 0 0 0
```
