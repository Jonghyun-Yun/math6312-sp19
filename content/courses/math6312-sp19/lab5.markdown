+++
title = "Clustering and Multidimensional scaling"
author = ["Jonghyun Yun"]
lastmod = 2020-04-13T06:36:20-05:00
tags = ["clustering", "MDS"]
categories = ["teaching", "math6312"]
draft = false
type = "docs"
toc = true
[menu."math6312-sp19"]
  identifier = "clustering-and-multidimensional-scaling"
  parent = "R Lab"
  weight = 50
  name = "Lab 5: Clustering and Multidimensional scaling"
+++

<a id="code-snippet--global-option,include=F"></a>


---


## Prerequisites {#prerequisites}

You need to install `genefilter`. Run the below once in your R console.


```r
source('http://bioconductor.org/biocLite.R')
biocLite("genefilter") # dependency in clusterSim
```

For Mac users, it is required to have [XQuartz](https://www.xquartz.org/) to fully utilize the code in this lab.

There are lots of packages needed to compile this document. The packages that have not been installed in your R library will be installed, and then all required packages will be loaded using the following code:

<a id="code-snippet--load"></a>

```r
ipak = function(pkg){
    new.pkg = pkg[!(pkg %in% installed.packages()[, "Package"])]
    if (length(new.pkg))
        install.packages(new.pkg, dependencies = TRUE)
    sapply(pkg, require, character.only = TRUE)
}

#pkg = search() # list loaded packages
#pkg = gsub('[A-z ]*:', '' , pkg)
#paste(pkg, collapse="','")

pkg = c('lle','rgl','plotrix','gplots','clusterSim','kernlab','scatterplot3d','ggplot2','colorspace','cluster','clusterGenomics','calibrate','fastcluster','animation','vegan','vegan3d')
ipak(pkg)
```

```
##             lle             rgl         plotrix          gplots 
##            TRUE            TRUE            TRUE            TRUE 
##      clusterSim         kernlab   scatterplot3d         ggplot2 
##            TRUE            TRUE            TRUE            TRUE 
##      colorspace         cluster clusterGenomics       calibrate 
##            TRUE            TRUE            TRUE            TRUE 
##     fastcluster       animation           vegan         vegan3d 
##            TRUE            TRUE            TRUE            TRUE
```

```r
knitr::knit_hooks$set(webgl = hook_webgl) ## to embed interative graphics into HTML

cwurl = 'https://math6312.rbind.io/data'
bburl = 'https://math6312.rbind.io/courses/math6312-sp19/data'
```


## K-means {#k-means}

An artificial data set is simulated. The two-dimensional data contains two clusters. True cluster centers are (0,0) and (1,1). We use the k-means to this data set.


```r
x <- rbind(matrix(rnorm(100, sd = 0.3), ncol = 2),
           matrix(rnorm(100, mean = 1, sd = 0.3), ncol = 2))
colnames(x) <- c("x", "y")
cl <- kmeans(x, centers = 2, iter.max = 10, nstart = 10)
plot(x, col = cl$cluster)
points(cl$centers, col = 1:2, pch = 8, cex = 2)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-2-1.png" width="1152" style="display: block; margin: auto;" />


## Hierarchical clustering {#hierarchical-clustering}

We simulate six samples from uniform and apply the hierarchical clustering with single linkage.


```r
set.seed(5)
x.1 = runif(6)
x.2 = runif(6)
dat = data.frame(x.1,x.2)

par(bty = 'n') # no border
plot(x.1,x.2,xaxt="n",yaxt="n",xlab="",ylab="", axes=F)
textxy(x.1, x.2, rownames(dat), cex = 1.3)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-3-1.png" width="1152" style="display: block; margin: auto;" />

```r
hc = hclust(dist(dat), "single")
#plot(hc)
plot(hc, hang = -1, cex = 1.3)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-3-2.png" width="1152" style="display: block; margin: auto;" />

We cut the tree to have `k=2` clusters, and the corresponding cluster assignment is returned.


```r
cutree(hc, k = 2)
```

```
## [1] 1 2 2 1 1 2
```

The hierarchical clustering with centroid linkage. The inversion occurs.


```r
# inversion
x.1 = c(1,5,3)
x.2 = c(1,1,1+2*sqrt(3))
dat = data.frame(x.1,x.2)
plot(x.1,x.2
     ,xaxt="n",yaxt="n",xlab="",ylab="", axes=F)
textxy(x.1, x.2, rownames(dat), cex = 1.3)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-5-1.png" width="1152" style="display: block; margin: auto;" />

```r
hc = hclust(dist(dat), "centroid")
plot(hc, hang = -1, cex = 1.3)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-5-2.png" width="1152" style="display: block; margin: auto;" />

We create an artificial gene expression data, and apply the hierarchical clustering with centroid linkage.


```r
test           = matrix(
  c(
    0.96, 0.07, 0.97, 0.98, 0.99, 0.50,
    0.28, 0.29, 0.77, 0.78, 0.08, 0.96,
    0.51, 0.51, 0.55, 0.14, 0.19, 0.41,
    0.51, 0.40, 0.97, 0.98, 0.99, 0.50
  ), ncol = 6, byrow = TRUE
)
colnames(test) = c("Exp1", "Exp2", "Exp3", "Exp4", "Exp5", "Exp6")
rownames(test) = c("Gene1", "Gene2", "Gene3", "Gene4")
test           = as.table(test)
mat            = data.matrix(test)

heatmap.2(
  mat, dendrogram = "row", Rowv = TRUE, Colv = FALSE,
  distfun = function(x)
    dist(x),
  hclustfun = function(x)
    hclust(x, method = 'centroid'),
  xlab = NULL, ylab = NULL, key = TRUE, keysize = 1, trace = "none",
  density.info = c("none"), margins = c(6, 12), col = bluered
)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-6-1.png" width="1152" style="display: block; margin: auto;" />

```r
hc = hclust(dist(test),  "centroid")
plot(hc, hang = -1, cex = 1.3)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-6-2.png" width="1152" style="display: block; margin: auto;" />

You can choose a color palette to be used for the visualization of clusters, observations, etc.

<a id="code-snippet--colpal"></a>

```r
col.pal = choose_palette()
```

The palette I generated is saved in an R data file.

<a id="code-snippet--loadcol"></a>

```r
load(url(paste(bburl, 'col.pal.RData', sep='/'))) # load color palette
```


## Spectral clustering {#spectral-clustering}

Three concentric circles data is simulated below:


```r
colkey = col.pal(3)
sc3b = shapes.circles3(numObjects = c(120,240,360))
X = sc3b$data
N = nrow(X)
plot(X, col=colkey[sc3b$clusters])
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-7-1.png" width="1152" style="display: block; margin: auto;" />

Spectral clustering is applied, which nicely detect the feature structure.


```r
sc = specc(X, centers=3)
sc@.Data # cluster assignment
```

```
##   [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
##  [36] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
##  [71] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
## [106] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
## [141] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
## [176] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
## [211] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
## [246] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
## [281] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
## [316] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
## [351] 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [386] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [421] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [456] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [491] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [526] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [561] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [596] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [631] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [666] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [701] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
```

```r
dat = data.frame(x1 = X[,1],x2 = X[,2], cluster = as.factor(sc@.Data))
sc.plot =
  ggplot(data = dat,aes(x = x1,y = x2,label = cluster,color = cluster)) +
  geom_text(angle = 65)  +  theme_bw() +
  theme(
    legend.position = "none",
    axis.line = element_line(colour = "black"),
    #panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    #panel.border = element_blank()
    panel.background = element_blank()
  )
sc.plot
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-8-1.png" width="1152" style="display: block; margin: auto;" />

Recall that the K-means clustering algorithm should be repeatedly implemented with arbitrary staring cluster centers, and choose the cluster assignment with the smallest with-in cluster variation.


```r
km = kmeans(X, centers=3, iter.max = 20, nstart = 20)
dat = data.frame(x1 = X[,1],x2 = X[,2], cluster = as.factor(km$cluster))
km.plot =
  ggplot(data = dat,aes(x = x1,y = x2,label = cluster,color = cluster)) +
  geom_text(angle = 65)  +  theme_bw() +
  theme(
    legend.position = "none",
    axis.line = element_line(colour = "black"),
    #panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    #panel.border = element_blank()
    panel.background = element_blank()
  )
km.plot
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-9-1.png" width="1152" style="display: block; margin: auto;" />


## NCI microarray {#nci-microarray}

[NCI microarray data]({{< relref "math6312-data" >}}) and cancer types are loaded. The <kbd>fread()</kbd> provided in <kbd>data.table</kbd> package can be used to read large data sets. It can be a lot faster than <kbd>read.table</kbd> or <kbd>read.csv</kbd>. It supports an OpenMP enabled compiler for the multi-thread processing. For the details, please see <https://github.com/Rdatatable/data.table/wiki/Installation> and <https://www.datacamp.com/community/tutorials/data-table-cheat-sheet>.

<a id="code-snippet--read-nci"></a>

```r
cancer = read.table("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/nci.info.txt", skip = 20, stringsAsFactors = F)
nci = data.table::fread("https://web.stanford.edu/~hastie/ElemStatLearn/datasets/nci.data")
colnames(nci) = c(cancer[,1]) # assign the cancer type.
```

We perform the SVD to obtain the first two PC scores for the visualization.


```r
X = t(nci)
rownames(X) = cancer[,1]

X = scale(X, TRUE, FALSE)
svd.x = svd(X)
low.x = svd.x$u[,1:2] %*% diag(svd.x$d[1:2])

dat = data.frame(PC1 = low.x[,1],PC2 = low.x[,2],cancer.type = as.factor(rownames(X)))
pca.plot =
  ggplot(data = dat,aes(x = PC1,y = PC2,label = cancer.type,color = cancer.type)) +
  geom_text(angle = 65)  +  theme_bw() +
  theme(
    legend.position = "none",
    axis.line = element_line(colour = "black"),
    #panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    #panel.border = element_blank()
    panel.background = element_blank()
  )
pca.plot
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-10-1.png" width="1152" style="display: block; margin: auto;" />

The first three PC scores are used to create a 3D plot for NCI microarray data.

<a id="code-snippet--nci3d,webgl=T"></a>

```r
low.x = svd.x$u[,1:3] %*% diag(svd.x$d[1:3])
colkey= col.pal(length(unique(cancer[,1])))[as.numeric(as.factor(cancer[,1]))]
dat = data.frame(cancer = rownames(X),low.x)
with(dat,text3d(X1,X2,X3,cancer,col=colkey, cex=.75))
decorate3d(xlab = "PC1", ylab = "PC2", zlab = "PC3")
```

We use `clusGap()` to calculate the gap statistics. The function does bootstrapping, and it requires to have a clustering function as an argument. The clustering function must take a data matrix and number of clusters as inputs and return the cluster assignment as outputs.

<a id="code-snippet--cluster-fun"></a>

```r
hclust.ave = function(x,k) {
  hc = fastcluster::hclust(dist(x),method = "average")
  c.assign = cutree(hc, k = k)
  names(c.assign) = NULL
  out = list(cluster = c.assign)
  return(out)
}
```

Using the NCI data, the gap statistics are calculated for the hierarchical clustering with average linkage.


```r
set.seed(1)
    gskmn = clusGap(X, FUNcluster = hclust.ave, K.max = 8, B = 20)
    plot(gskmn, ylim = c(0.36,0.45), main = "clusGap(., FUN = hclust.ave, B= 20)")
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-11-1.png" width="1152" style="display: block; margin: auto;" />

```r
    hc = agnes(X,  metric = "euclidean", stand = F, method = "average")
    plot(hc,which.plots=2)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-11-2.png" width="1152" style="display: block; margin: auto;" />

We simulate the data set with nicely separated clusters (K=4) to see how well the gap statistic guides us for choosing the number of clusters.


```r
x = rbind(
  matrix(rnorm(150, sd = 0.1), ncol = 3),
  matrix(rnorm(150, mean = 1, sd = 0.1), ncol = 3),
  matrix(rnorm(150, mean = 2, sd = 0.1), ncol = 3),
  matrix(rnorm(150, mean = 3, sd = 0.1), ncol = 3))

gskmn = clusGap(x, FUNcluster = hclust.ave, K.max = 8, B = 20)
plot(gskmn, main = "clusGap(., FUN = hclust.ave, B= 20)")
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-12-1.png" width="1152" style="display: block; margin: auto;" />

```r
hc = agnes(x,  metric = "euclidean", stand = F, method = "average")
plot(hc,which.plots=1)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-12-2.png" width="1152" style="display: block; margin: auto;" />

The banner plot is just an alternative to the dendogram. The white area on the left of the banner plot represents the unclustered data while the white lines that stick into the red are show the heights at which the clusters were formed. Since we do not want to include too many clusters that joined together at similar heights, it looks like four clusters, at a height of about 0.5 is a good solution.

The silhouette plot for K=2,...,7 are presented for the artificial data. The silhouettes look similar when K=4, and average silhouette width is the largest at K=4.


```r
X = x
dmatrix = dist(X)
si = list()
for (k in 2:7) {
  mod = kmeans(X, centers = k, nstart = 20)
  si[[k]] = silhouette(mod$cluster, dmatrix)
}

op = list(
  mfrow = c(1,2), oma = c(0,0, 3, 0),
  mgp = c(1.6,.8,0), mar = .1 + c(4,2,2,2)
)
par(op)
colvec = col.pal(7)
for (k in 2:7) {
  plot(si[[k]], main = paste("k = ",k), do.n.k = FALSE,
       col = colvec[1:k])
}
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-13-1.png" width="1152" style="display: block; margin: auto;" /><img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-13-2.png" width="1152" style="display: block; margin: auto;" /><img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-13-3.png" width="1152" style="display: block; margin: auto;" />

The below is R code to simulated a two-dimensional feature matrix. The code creates plots to illustrate the linear PCA.


```r
set.seed(1)
N = 500
x = matrix(rnorm(N),N / 2,2)

S = matrix(c(2,1,1,1),2,2) / 32
x = x %*% chol(S)

xy.lim = 1.2

par(pty = "s")
plot(x,xlab = "",ylab = "",xlim = c(-xy.lim,xy.lim),ylim = c(-xy.lim,xy.lim))
draw.circle(0,0,radius = .96,nv = 200,border = "orange",lty = 2,lwd = 1)

pca.x = princomp(x,cor = F)
v1 = pca.x$loadings[,1]
v2 = pca.x$loadings[,2]

arrows(0,0,0.98 * v1[1],0.98 * v1[2], col = "red")
arrows(0,0,v2[1],v2[2], col = "green")
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-14-1.png" width="1152" style="display: block; margin: auto;" />

```r
plot(pca.x$scores[,1],0 * numeric(N / 2),col = "red",xlim = c(-1,1)
  ,ylim = c(0,0.01),xlab = "1st PC score",ylab = "",axes = F)
axis(side = 1, at = c(-1,0,1))
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-14-2.png" width="1152" style="display: block; margin: auto;" />

```r
plot(pca.x$scores[,2],0 * numeric(N / 2),col = "green",xlim = c(-1,1)
  ,ylim = c(0,0.01),xlab = "2nd PC score",ylab = "",axes = F)
axis(side = 1, at = c(-1,0,1))
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-14-3.png" width="1152" style="display: block; margin: auto;" />

```r
plot(pca.x$scores,xlab = "",ylab = "",xlim = c(-xy.lim,xy.lim),ylim = c(-xy.lim,xy.lim))
draw.circle(0,0,radius = .96,nv = 200,border = "orange",lty = 2,lwd = 1)
arrows(0,0,0.96,0, col = "red")
arrows(0,0,0,1.03, col = "green")
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-14-4.png" width="1152" style="display: block; margin: auto;" />


## Three concentric circles {#three-concentric-circles}


```r
colkey = col.pal(3)
sc3b = shapes.circles3(numObjects = c(120,240,360))
X = sc3b$data
N = nrow(X)
plot(X, col=colkey[sc3b$clusters])
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-15-1.png" width="1152" style="display: block; margin: auto;" />

The 1st and 2nd PCs are extracted using Gaussian kernel PCA.


```r
x.kpca = kpca(X, kernel = "rbfdot", kpar = list(sigma = 2.5), features = 2, th = 1e-4)
plot(x.kpca@pcv[,1],x.kpca@pcv[,2],xlab = "KPC1",ylab = "KPC2",col = colkey[sc3b$clusters],ylim = c(-.3,.3),xlim = c(-.3,.3))
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-16-1.png" width="1152" style="display: block; margin: auto;" />

```r
plot(x.kpca@pcv[,1],0 * numeric(N),col = colkey[sc3b$clusters],ylim = c(0,0.01),xlim = c(-.3,.3)
     ,xlab = "1st PC score (Inverse kernel width: 2.5)",ylab = "",axes = F)
axis(side = 1, at = c(-1,0,1))
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-16-2.png" width="1152" style="display: block; margin: auto;" />

The 1st PC is extracted from the linear PCA.


```r
x.kpca = kpca(X, kernel = "vanilladot", kpar = list(), features = 2, th = 1e-4)
plot(x.kpca@pcv[,1],0 * numeric(N),col = colkey[sc3b$clusters],ylim = c(0,0.01)
     ,xlab = "1st PC score (Linear PCA)",ylab = "",axes = F)
axis(side = 1, at = c(-1,0,1))
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-17-1.png" width="1152" style="display: block; margin: auto;" />

We apply the Gaussian kernel PCA with multiple values of inverse kernel width (\\(\sigma\\)), and create an animation file.

<a id="code-snippet--ani,eval=F"></a>

```r
ani.options('interval' = 0.5) # interval between figures
sigma.grid = (1:50) / 8

saveGIF({
  for (i in 1:50) {
    title = paste("Inverse kernel width: ",sprintf("%0.2f",sigma.grid[i]),sep = "")
    x.kpca = kpca(
      X, kernel = "rbfdot", kpar = list(sigma = sigma.grid [i]), features = 2, th = 1e-4
    )
    plot(
      x.kpca@pcv[,1],x.kpca@pcv[,2],xlab = "KPC1",ylab = "KPC2",col = colkey[sc3b$clusters],ylim = c(-.3,.3),xlim =
        c(-.3,.3),main = title)
  }
},movie.name = "RBF-PCA.gif")
```


## Half-moon shapes {#half-moon-shapes}


```r
set.seed(1)
colkey = col.pal(2)
stm = shapes.two.moon(180)
X = stm$data
N = nrow(X)
#X = scale(X,T,T)
plot(X,col = colkey[stm$clusters])
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-18-1.png" width="1152" style="display: block; margin: auto;" />

We apply polynomial kernel and linear PCA to half moon shape (with one moon).


```r
halfX = X[stm$clusters==1,c(2,1)]
plot(X)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-19-1.png" width="1152" style="display: block; margin: auto;" />

```r
x.kpca = kpca(halfX, kernel = "vanilladot", kpar = list(), features = 0)
x.kpca@eig
```

```
##    Comp.1    Comp.2 
## 0.5030047 0.1083626
```

```r
x.kpca = kpca(halfX, kernel = "polydot", kpar = list(degree=2), features = 0)
x.kpca@eig
```

```
##      Comp.1      Comp.2      Comp.3      Comp.4      Comp.5 
## 1.006705262 0.352005137 0.082623372 0.027620536 0.007623235
```

```r
plot(x.kpca@pcv[,1],x.kpca@pcv[,2],xlab = "KPC1",ylab = "KPC2")
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-19-2.png" width="1152" style="display: block; margin: auto;" />

Gaussian kernel PCA with \\(\sigma = 7.5\\) is applied.


```r
x.kpca = kpca(X, kernel = "rbfdot", kpar = list(sigma = 60 / 8), features = 0, th = 1e-4)

title = paste("Inverse kernel width: ",sprintf("%0.2f",7.5),sep = "")
x.kpca = kpca(X, kernel = "rbfdot", kpar = list(sigma = 60 / 8), features = 0, th = 1e-4)
plot(x.kpca@pcv[,1],x.kpca@pcv[,2],xlab = "KPC1",ylab = "KPC2",col = colkey[stm$clusters],main = title)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-20-1.png" width="1152" style="display: block; margin: auto;" />

```r
plot(x.kpca@pcv[,1],x.kpca@pcv[,2],xlab = "KPC1",ylab = "KPC2",col = colkey[stm$clusters]
     ,ylim = c(-.5,.5),xlim = c(-.5,.5) ,main = title)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-20-2.png" width="1152" style="display: block; margin: auto;" />

```r
plot(x.kpca@pcv[,1],0 * numeric(N),col = colkey[stm$clusters],ylim = c(0,0.01)
     ,xlim = c(-.5,.5),xlab = "1st PC score (Inverse kernel width: 7.5)",ylab = "",axes = F)
axis(side = 1, at = c(-.5,0,.5))
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-20-3.png" width="1152" style="display: block; margin: auto;" />

The first PC scores from linear PCA.


```r
x.kpca = kpca(X, kernel = "vanilladot", kpar = list(), features = 0, th = 1e-4
)
plot(-x.kpca@pcv[,1],0 * numeric(N),col = colkey[stm$clusters],ylim = c(0,0.01)
     ,xlab = "1st PC score (Linear PCA)",ylab = "",axes = F)
axis(side = 1, at = c(-1,0,1))
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-21-1.png" width="1152" style="display: block; margin: auto;" />

The below is code to create animated GIF.

<a id="code-snippet--moonani,eval=F"></a>

```r
ani.options('interval' = 0.5) # inverval between figures
sigma.grid = (1:80) / 8

saveGIF({
  for (i in 1:80) {
    title = paste("Inverse kernel width: ",sprintf("%0.2f",sigma.grid[i]),sep = "")
    x.kpca = kpca(X, kernel = "rbfdot", kpar = list(sigma = sigma.grid [i]), features = 2, th = 1e-4)
    plot(x.kpca@pcv[,1],x.kpca@pcv[,2],xlab = "KPC1",ylab = "KPC2",col = colkey[stm$clusters]
         ,ylim = c(-.5,.5),xlim = c(-.5,.5),main = title)
  }},movie.name = "moonPCA.gif")
```


## Swiss roll {#swiss-roll}

<style>.org-center { margin-left: auto; margin-right: auto; text-align: center; }</style>

<div class="org-center">
  <div></div>

{{< figure src="https://i.pinimg.com/736x/4f/e5/00/4fe5002ad4e4236d59098f61a9c969a7--strawberry-jam-pastry-strawberry-and-cream-swiss-roll.jpg" width="100px" >}}

</div>

We generate the Swiss roll data with no noise. For more perceptive visualization, we mark each point with slightly different colors.


```r
set.seed(1)
N = 1500
t = (3 * pi / 2) * (1 + 2 * runif(N)); height = runif(N);
X = cbind(t * cos(t), height, t * sin(t))
X = scale(X)

tcol = col.pal(32)
colvec = floor((t - min(t)) / (max(t) - min(t)) * 32); colvec[colvec == 0] = 1

scatterplot3d(X,pch=18,color=tcol[colvec],xlab=expression("x"[1]),
              ylab=expression("x"[2]),zlab=expression("x"[3]),cex.lab=1.5,
              main="Swiss Roll",cex.main=1.5)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-22-1.png" width="1152" style="display: block; margin: auto;" />

We create a 3d plot object that we can freely rotate axes.

<a id="code-snippet--swiss3d,webgl=TRUE"></a>

```r
plot3d( X,col = tcol[colvec],xlab = "x", ylab = "y", zlab = "z")
```

We apply polynomial and Gaussian kernel PCAs. They does not work well to learn the manifold structure.


```r
x.kpca = kpca(X, kernel = "polydot", kpar = list(degree = 6), features = 2, th = 1e-4)
plot(x.kpca@pcv[,1],x.kpca@pcv[,2],xlab = "KPC1",ylab = "KPC2",col = tcol[colvec], main = "Polynomial kernel PCA")
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-23-1.png" width="1152" style="display: block; margin: auto;" />

```r
x.kpca = kpca(X, kernel = "rbfdot", kpar = list(sigma = 3),features = 0, th = 1e-4)

plot(x.kpca@pcv[,1],x.kpca@pcv[,2],xlab = "KPC1",ylab = "KPC2",col = tcol[colvec], main = "Gaussian kernel PCA")
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-23-2.png" width="1152" style="display: block; margin: auto;" />

The linear PCA is applied. The first and third PCs are used for the plot below.


```r
x.pca = princomp(X)
plot(x.pca$score[,1],x.pca$score[,3],xlab = "PC1",ylab = "PC3", col = tcol[colvec], main = "Linear PCA")
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-24-1.png" width="1152" style="display: block; margin: auto;" />


### Isomap {#isomap}

The 2 dimensional Isomap is applied to the Swiss roll.

<a id="code-snippet--isomap-n2-k5"></a>

```r
dis = dist(X)
colkey = tcol[colvec]
imap=isomap(dis,ndim=2,k=5);
plot(imap,col=colkey)
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/isomap_n2_k5-1.png" width="1152" style="display: block; margin: auto;" />


### Local linear embedding {#local-linear-embedding}

The optimal number of neighbours `k` can be found using `calc_k()`. It takes a few minutes, and can be a bit faster if you use <kbd>parallel=T</kbd>.

<a id="code-snippet--lle-calc-k"></a>

```r
 dcores = parallel::detectCores() - 1 # number of cpu cores for parrallel computing
calc_k(X, m = 2, kmin = 5, kmax = 15, plotres = T, cpus=dcores, parallel=T)
```

```
## R Version:  R version 3.6.1 (2019-07-05) 
## 
## Library lle loaded.
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/lle_calc_k-1.png" width="1152" style="display: block; margin: auto;" />

```
##     k       rho
## 1   5 0.9253778
## 2   6 0.8155542
## 3   7 0.8215571
## 4   8 0.8187899
## 5   9 0.7761368
## 6  10 0.7689874
## 7  11 0.7285092
## 8  12 0.7257432
## 9  13 0.7653296
## 10 14 0.7665756
## 11 15 0.7555881
```

```r
 results = lle(X = X, m = 2, k = 12, id = TRUE, v = 0.9)
```

```
## finding neighbours
## calculating weights
## intrinsic dim: mean=1.999333, mode=2
## computing coordinates
```

```r
 plot(results$Y[,2],results$Y[,1], main = "embedded data"
     ,xlab = expression(y[2]), ylab = expression(y[1]), col = tcol[colvec]
      )
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/lle_calc_k-2.png" width="1152" style="display: block; margin: auto;" />


## Revisit: NCI microarray {#revisit-nci-microarray}

We apply the MDS and Gaussian kernel PCA to NCI data. The results of MDS are the same as one using linear PCA if you use Euclidean distance.


```r
X = t(nci)
rownames(X) = cancer[,1]
dis = dist(X, method = "euclidean") # euclidean distances between the rows
#fit = isoMDS(dis, k=2) # k is the number of dim
fit = cmdscale(dis,eig=TRUE, k=2)
fit # view results
```

```
## $points
##                   [,1]        [,2]
## CNS         -19.795782  -0.1152691
## CNS         -21.546101   1.4573503
## CNS         -25.056621  -1.5260929
## RENAL       -37.409536  11.3894784
## BREAST      -50.218642   1.3461737
## CNS         -26.435203  -0.4629819
## CNS         -27.339334  -2.6503128
## BREAST      -21.489658  -4.9541432
## NSCLC       -20.852496 -10.1630615
## NSCLC       -26.952915 -21.4733142
## RENAL       -24.446697  -9.8421205
## RENAL       -35.075028  -6.2621247
## RENAL       -21.483161 -10.6705481
## RENAL       -25.004732 -11.9464198
## RENAL       -31.745680 -14.9829235
## RENAL       -24.237312 -14.7479701
## RENAL       -20.503015 -16.1817793
## BREAST      -11.985682  -8.0623004
## NSCLC       -24.344223  -3.7106407
## RENAL       -14.307441  -4.0257867
## UNKNOWN     -11.696115 -11.5399627
## OVARIAN     -17.551264 -16.6589889
## MELANOMA    -10.158885   1.5009404
## PROSTATE      2.609693  -7.0693601
## OVARIAN       4.273638 -14.7401698
## OVARIAN      -7.003487 -14.7512986
## OVARIAN       2.175539 -10.0689995
## OVARIAN     -10.390669 -17.2133665
## OVARIAN      -4.105585 -12.8336713
## PROSTATE     -3.107199 -13.5041785
## NSCLC       -13.920131 -10.7706320
## NSCLC        -7.340931 -11.8634764
## NSCLC        -5.031984  -1.6785213
## LEUKEMIA     21.303920   6.0889181
## K562B-repro  38.922020   3.6351811
## K562A-repro  46.441061   4.8194665
## LEUKEMIA     48.513831   7.0614137
## LEUKEMIA     41.878712   6.9915000
## LEUKEMIA     56.922686   9.5122363
## LEUKEMIA     50.959323   8.1773536
## LEUKEMIA     26.031599   8.7690748
## COLON        11.818059  -8.0595882
## COLON        31.654807  -6.5893268
## COLON        26.333583 -15.0839917
## COLON        20.568131 -18.2511162
## COLON        21.536202 -13.4346199
## COLON        34.669827 -13.2857448
## COLON        18.605803 -17.0724922
## MCF7A-repro  38.194352 -13.3673940
## BREAST       38.155371 -11.5925449
## MCF7D-repro  30.893122 -12.9394271
## BREAST       21.585841  -9.8917234
## NSCLC         3.110738 -17.6582405
## NSCLC         9.160494  -1.4605448
## NSCLC        14.878915  -0.6309639
## MELANOMA      5.272994  45.4663832
## BREAST       -3.319772  46.3001255
## BREAST        1.321339  51.3627117
## MELANOMA    -11.087277  37.6788847
## MELANOMA    -15.446086  44.1646074
## MELANOMA     -1.925437  35.3286112
## MELANOMA    -14.359566  33.2916782
## MELANOMA    -12.740136  45.2228727
## MELANOMA     -8.377818  34.2231717
## 
## $eig
##  [1]  3.989258e+04  2.223445e+04  1.763489e+04  1.153423e+04  1.030411e+04
##  [6]  9.393097e+03  7.704158e+03  7.546846e+03  7.067195e+03  5.777779e+03
## [11]  5.625239e+03  5.363188e+03  4.903092e+03  4.734148e+03  4.479053e+03
## [16]  4.313525e+03  4.226112e+03  3.894810e+03  3.877879e+03  3.781525e+03
## [21]  3.678888e+03  3.463529e+03  3.376313e+03  3.171466e+03  3.159457e+03
## [26]  2.971593e+03  2.843771e+03  2.760077e+03  2.721835e+03  2.662913e+03
## [31]  2.567680e+03  2.513886e+03  2.346339e+03  2.321256e+03  2.241116e+03
## [36]  2.217796e+03  2.172898e+03  2.140319e+03  2.031981e+03  1.998809e+03
## [41]  1.955073e+03  1.914526e+03  1.875043e+03  1.823102e+03  1.784005e+03
## [46]  1.686705e+03  1.667479e+03  1.586297e+03  1.466288e+03  1.438720e+03
## [51]  1.386747e+03  1.312147e+03  1.270309e+03  1.226021e+03  1.150863e+03
## [56]  1.118501e+03  1.051878e+03  9.678660e+02  9.040303e+02  7.823942e+02
## [61]  6.567918e+02  6.262224e+02  5.615703e+02 -3.111845e-12
## 
## $x
## NULL
## 
## $ac
## [1] 0
## 
## $GOF
## [1] 0.2319364 0.2319364
```

```r
# plot solution
x = fit$points[,1]
y = fit$points[,2]

dat = data.frame(dim1 = x,dim2 = y,cancer.type = as.factor(rownames(X)))
mds.plot =
  ggplot(data = dat,aes(x = dim1,y = dim2,label = cancer.type,color = cancer.type)) +
  geom_text(angle = 65)  +  theme_bw() +
  theme(
    legend.position = "none",
    axis.line = element_line(colour = "black"),
    #panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    #panel.border = element_blank()
    panel.background = element_blank()
  )
mds.plot
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-25-1.png" width="1152" style="display: block; margin: auto;" />


```r
x.kpca = kpca(X, kernel = "rbfdot", kpar = list(sigma = 0.00005), features = 0, th = 1e-4)
dat = data.frame(KPC1 = x.kpca@pcv[,1],KPC2 = x.kpca@pcv[,2],cancer.type = as.factor(rownames(X)))
kpca.plot =
  ggplot(data = dat,aes(x = KPC1,y = KPC2,label = cancer.type,color = cancer.type)) +
  geom_text(angle = 65)  +  theme_bw() +
  theme(
    legend.position = "none",
    axis.line = element_line(colour = "black"),
    #panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    #panel.border = element_blank()
    panel.background = element_blank()
  )
kpca.plot
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-26-1.png" width="1152" style="display: block; margin: auto;" />

The below is code to apply locally linear embedding.

<a id="code-snippet--nci-lle-calc-k"></a>

```r
# find best tuning parameter k
calc_k(X, m = 2, kmin = 5, kmax = 20, plotres = T, cpus=dcores, parallel=T)
```

```
## Library lle loaded.
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/nci_lle_calc_k-1.png" width="1152" style="display: block; margin: auto;" />

```
##     k       rho
## 1   5 0.7190052
## 2   6 0.7437115
## 3   7 0.7883512
## 4   8 0.7729525
## 5   9 0.7644099
## 6  10 0.7147773
## 7  11 0.7051918
## 8  12 0.6945983
## 9  13 0.6862212
## 10 14 0.6926298
## 11 15 0.6819705
## 12 16 0.6589292
## 13 17 0.6547393
## 14 18 0.6606307
## 15 19 0.6589010
## 16 20 0.6608254
```

```r
results = lle(X = X, m = 2, k = 17, id = T, v = 0.9)
```

```
## finding neighbours
## calculating weights
## intrinsic dim: mean=11.04688, mode=11
## computing coordinates
```


```r
dat = data.frame(Y1 = results$Y[,1],Y2 = results$Y[,2],cancer.type = as.factor(rownames(X)))
lle.plot =
  ggplot(data = dat,aes(x = Y1,y = Y2,label = cancer.type,color = cancer.type)) +
  geom_text(angle = 65)  +  theme_bw() +
  theme(
    legend.position = "none",
    axis.line = element_line(colour = "black"),
    #panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    #panel.border = element_blank()
    panel.background = element_blank()
  )
lle.plot
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-27-1.png" width="1152" style="display: block; margin: auto;" />

The three dimensional Isomap (<kbd>ndim = 3</kbd>) is applied.

<a id="code-snippet--iso3d,webgl=TRUE"></a>

```r
imap=isomap(dis,ndim=3,k=5);
temp = list(imap = imap, cancer = cancer[,1])
within(temp, rgl.isomap(imap, web = "White", col=colkey))
```

```
## $imap
## 
## Isometric Feature Mapping (isomap)
## 
## Call:
## isomap(dist = dis, ndim = 3, k = 5) 
## 
## Distance method: euclidean shortest isomap 
## Criterion: k = 5 
## 
## $cancer
##  [1] "CNS"         "CNS"         "CNS"         "RENAL"       "BREAST"     
##  [6] "CNS"         "CNS"         "BREAST"      "NSCLC"       "NSCLC"      
## [11] "RENAL"       "RENAL"       "RENAL"       "RENAL"       "RENAL"      
## [16] "RENAL"       "RENAL"       "BREAST"      "NSCLC"       "RENAL"      
## [21] "UNKNOWN"     "OVARIAN"     "MELANOMA"    "PROSTATE"    "OVARIAN"    
## [26] "OVARIAN"     "OVARIAN"     "OVARIAN"     "OVARIAN"     "PROSTATE"   
## [31] "NSCLC"       "NSCLC"       "NSCLC"       "LEUKEMIA"    "K562B-repro"
## [36] "K562A-repro" "LEUKEMIA"    "LEUKEMIA"    "LEUKEMIA"    "LEUKEMIA"   
## [41] "LEUKEMIA"    "COLON"       "COLON"       "COLON"       "COLON"      
## [46] "COLON"       "COLON"       "COLON"       "MCF7A-repro" "BREAST"     
## [51] "MCF7D-repro" "BREAST"      "NSCLC"       "NSCLC"       "NSCLC"      
## [56] "MELANOMA"    "BREAST"      "BREAST"      "MELANOMA"    "MELANOMA"   
## [61] "MELANOMA"    "MELANOMA"    "MELANOMA"    "MELANOMA"
```

```r
within(temp, text3d(imap$points[,1],imap$points[,2],imap$points[,3],cancer,col=colkey, cex=.75))
```

```
## $imap
## 
## Isometric Feature Mapping (isomap)
## 
## Call:
## isomap(dist = dis, ndim = 3, k = 5) 
## 
## Distance method: euclidean shortest isomap 
## Criterion: k = 5 
## 
## $cancer
##  [1] "CNS"         "CNS"         "CNS"         "RENAL"       "BREAST"     
##  [6] "CNS"         "CNS"         "BREAST"      "NSCLC"       "NSCLC"      
## [11] "RENAL"       "RENAL"       "RENAL"       "RENAL"       "RENAL"      
## [16] "RENAL"       "RENAL"       "BREAST"      "NSCLC"       "RENAL"      
## [21] "UNKNOWN"     "OVARIAN"     "MELANOMA"    "PROSTATE"    "OVARIAN"    
## [26] "OVARIAN"     "OVARIAN"     "OVARIAN"     "OVARIAN"     "PROSTATE"   
## [31] "NSCLC"       "NSCLC"       "NSCLC"       "LEUKEMIA"    "K562B-repro"
## [36] "K562A-repro" "LEUKEMIA"    "LEUKEMIA"    "LEUKEMIA"    "LEUKEMIA"   
## [41] "LEUKEMIA"    "COLON"       "COLON"       "COLON"       "COLON"      
## [46] "COLON"       "COLON"       "COLON"       "MCF7A-repro" "BREAST"     
## [51] "MCF7D-repro" "BREAST"      "NSCLC"       "NSCLC"       "NSCLC"      
## [56] "MELANOMA"    "BREAST"      "BREAST"      "MELANOMA"    "MELANOMA"   
## [61] "MELANOMA"    "MELANOMA"    "MELANOMA"    "MELANOMA"
```

The two dimensional Isomap (<kbd>ndim = 2</kbd>) is applied below.


```r
dis = dist(X)
imap=isomap(dis,ndim=2,k=5)
colkey= col.pal(length(unique(cancer[,1])))[as.numeric(as.factor(cancer[,1]))]
plot(imap,col=colkey,cex=0.5,type = "text")
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-28-1.png" width="1152" style="display: block; margin: auto;" />

Spectral clustering is applied and we visualize the clusters in two variables from Isomap and PCA.


```r
sc.col = col.pal(3)
sc = specc(X, centers=3)
size(sc)
```

```
## [1] 16 41  7
```

```r
withinss(sc)
```

```
## [1]  84675.62 175497.39  69076.56
```

```r
sc@.Data
```

```
##  [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 2 2 2 2 2 2 2 2 1 3
## [36] 3 3 3 3 3 3 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2
```


```r
dat = data.frame(x1 = imap$points[,1], x2 = imap$points[,2], cluster = as.factor(sc@.Data))
sc.plot =
  ggplot(data = dat,aes(x = x1,y = x2,label = cluster,color = cluster)) +
  geom_text(angle = 65)  +  theme_bw() +
  theme(
    legend.position = "none",
    axis.line = element_line(colour = "black"),
    #panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    #panel.border = element_blank()
    panel.background = element_blank()
  )
sc.plot
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-30-1.png" width="1152" style="display: block; margin: auto;" />


```r
x.kpca = kpca(X, kernel = "vanilladot", kpar=list(), features = 2)
dat = data.frame(x1 = x.kpca@pcv[,1],x2 = x.kpca@pcv[,2], cluster = as.factor(sc@.Data))
sc.plot =
  ggplot(data = dat,aes(x = x1,y = x2,label = cluster,color = cluster)) +
  geom_text(angle = 65)  +  theme_bw() +
  theme(
    legend.position = "none",
    axis.line = element_line(colour = "black"),
    ## panel.grid.major = element_blank(),
    panel.grid.minor = element_blank(),
    ## panel.border = element_blank()
    panel.background = element_blank()
  )
 sc.plot
```

<img src="/courses/math6312-sp19/lab5_files/figure-html/unnamed-chunk-31-1.png" width="1152" style="display: block; margin: auto;" />
