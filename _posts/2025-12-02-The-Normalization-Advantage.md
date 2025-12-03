---
layout: post
title: "The Normalization Advantage for Laplacian Matrices"
author: Carol Pachar
date: 2025-12-02 
categories: [Laplacian Matrix, Spectral Graph Theory, Python]
---

## Introduction

Clustering is a data analysis technique. When looking at large data sets, clustering can help us identify groups of data that have a “similar behavior”. 
The goal of clustering is to make groups of data in which the data points inside of a group are similar to each other while datapoints from different groups are dissimilar 
to each other. Spectral clustering is a clustering algorithm that's been found to have superior advantages compared to traditional clustering algorithms such as k-means or single 
linkage (von Luxburg, 2007). 

Next, I will introduce the mathematical objects used in spectral clustering: similarity graphs and laplacian matrices.


## Similarity Graphs
 
A similarity graph is a node-link diagram in which nodes are laid out according to their similarity. The more similar two nodes are to each other, the closer they appear in the diagram. 
Often, similarity is a function of the edges, that is, strongly connected nodes are attracted to each other. There are different variants of Similarity Graphs, which differ in what 
nodes and edges represent and how similarity is defined (Hammad et al., 2020). 

![]

## A Laplacian Matrix
The main tool of spectral clustering are laplacian matrices. There is a whole field that's dedicated to studying these matrices, called spectral graph theory (von Luxburg, 2007). 

A laplacian matrix is defined as a matrix that is diagnolly dominant, contains non-positive off-diagnol entries, and is symmetric (Spielman, 2017).
![]


## A Real-World Application of Laplacian Matrices 
We have the following graph:

![]

Let each node represent a gene. We know the values of the function at some of the nodes, or genes (0 and 1). We want to guess the values of the function at other nodes. 
We know that some of these genes interact with each other. A scientist might be trying to figure out which genes might be implicated in a disease (e.g., diabetes). 
By using the known values of two genes (gene CDC27 with a value of 0 means that it’s not implicated in diabetes while gene ANAPC10 with a value of 1 means that it’s certain to be 
implicated in diabetes), we can use the Laplacian matrix to calculate the other gene’s probability of being related to the disease (Spielman, 2017). 

![]

## Which Matrix is Better to Use?
In most literature reviews, when an author is using a laplacian matrix, they tend to just call it the "graph Laplacian". However, I believe that a more precise distinction is necessary:
there are two types of laplacian matrices: the Unnormalized Laplacian matrix and the Normalized Laplacian matrix (von Luxburg, 2007). When an author says that they're using a 
laplacian matrix, it's usually one of these matrices. 

This brings us to the question: which matrix is better to use for spectral clustering?  

To answer this question, we will evaluate each matrix in terms of their clustering performance. In other words, we will be looking at the adjusted rand index and the runtime. 
The adjusted rand index is a metric that's used to measure clustering performance. It compares two different clusterings: the predicted clustering from a model and a known accurate clustering. 
Runtime refers to how long it takes for an algorithm to execute.

In order to evaluate each matrix's performance when used in spectral clustering, I've applied a spectral clustering function on two benchmark graph datasets: Zachary's Karate Club and a 
Stochastic Block Model. My approach involved using linear algebra to implement and compare the Unnormalized Laplacian and the Symmetrically Normalized Laplacian. For each matrix, 
the $k$ smallest non-zero eigenvectors (using numpy.linalg.eigh) was computed, discarding the trivial first vector, to create the feature embedding. This embedding was then clustered using 
the $k$-means algorithm. 

## Result

1. Zachary's Karate Club 

$$Matrix       |    ARI (Clustering Quality) |  Runtime (seconds)$$ 

$$Unnormalized |    0.0000                 |   0.0728$$      

$$Normalized   |    0.0216                 |   0.0039$$ 



2. Stochastic Block Model (SBM) 
_______________________________________________________________
Matrix       |    ARI (Clustering Quality) |  Runtime (seconds) 

Unnormalized |    $0.0015$                 |   $0.0602$      

Normalized   |    $0.6789$                 |   $0.0037$ 


## Conclusion 

The main finding was that the choice of Laplacian matrix has a massive impact on clustering performance.

The Normalized Laplacian provided dramatically superior clustering quality over the Unnormalized Laplacian in the structured SBM dataset, where it achieved an ARI of 0.6789 
compared to the near-random performance of the Unnormalized Laplacian (ARI 0.0015). It also also offered a significantly faster runtime (up to 18x faster in the Karate Club trial) for the 
combined spectral decomposition and $k$-means steps across both datasets.

In short, the Normalized Laplacian matrix proved to be the better choice of matrices for the spectral clustering algorithm when applied in Zachary's Karate Club and in the 
Stochastic Block Model datasets. 


## Citations

Hammad, M., Basit, H. A., Jarzabek, S., & Koschke, R. (2020). A systematic mapping study of clone visualization. Computer Science Review, 37, Article 100266.
https://doi.org/10.1016/j.cosrev.2020.100266

von Luxburg, U. (2007). A tutorial on spectral clustering. Statistics and Computing, 17(4), 395–416. https://doi.org/10.1007/s11222-007-9033-z

Spielman, D. A. [uwaterloo]. (2017, April 17). The Laplacian Matrices of Graphs: Algorithms and Applications [Video]. YouTube. http://www.youtube.com/watch?v=EjpMnU79neo



Please ignore anything below this line. :) 
### Step 1: Handling Missing Values

```python
# Example Python code block
import pandas as pd
df = pd.read_csv('train.csv')
print(df.isnull().sum())




## Citations

Hammad, M., Basit, H. A., Jarzabek, S., & Koschke, R. (2020). A systematic mapping study of clone visualization. Computer Science Review, 37, Article 100266.
https://doi.org/10.1016/j.cosrev.2020.100266

von Luxburg, U. (2007). A tutorial on spectral clustering. Statistics and Computing, 17(4), 395–416. https://doi.org/10.1007/s11222-007-9033-z

Spielman, D. A. [uwaterloo]. (2017, April 17). The Laplacian Matrices of Graphs: Algorithms and Applications [Video]. YouTube. http://www.youtube.com/watch?v=EjpMnU79neo

