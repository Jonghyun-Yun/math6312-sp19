+++
title = "HW 1"
author = ["Jonghyun Yun"]
lastmod = 2020-04-04T20:05:45-05:00
categories = ["teaching"]
draft = false
toc = true
type = "docs"
comments = false
[menu."math6312-sp19"]
  identifier = "hw-1"
  parent = "Homework"
  weight = 10
+++

---

-   Create a R markdown document to include your R codes and results. Submit both R markdown and rendered PDF documents to Blackboard.
-   Every files you submit should have a file name in the following format: `WorkType#.FirstName.LastName`. For example, `HW7.Jonghyun.Yun.Rmd`.
-   Show all your work to receive full credit for a problem. You should not include unnecessary code chunks and results that are not necessary to draw the conclusion. An unnecessarily lengthy solution will be penalized.

---


## Question 1 {#question-1}


### a) {#a}

Use R to create the following two matrices and do the indicated matrix multiplication. What is the resulting matrix? \\[ \left[ {\begin{array}{\*{20}{c}} {\begin{array}{\*{20}{c}} 7&9&{20} \end{array}}\\\\\\ {\begin{array}{\*{20}{c}} 2&4&{10} \end{array}} \end{array}} \right] \times \left[ {\begin{array}{\*{20}{c}} {\begin{array}{\*{20}{c}} 1&7&{12}&{40} \end{array}}\\\\\\ {\begin{array}{\*{20}{c}} 2&8&{13}&{20} \end{array}}\\\\\\ {\begin{array}{\*{20}{c}} 3&9&{14}&{21} \end{array}} \end{array}} \right] \\]


### b) {#b}

Let \\[A = \left( {\begin{array}{\*{20}{c}} 2&1&0 \\\\\\ 1&2&0 \\\\\\ 0&0&2 \end{array}} \right).\\] Find a 3 by 3 matrix \\(G\\) such that \\(A = G^2\\) (_hint_: use the spectral decomposition.)


### c) {#c}

Reproduce the equation below using LaTeX Math in R Markdown. \\[ \Phi(x) = \int\_{-\infty}^{x} \frac{1}{\sqrt{2 \pi }} e^{- \frac{z^{2}}{2}} \, dz \\]


## Question 2 {#question-2}

The distances between pairs of five items are as follows:

\begin{array}{\*{20}{c}}
  {}&{\begin{array}{\*{20}{c}}
  1&2&3&4&5
\end{array}} \\\\\\
  {\begin{array}{\*{20}{c}}
  1 \\\\\\
  2 \\\\\\
  3 \\\\\\
  4 \\\\\\
  5
\end{array}}&{\left[ {\begin{array}{\*{20}{c}}
  0&{}&{}&{}&{} \\\\\\
  4&0&{}&{}&{} \\\\\\
  6&9&0&{}&{} \\\\\\
  1&7&{10}&0&{} \\\\\\
  6&3&5&8&0
\end{array}} \right]}
\end{array}

Cluster the five items using the single, complete and average linkage hierarchical methods. Draw dendrograms and compare the results.


## Question 3 {#question-3}

The [`cereals`]({{< relref "math6312-data" >}}) data set lists measurements on 8 variables for 43 breakfast cereals (drop `Manufacturer` and `Group`).


### a) {#a}

Calculate the Euclidean distances between pairs of cereal brands (provide the relevant R code, not the results).


### b) {#b}

Treating the distances in a) as dissimilarity measure, cluster the cereals using the single and centroid linkage hierarchical procedures. Construct dendrograms and compare the results.


## Question 4 {#question-4}

The [`cereals`]({{< relref "math6312-data" >}}) data set lists measurements on 8 variables for 43 breakfast. Apply a K=means method to the [`cereals`]({{< relref "math6312-data" >}}) data. Cluster the cereals into \\(K=2,3\\) groups using 8 variables (except `Manufacturer` and `Group`). Compare the results with those in Question 3.


## Question 5 {#question-5}

Generate 10,000 random numbers from the standard normal distribution using `set.seed(1); x = rnorm(10000)`.


### a) {#a}

Count how may values in `x` is between -2 and 2.


### b) {#b}

Find the index of `x` whose value is \\(\ge 3\\) or \\(\le -3\\).


### c) {#c}

Create a histogram of `x`. Draw a dotted vertical line in the plot to denote the median of `x`.


## Question 6 {#question-6}

The [`cereals`]({{< relref "math6312-data" >}}) data set lists measurements on 8 variables for 43 breakfast.


### a) {#a}

Suppose cereals with high protein (\\(\ge 3)\\) and low sugar (\\(\le 9\\)) are considered "healthy". Use the R to calculate mean calories of "healthy" cereals in each manufacturer.


### b) {#b}

Create a new variables, `Healthy` in the [`cereals`]({{< relref "math6312-data#cereals" >}}) data set. A value of this variable should be 1 if cereal is healthy; 0 otherwise.
