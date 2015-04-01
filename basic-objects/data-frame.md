

# Data frame

A data frame is a table with a number of rows and columns. It looks like a matrix but its columns are not necessarily the same type. This is consistent with most data tables: a row, or data record is described by multiple columns of different types.

The following table is an example that can be fully characterized by a data frame.

| Name     | Gender | Age | Major            |
|----------|--------|-----|------------------|
| Ken      | Male   | 24  | Finance          |
| Ashley   | Female | 25  | Statistics       |
| Jennifer | Female | 23  | Computer Science |

## Creating data frame

To create such a data frame, we can call `data.frame` function and supply the data in each column.


```r
> persons <- data.frame(Name=c("Ken","Ashley","Jennifer"),
+   Gender=c("Male","Female","Female"),
+   Age=c(24,25,23),
+   Major=c("Finance","Statistics","Computer Science"))
> persons
```

```
      Name Gender Age            Major
1      Ken   Male  24          Finance
2   Ashley Female  25       Statistics
3 Jennifer Female  23 Computer Science
```

Note that creating a data frame is exactly the same with creating a list. It is because in essence a data frame *is* a list in which each member represents a column and has the same number of elements.

Other than creating a data frame from raw data, we can also create it from a list by calling either `data.frame` directly or `as.data.frame`.


```r
> l1 <- list(x=c(1,2,3),y=c("a","b","c"))
> data.frame(l1)
```

```
  x y
1 1 a
2 2 b
3 3 c
```

```r
> as.data.frame(l1)
```

```
  x y
1 1 a
2 2 b
3 3 c
```

We can also create a data frame from a matrix with the same method.


```r
> m1 <- matrix(c(1,2,3,4,5,6,7,8,9),nrow=3,byrow=FALSE)
> data.frame(m1)
```

```
  X1 X2 X3
1  1  4  7
2  2  5  8
3  3  6  9
```

```r
> as.data.frame(m1)
```

```
  V1 V2 V3
1  1  4  7
2  2  5  8
3  3  6  9
```

Note that the conversion also automatically assign column names to the new data frame. In fact, as you may verify, if the matrix already has column names or row names, they will be preserved in the conversion.

## Naming rows and columns

Since a data frame is a list but also looks like a matrix, the way we access these two types of objects both apply to data frame.


```r
> df1 <- data.frame(id=1:5,x=c(0,2,1,-1,-3),y=c(0.5,0.2,0.1,0.5,0.9))
> df1
```

```
  id  x   y
1  1  0 0.5
2  2  2 0.2
3  3  1 0.1
4  4 -1 0.5
5  5 -3 0.9
```

We can rename the columns and rows just like how we do so with a matrix.


```r
> colnames(df1) <- c("id","level","score")
> rownames(df1) <- letters[1:5]
> df1
```

```
  id level score
a  1     0   0.5
b  2     2   0.2
c  3     1   0.1
d  4    -1   0.5
e  5    -3   0.9
```

## Subsetting data frame

Since data frame is a matrix-like list of columns, we can use both set of notations to access the elements and subsets in a data frame.

### Subsetting as a list

If we would like to regard a data frame as a list of columns, we can use list notations to extract value or perform subsetting.

For example, we can use the dollar-sign to extract the value of one column by name, or use double square brackets to do so by index.


```r
> df1$id
```

```
[1] 1 2 3 4 5
```

```r
> df1[[1]]
```

```
[1] 1 2 3 4 5
```

List subsetting perfectly applies to a data frame and also yields new data frame. The single square brackets allows us to use an integer vector to extract columns by index, a character vector to extract columns by name, or a logical vector to extract columns by `TRUE` and `FALSE` selection.


```r
> df1[1]
```

```
  id
a  1
b  2
c  3
d  4
e  5
```

```r
> df1[1:2]
```

```
  id level
a  1     0
b  2     2
c  3     1
d  4    -1
e  5    -3
```

```r
> df1["level"]
```

```
  level
a     0
b     2
c     1
d    -1
e    -3
```

```r
> df1[c("id","score")]
```

```
  id score
a  1   0.5
b  2   0.2
c  3   0.1
d  4   0.5
e  5   0.9
```

```r
> df1[c(TRUE,FALSE,TRUE)]
```

```
  id score
a  1   0.5
b  2   0.2
c  3   0.1
d  4   0.5
e  5   0.9
```

### Subsetting as a matrix

However, the list notation does not support row access. In contrast, the matrix notation provides more flexibility. If we view a data frame as a matrix, the two-dimensional accessor enables us to easily access an entry of a subset, which supports both column selection and row selection.

In other words, we can use `[row,column]` notation to subset a data frame by specifying the row selector and column selector, which can be integer vectors, character vectors, and/or logical vectors.

For example, we can specify the column selector:


```r
> df1[,"level"]
```

```
[1]  0  2  1 -1 -3
```

```r
> df1[,c("id","level")]
```

```
  id level
a  1     0
b  2     2
c  3     1
d  4    -1
e  5    -3
```

