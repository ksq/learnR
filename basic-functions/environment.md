

# Environment functions

In R, every expression is evaluated within a specific environment. An environment is a collection of symbols and their bindings. When we bind a value to a symbol, call a function, or refer to a name, R will first find the symbols in the current environment. If you type commands in the RStudio console, your commands are evaluated in the *Global Environment*.

It is helpful to know some functions that work with environment.

`objects()` or equivalently, `ls()` returns a character vector of the existing symbols in the current environment.


```r
> objects()
```

```
character(0)
```

```r
> x <- 1
> y <- 1
> objects()
```

```
[1] "x" "y"
```

`remove()` or equivalently, `rm()` removes existing symbols.


```r
> remove(x)
> rm(y)
> objects()
```

```
character(0)
```

If we want to clear all the bindings in the current environment, we call like this:


```r
> rm(list=objects())
```

Sometimes we need to get the current working directoy to ensure that we can load a file without specifying the absolute path, we can call `getwd()`. If we need to manually change the working directory, we can call `setwd()`.

There are some other functions that do not work with environments but function in global scale. An example is global options which are functioning in the current R session.

For instance, the numberic digits are set by default to 7. Sometimes it is not sufficient when we deal with some big numbers that need higher precision. We can call `options()` to change the `digits` option to a larger number.


```r
> getOption("digits")
```

```
[1] 7
```

```r
> 1e10 + 0.5
```

```
[1] 1e+10
```

```r
> options(digits=15)
> 1e10 + 0.5
```

```
[1] 10000000000.5
```

```r
> options(digits=7)
> 1e10 + 0.5
```

```
[1] 1e+10
```

There are some other important functions related to environment manipulation. However, it is beyond basic level so we will cover this topic in detail in the advanced chapters.
