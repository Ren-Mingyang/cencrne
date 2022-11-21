# cencrne
An R package for consistent estimation of the number of communities via regularized network embedding.

## Introduction
The network analysis plays an important role in numerous application domains including biomedicine. 
Estimation of the number of communities is a fundamental and critical issue in network analysis. 
Most existing studies assume that the number of communities is known a priori, 
or lack of rigorous theoretical guarantee on the estimation consistency.
This method proposes a regularized network embedding model to simultaneously estimate 
the community structure and the number of communities in a unified formulation. 
The proposed model equips network embedding with a novel composite regularization term, 
which pushes the embedding vector towards its center and collapses similar community centers 
with each other. A rigorous theoretical analysis is conducted, 
establishing asymptotic consistency in terms of community detection and estimation of the number of communities.


## Publication
Ren, M., Zhang S. and Wang J. (2022+). Consistent Estimation of the Number of Communities via Regularized Network Embedding. Manuscript.

## Maintainer
Mingyang Ren <renmingyang17@mails.ucas.ac.cn>  

## Installation

Method 1: Run the following codes directly in R.
```{r eval=FALSE}
library("devtools")
devtools::install_github("Ren-Mingyang/cencrne")
```
Method 2: Download the cencrne_1.0.0.tar.gz, and install from Package Archive File using RStudio.


## Usage
To make the package more user-friendly, there are detailed help documents and 
vignettes in the package, which can be referred to after the installation.


## Quick Start
First, we call the built-in simulation data set (K* = 4) and the sequences of the tuning parameters (lambda1, lambda2, and lambda3).

```{r eval=FALSE}
library(cencrne)
# example.data
data(example.data)
A                   = example.data$A
K.true              = example.data$K.true
Z.true              = example.data$Z.true
B.true              = example.data$B.true
P.true              = example.data$P.true
Theta.true          = example.data$Theta.true
cluster.matrix.true = example.data$cluster.matrix.true

n       = dim(A)[1]
lam.max = 3
lam.min = 0.5
lam1.s  = 2/log(n)
lam2.s  = sqrt(8*log(n)/n)
lam3.s  = 1/8/log(n)/sqrt(n)
lambda  = genelambda.obo(nlambda1=3,lambda1_max=lam.max*lam1.s,lambda1_min=lam.min*lam1.s,
                         nlambda2=10,lambda2_max=lam.max*lam2.s,lambda2_min=lam.min*lam2.s,
                         nlambda3=1,lambda3_max=lam.max*lam3.s,lambda3_min=lam.min*lam3.s)

```

Apply the proposed method.
```{r eval=FALSE}
sample.index.n = rbind(combn(n,2),1:(n*(n-1)/2))
int.list       = gen.int(A)
Z.int          = int.list$Z.int
B.int          = int.list$B.int
res            = network.comm.num(A, sample.index.n, lambda, Z.int, B.int)

# output results
K.hat = res$Opt_K # the estimated number of communities
Z.hat = res$Opt_Z # the estimated embedding vectors corresponding to n nodes
cluster.matrix.hat = res$Opt_cluster.matrix # the n * n estimated membership matrix
evaluation(Z.hat, Z.true, cluster.matrix.hat, cluster.matrix.true,
           P.true, Theta.true, K.hat, K.true)

```

The algorithm is efficient, and it takes less than five minutes for the toy example.
