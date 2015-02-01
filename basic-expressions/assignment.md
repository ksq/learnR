

# Assignment

Assignment may be one of the most fundamental expressions in all programming languages. What it does is to assign, or bind a value to a symbol so that we can refer to the value by that symbol later.

Despite the similarity, R adopts the operator `<-` to perform assignment. This is a bit different from many other languages which use `=`, although it is also allowed in R.


```r
> x <- 1
> y <- c(1,2,3)
> z <- list(x,y)
```

We don't have to declare the symbol and its type before assigning a value to it. If a symbol does not exist in the environment, assignment will create that symbol. If a symbol already exists, assignment will not end up in conflict but rebind the new value to that symbol.

There are some alternative yet equivalent operators we can use.


```r
> 2 -> x1
> x2 = c(1,2,3)
```

We can even chain the assignment operators so that a set of symbols all take the same value.


```r
> x3 <- x2 <- x1 <- 0
```

The expression `0` is evaluated only once so that the same value is assigned to the three symbols. To verify how it works, we can change `0` to a random number generator.


```r
> x3 <- x2 <- x1 <- rnorm(1)
> c(x1,x2,x3)
```

```
[1] -0.5021924 -0.5021924 -0.5021924
```

`rnorm(1)` generates a random number following the standard normal distribution. If each assignment re-invokes the random number generator, each symbol will have different values. But in fact, it does not happen. Later we will explain what really happens and you will have a better understanding on it.

## Quotation

Assignment operators allow us to assign a value to a symbol (or name). However, direct assignment limits the format of the name. A symbol must start with a to z or ., and cannot directly contain spaces. Otherwise, we need to use quotation marks to use any name we want.


```r
> `_a?` <- 1
```

The quotation marks can be used in any place where we create a symbol.


```r
> `-a` <- function(a,d) { a-d }
> l1 <- list(`a+-`=rnorm(10),`a-+`=rnorm(10))
```

If the symbol name cannot be validly referred to directly, we also need to use the quotation marks to refer to the symbol.


```r
> `-a`(1,2)
```

```
[1] -1
```

```r
> l1$`a+-`
```

```
 [1]  0.13153117 -0.07891709  0.88678481  0.11697127  0.31863009
 [6] -0.58179068  0.71453271 -0.82525943 -0.35986213  0.08988614
```

An exception is `data.frame`. Even if we use quotation marks around a name with unusual symbols, the resulted `data.frame` will replace those symbols with `.`.


```r
> data.frame(`a-+`=rnorm(10),`a+-`=rnorm(10))
```

```
           a..      a...1
1   0.76406062  1.7573756
2   0.26196129 -0.1379296
3   0.77340460 -0.1111935
4  -0.81437912 -0.6900143
5  -0.43845057 -0.2217942
6  -0.72022155  0.1829077
7   0.23094453  0.4173233
8  -1.15772946  1.0654023
9   0.24707599  0.9702020
10 -0.09111356 -0.1016292
```