```r
> df1[,1:2]
```

```
  id level
a  1     0
b  2     2
c  3     1
d  4    -1
e  5    -3
```

or the row selector:


```r
> df1[1:4,]
```

```
  id level score
a  1     0   0.5
b  2     2   0.2
c  3     1   0.1
d  4    -1   0.5
```

```r
> df1[c("c","e"),]
```

```
  id level score
c  3     1   0.1
e  5    -3   0.9
```

or both selectors at the same time:


```r
> df1[,"id"]
```

```
[1] 1 2 3 4 5
```

```r
> df1[1:4,"id"]
```

```
[1] 1 2 3 4
```

```r
> df1[1:3,c("id","score")]
```

```
  id score
a  1   0.5
b  2   0.2
c  3   0.1
```

Note that the matrix notation automatically simplifies the output. That is, if only one column is selected, the result won't be a data frame but the value of that column. To keep the result always being a data frame even if it only has a single column, we can use both notations together.


```r
> df1[1:4,]["id"]
```

```
  id
a  1
b  2
c  3
d  4
```

Here the first group of brackets subsets the data frame as a matrix with the first four rows and all columns selected. The second group of brackes subsets the resulted data frame as a list with only `id` column selected, which results in a data frame.

### Data filtering

The following code filters the rows of `df1` by a criterion that `score >= 0.5` and selects `id` and `level` columns:


```r
> df1$score >= 0.5
```

```
[1]  TRUE FALSE FALSE  TRUE  TRUE
```

```r
> df1[df1$score>=0.5,c("id","level")]
```

```
  id level
a  1     0
d  4    -1
e  5    -3
```

The following code filters the rows of `df1` by a criterion that the row name must be among a, d, or e, and selects the `id` and `score` columns.


```r
> rownames(df1) %in% c("a","d","e")
```

```
[1]  TRUE FALSE FALSE  TRUE  TRUE
```

```r
> df1[rownames(df1) %in% c("a","d","e"),c("id","score")]
```

```
  id score
a  1   0.5
d  4   0.5
e  5   0.9
```

Both examples above basically uses matrix notation to select rows by logical vector and select columns by character vector.

## Setting values

Setting the values or a subset of a data frame is almost the same as how we do so with either a list or a matrix.

### Setting values as a list

We can assign new values to a list member using `$` and `<-` together.


```r
> df1$score <- c(0.6,0.3,0.2,0.4,0.8)
> df1
```

```
  id level score
a  1     0   0.6
b  2     2   0.3
c  3     1   0.2
d  4    -1   0.4
e  5    -3   0.8
```

Alternatively, single square brackets work too, but it also allows multiple changes in one expression in contrast with double square brackets.


```r
> df1["score"] <- c(0.8,0.5,0.2,0.4,0.8)
> df1
```

```
  id level score
a  1     0   0.8
b  2     2   0.5
c  3     1   0.2
d  4    -1   0.4
e  5    -3   0.8
```

```r
> df1[["score"]] <- c(0.4,0.5,0.2,0.8,0.4)
> df1
```

```
  id level score
a  1     0   0.4
b  2     2   0.5
c  3     1   0.2
d  4    -1   0.8
e  5    -3   0.4
```

```r
> df1[c("level","score")] <- list(level=c(1,2,1,0,0),score=c(0.1,0.2,0.3,0.4,0.5))
> df1
```

```
  id level score
a  1     1   0.1
b  2     2   0.2
c  3     1   0.3
d  4     0   0.4
e  5     0   0.5
```

### Setting values as a matrix

Using list notations to set values of a data frame has the same problem with subsetting. We can only access the columns. If we need to set values with more flexibility, we can use matrix notations.


```r
> df1[1:3,"level"] <- c(-1,0,1)
> df1
```

```
  id level score
a  1    -1   0.1
b  2     0   0.2
c  3     1   0.3
d  4     0   0.4
e  5     0   0.5
```

```r
> df1[1:2,c("level","score")] <- list(level=c(0,0),score=c(0.9,1.0))
> df1
```

```
  id level score
a  1     0   0.9
b  2     0   1.0
c  3     1   0.3
d  4     0   0.4
e  5     0   0.5
```

## Factor

One thing to notice is that the default behavior of a data frame tries to use memory more efficiently. Sometimes this behavior might lead to unexpected problems silently.

For example, when we create a data frame by supplying a character vector as a column, it  will by default convert the character vector to a **factor** that only stores the same value once so that repetitions will not cost much memory.

We can verify this by calling `str` on data frame `persons` we created in the beginning.


```r
> str(persons)
```

```
'data.frame':	3 obs. of  4 variables:
 $ Name  : Factor w/ 3 levels "Ashley","Jennifer",..: 3 1 2
 $ Gender: Factor w/ 2 levels "Female","Male": 2 1 1
 $ Age   : num  24 25 23
 $ Major : Factor w/ 3 levels "Computer Science",..: 2 3 1
```

