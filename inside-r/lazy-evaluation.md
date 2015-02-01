

# Lazy evaluation

R adopts a mechanism called lazy evaluation. The following example shows how expressions are laziy evaluated.

Suppose we define such a function.


```r
fun <- function(x,y=m) {
  m <- x+1
  c(x,y)
}
```

Note that the default value of the second argument `y` is `m` which does not exist before calling the function. Let's see what happens when we call it.


```r
fun(1)
```

```
[1] 1 2
```

Why the function does not end up in error? Intuitively, when we call `fun(1)`, the value of all arguments should be determined. In other words, the local environment of the function should be initiated with all arguments evaluated, that is `x=1` and `y=m`. Since `m` does not exist prior to the call, `y=m` should produce an error. But why it does not?

The above mechanism is intuitive but not true for R interpreter. R adopts a mechanism called *lazy evaluation*, that is, the expression is evaluated only at their first call.

Let's keep it in mind and see what really happens when we call `fun(1)`. The following bullets analytically show how exactly the function is run.

- Find a function named `fun`
- Match given arguments so that `x` takes 1, and `y` still is an unevaluated expression `m`.
- Within in local environment, the value of `x+1` is assigned to previously undefined symbol `m`.
- Finally, when evaluating the last expression `c(x,y)`, `x=1` is known, and `y` is at its first call and is evaluated by its definition `y=m` where `m` has been assigned `x+1`, which is 2.
- A numeric vector `c(1,2)` is returned.

Therefore in the whole computing process, no warning or error occurs because no rules are violated.

