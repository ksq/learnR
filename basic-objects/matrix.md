

# Matrix

A matrix is a vector represented and accessible in two dimensions. Therefore, what applies to vectors also applies to matrix. For example, each type of vector (e.g. numeric vector, logical vector) has their matrix version, that is, there are numeric matrix, logical matrix and so on.

## Creating matrix

We can call `matrix` function to create a matrix from a vector by setting up one of its two dimensions.


```r
> matrix(c(1,2,3,2,3,4,3,4,5),ncol = 3)
```

```
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    2    3    4
[3,]    3    4    5
```

```r
> matrix(c(1,2,3,
+          4,5,6,
+          7,8,9),nrow=3,byrow=FALSE)
```

```
     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9
```

```r
> matrix(c(1,2,3,
+          4,5,6,
+          7,8,9),nrow=3,byrow=TRUE)
```

```
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6
[3,]    7    8    9
```

Oftentimes, we may need to create a diagonal matrix. Here `diag` function is the most handy one.


```r
> diag(1,nrow=5)
```

```
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    0    0    0    0
[2,]    0    1    0    0    0
[3,]    0    0    1    0    0
[4,]    0    0    0    1    0
[5,]    0    0    0    0    1
```


## Naming rows and columns

By default, a matrix does not give names to its rows and columns. Sometimes it is useful and straightforward to do so when different rows and columns have different meanings. We can give row names and/or column names either when constrcuting the matrix


```r
> matrix(c(1,2,3,4,5,6,7,8,9),nrow=3,byrow=TRUE,
+   dimnames=list(c("r1","r2","r3"),c("c1","c2","c3")))
```

```
   c1 c2 c3
r1  1  2  3
r2  4  5  6
r3  7  8  9
```

or after the matrix is created.


```r
> m1 <- matrix(c(1,2,3,2,3,4,3,4,5),ncol = 3)
> rownames(m1) <- c("r1","r2","r3")
> colnames(m1) <- c("c1","c2","c3")
```

Here we encounter two new things: a *list* and a type of function like `rownames(...) <-`. We will discuss them later.

## Subsetting matrix

Like how we deal with vectors, we need not only create matrices but also extract data from a matrix. It is called *matrix subsetting*. 

Note that a matrix is a vector that is represented and accessible in two dimensions, we not only view a matrix in two dimensions, but also access it with a two-dimensional accessor `[,]`, which is quite similar to the one dimensional accessor for subsetting vectors `[]`.

To use it we can supply two vectors for each dimension to determine a subset of a matrix. The first argument in the square bracket means the row selector, and the second means the column selector. As what we tried for vectors, numeric vectors and character vectors both work.


```r
> m1
```

```
   c1 c2 c3
r1  1  2  3
r2  2  3  4
r3  3  4  5
```

```r
> m1[1,]
```

```
c1 c2 c3 
 1  2  3 
```

```r
> m1[,2]
```

```
r1 r2 r3 
 2  3  4 
```

```r
> m1[1,1]
```

```
[1] 1
```

```r
> m1[1:2,]
```

```
   c1 c2 c3
r1  1  2  3
r2  2  3  4
```

```r
> m1[,2:3]
```

```
   c2 c3
r1  2  3
r2  3  4
r3  4  5
```

```r
> m1[1:2,2:3]
```

```
   c2 c3
r1  2  3
r2  3  4
```

```r
> m1[-1,]
```

```
   c1 c2 c3
r2  2  3  4
r3  3  4  5
```

```r
> m1[,-2]
```

```
   c1 c3
r1  1  3
r2  2  4
r3  3  5
```

```r
> m1[c("r1","r3"),c("c1","c3")]
```

```
   c1 c3
r1  1  3
r3  3  5
```

Note again that a matrix is a vector that is represented and accessible in two dimensions, it still is a vector in its nature though. This allows us to use one dimensional accessor for vectors to subset a matrix.


```r
> m1[1]
```

```
[1] 1
```

```r
> m1[9]
```

```
[1] 5
```

```r
> m1[3:7]
```

```
[1] 3 2 3 4 3
```

Since a vector only contains entries of the same type, so does a matrix. Therefore, their operations are quite similar. If you type an inequality it will return another logical matrix of equal size.


```r
> m1 > 3
```

```
      c1    c2    c3
r1 FALSE FALSE FALSE
r2 FALSE FALSE  TRUE
r3 FALSE  TRUE  TRUE
```

We can use an equal-sized logical matrix for subsetting as if it is a vector.


```r
> m1[m1 > 3]
```

```
[1] 4 4 5
```

## Operators for matrices

All arithmetic operators for vectors also work with matrices as if they were vectors. These operators perform calculations element-wise except for matrix-only operators, such as matrix product `%*%`.


```r
> m1 + m1
```

```
   c1 c2 c3
r1  2  4  6
r2  4  6  8
r3  6  8 10
```

```r
> m1 - 2*m1
```

```
   c1 c2 c3
r1 -1 -2 -3
r2 -2 -3 -4
r3 -3 -4 -5
```

```r
> m1 * m1
```

```
   c1 c2 c3
r1  1  4  9
r2  4  9 16
r3  9 16 25
```

```r
> m1 / m1
```

```
   c1 c2 c3
r1  1  1  1
r2  1  1  1
r3  1  1  1
```

```r
> m1^2
```

```
   c1 c2 c3
r1  1  4  9
r2  4  9 16
r3  9 16 25
```

```r
> m1 %*% m1
```

```
   c1 c2 c3
r1 14 20 26
r2 20 29 38
r3 26 38 50
```

We can also transpose a matrix by `t()`.


```r
> t(m1)
```

```
   r1 r2 r3
c1  1  2  3
c2  2  3  4
c3  3  4  5
```