As we can clearly find out that `Name`, `Gender`, and `Major` are not character vectors but factor object. It is reasonable that `Gender` is represented by a factor because it may only be either `Female` or `Male` so that using two integers to represent these two values is more efficient than using character vector to store all the values regardless of the repetition.

However, it may induce problems for other columns not limited to take several possible values. For example, if we want to set a `Name` in `persons`,


```r
> persons[1,"Name"] <- "John"
```

```
Warning in `[<-.factor`(`*tmp*`, iseq, value = "John"): invalid factor
level, NA generated
```

```r
> persons
```

```
      Name Gender Age            Major
1     <NA>   Male  24          Finance
2   Ashley Female  25       Statistics
3 Jennifer Female  23 Computer Science
```

a warning message appears. It happens because in the initial `Name` dictionary, there is no word called John, therefore we cannot set the name of the first person to be such "non-existing" value. The same thing happens when we set any `Gender` to be `Unknown`. The reason is  exactly the same: when the column is initially created from a character vector while we define a data frame, the column will by default be a factor whose value must be taken from the dictionary created from the unique values in that character vector.

This behavior is sometimes very annoying and does not really help much, especially when memory is cheap today. The simplist way to avoid this behavior is to set `stringsAsFactors = FALSE` when we create a data frame.


```r
> persons <- data.frame(Name=c("Ken","Ashley","Jennifer"),
+   Gender=factor(c("Male","Female","Female")),
+   Age=c(24,25,23),
+   Major=c("Finance","Statistics","Computer Science"),
+   stringsAsFactors=FALSE)
> str(persons)
```

```
'data.frame':	3 obs. of  4 variables:
 $ Name  : chr  "Ken" "Ashley" "Jennifer"
 $ Gender: Factor w/ 2 levels "Female","Male": 2 1 1
 $ Age   : num  24 25 23
 $ Major : chr  "Finance" "Statistics" "Computer Science"
```

If we really want factor object to play its role, we can explicitly call `factor` function at specific columns, just like what we do for `Gender` column above.

## Useful functions for data frame

There are many useful functions for data frame. Here we only introduce a few but the most commonly used ones.

`summary` function works with a data frame by generating a table that shows the summary statistics of each column.


```r
> summary(persons)
```

```
     Name              Gender       Age          Major          
 Length:3           Female:2   Min.   :23.0   Length:3          
 Class :character   Male  :1   1st Qu.:23.5   Class :character  
 Mode  :character              Median :24.0   Mode  :character  
                               Mean   :24.0                     
                               3rd Qu.:24.5                     
                               Max.   :25.0                     
```

For a factor (`Gender`) the summary counts the number of rows taking each value, or level. For a numeric vector, the summary shows the important quantiles of the numbers. For other types of columns, it shows the length, class, and mode of them.

`rbind` and `cbind`, as their names suggest, perform row binding and column binding respectively.


```r
> rbind(persons,data.frame(Name="John",Gender="Male",Age=25,Major="Statistics"))
```

```
      Name Gender Age            Major
1      Ken   Male  24          Finance
2   Ashley Female  25       Statistics
3 Jennifer Female  23 Computer Science
4     John   Male  25       Statistics
```

```r
> cbind(persons,Registered=c(TRUE,TRUE,FALSE),Projects=c(3,2,3))
```

```
      Name Gender Age            Major Registered Projects
1      Ken   Male  24          Finance       TRUE        3
2   Ashley Female  25       Statistics       TRUE        2
3 Jennifer Female  23 Computer Science      FALSE        3
```

`expand.grid` function generates a data frame that includes all combinations of the values in the columns.


```r
> expand.grid(type=c("A","B"),class=c("M","L","XL"))
```

```
  type class
1    A     M
2    B     M
3    A     L
4    B     L
5    A    XL
6    B    XL
```

## Load from/Write to a file

In practice, data are usually stored in files. R provides a number of functions to read table from a file or write a data frame to a file. If a file stores a data table, it is often well organized and follow some convention that specifies how rows and columns are arranged. In most cases, we don't have to read a file byte to byte but call functions like `read.table` or `read.csv`.

The most popular software-neutral data format is CSV (Comma-Separated Values). The format is basically organized in a way that values in different columns are separated by comma and the first row is by default regarded as the header. For example, `persons` may be represented in the following CSV format:

```
Name,Gender,Age,Major
Ken,Male,24,Finance
Ashley,Female,25,Statistics
Jennifer,Female,23,Computer Science
```

To read the data into R environment, we only need to call `read.csv(file)` where `file` is the path of the file.


```r
> read.csv("data/persons.csv")
```

```
      Name Gender Age            Major
1      Ken   Male  24          Finance
2   Ashley Female  25       Statistics
3 Jennifer Female  23 Computer Science
```

If we need to save a data frame to a CSV file, we may call `write.csv(file)` with some additional arguments.


```r
> write.csv(persons,"data/persons.csv",row.names=FALSE,quote=FALSE)
```

The argument `row.names=FALSE` avoids storing the row names which are not necessary, and the argument `quote=FALSE` avoids quoting texts in the output, which in most cases are not necessary either.
