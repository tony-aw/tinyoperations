
- <a href="#tidyoperators" id="toc-tidyoperators">tidyoperators</a>
  - <a href="#installation" id="toc-installation">Installation</a>
- <a href="#overview" id="toc-overview">Overview</a>
- <a href="#simple-additional-logicals"
  id="toc-simple-additional-logicals">Simple additional logicals</a>
- <a href="#safer-float-inequality-operators"
  id="toc-safer-float-inequality-operators">Safer float (in)equality
  operators</a>
- <a href="#in-place-modifying-mathematical-arithmetic"
  id="toc-in-place-modifying-mathematical-arithmetic">In-place modifying
  mathematical arithmetic</a>
  - <a href="#the-problem" id="toc-the-problem">The problem</a>
  - <a href="#the-solution-from-this-r-package"
    id="toc-the-solution-from-this-r-package">The solution from this R
    package</a>
  - <a href="#comparisons" id="toc-comparisons">Comparisons</a>
- <a href="#unreal-replacement" id="toc-unreal-replacement">Unreal
  replacement</a>
- <a href="#basic-string-operations"
  id="toc-basic-string-operations">Basic string operations</a>
  - <a href="#string-subsetting" id="toc-string-subsetting">String
    subsetting</a>
  - <a href="#string-arithmetic" id="toc-string-arithmetic">String
    arithmetic</a>
  - <a href="#in-place-modifying-string-arithmetic-and-subsetting"
    id="toc-in-place-modifying-string-arithmetic-and-subsetting">In-place
    modifying string arithmetic and subsetting</a>
- <a href="#more-advanced-string-operations"
  id="toc-more-advanced-string-operations">More advanced string
  operations</a>
  - <a href="#pattern-attributes-in-strings"
    id="toc-pattern-attributes-in-strings">Pattern attributes in strings</a>
  - <a href="#using-stringi-with-tidyoperators"
    id="toc-using-stringi-with-tidyoperators">Using stringi with
    tidyoperators</a>
  - <a href="#more-string-flexibility-s_strapply"
    id="toc-more-string-flexibility-s_strapply">More string flexibility:
    s_strapply</a>
  - <a href="#parallel-computing--multi-threading-in-string-functions"
    id="toc-parallel-computing--multi-threading-in-string-functions">Parallel
    computing / multi-threading in string functions</a>
- <a href="#recommended-r-packages"
  id="toc-recommended-r-packages">Recommended R packages</a>
- <a href="#conclusion" id="toc-conclusion">Conclusion</a>

<!-- README.md is generated from README.Rmd. Please edit that file -->

# tidyoperators

<!-- badges: start -->

[![R build
status](https://github.com/tony-aw/tidyoperators/workflows/R-CMD-check/badge.svg)](https://github.com/tony-aw/tidyoperators/actions)
[![Project Status: Active - The project has reached a stable, usable
state and is being actively
developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![](https://img.shields.io/badge/ORCID-0000--0001--9498--8379-green.svg)](https://orcid.org/0000-0001-9498-8379)
<!-- badges: end -->

![](tidyoperators.svg)

The ‘tidyoperators’ R-package adds some much needed infix operators, and
a few functions, to make your R code much more tidy. It includes infix
operators for the negation of logical operators (exclusive-or, not-and,
not-in), safer float (in)equality operators, in-place modifying
mathematical arithmetic, string arithmetic, string sub-setting, in-place
modifying string arithmetic, in-place modifying string sub-setting, and
in-place modifying unreal replacers. Moreover, it includes some helper
functions for more complex string arithmetic, some of which are missing
from popular R packages like stringi. All base R expressions options
(Regex, Perl, fixed, useBytes, ignore.case) are available for all
string-pattern-related functions, as well as all options from stringi
(regex, fixed, boundary, charclass, coll). This package also allows
integrating third-party parallel computing packages for some of its
functions.

## Installation

You can install `tidyoperators` from github like so:

``` r
library(devtools)
devtools::install_github("https://github.com/tony-aw/tidyoperators")
```

Then load it using:

``` r
library(tidyoperators)
```

# Overview

The `tidyoperators` R package adds the following functionality:

- Infix logical operators for exclusive-or, not-and, not-in,
  number-type, and string-type.
- Safer (in)equality operators for floating numbers.
- Infix operators for In-place modifiers for mathematical arithmetic.
- Infix operators for String arithmetic.
- Infix operators (and a few functions) for string sub-setting.
- Infix operators for In-place modifying string arithmetic.
- Infix operators for In-place modifying string sub-setting.
- Some additional string manipulation functions.
- All base R expressions options (Regex, Perl, fixed, useBytes,
  ignore.case) are available for all string-pattern-related functions,
  as well as all options from stringi (regex, fixed, boundary,
  charclass, coll).
- Allow multi-threading of functions (when appropriate) through
  third-party packages to improve efficiency.
- This R package has **zero dependencies**, thus avoiding the so-called
  dependency hell.
- Although this package has zero dependencies, it can use patterns from
  the `stringi` R package, provided the user has the `stringi` R-package
  installed.

I realize there are other R-packages that cover some of the above
functionalities. But I often experience that these R packages (or at
least those I know of) either do not cover all that I required, had some
inconsistencies, or suffered from some other significant drawbacks.
Hence this package was created.

Currently this R package is only available on GitHub.

I understand one may not want to go through this entire Read-Me without
knowing if the R package is worthy of your time. Therefore, allow me to
give you a quick glimpse of what is possible in this R package before
jumping into the details.

First, some in-place mathematical arithmetic with `tidyoperators`:

``` r
x <- 1:10
print(x)
#>  [1]  1  2  3  4  5  6  7  8  9 10
x %^ <-% 2 # is the same as x <- x^2
print(x)
#>  [1]   1   4   9  16  25  36  49  64  81 100
```

Second, some numeric and string sub-types checks:

``` r
n <- c(0:5, 0:-5, 0.1, -0.1, 0, 1, Inf, -Inf, NA, NaN)
n[n %=numtype% "B"]
#> [1] 0 1 0 0 1
n[n %=numtype% "N"]
#> [1] 0 1 2 3 4 5 0 0 1
n[n %=numtype% "I"]
#>  [1]  0  1  2  3  4  5  0 -1 -2 -3 -4 -5  0  1
n[n %=numtype% "unreal"]
#> [1]  Inf -Inf   NA  NaN

s <- c(" AbcZ123 ", " abc ", " 1.3 ", " !#$%^&*() ", "  ", "  NA  ", "  NaN  ", " Inf ")
s[s %=strtype% "empty"]
#> [1] "  "
s[s %=strtype% "unreal"]
#> [1] "  NA  "  "  NaN  " " Inf "
s[s %=strtype% "numeric"]
#> [1] " 1.3 " " Inf "
s[s %=strtype% "special"]
#> [1] " !#$%^&*() "
```

And now some fun string manipulations using `tidyoperators`:

``` r
x <- c("Hello World", "Goodbye World")

# Capitalize ONLY the ODD indices of each string:
x <- c("Hello World", "Goodbye World")
s_strapply(x, fun=\(x){
  replace(x, seq(1, length(x), 2), toupper(x)[seq(1, length(x), 2)])
})
#> [1] "HeLlO WoRlD"   "GoOdByE WoRlD"

# Extract second-last vowel of every word of every string in a vector:
x <- c("Outrageous, egregious, preposterous!", "Pleasant evening everyone")
p <- s_pattern_b("a|e|i|o|u", ignore.case = TRUE)
s_strapply(x, w=T, fun=\(x)s_extract_ith(x, -2, p))
#> [1] "o o o" "a e o"
```

And some string arithmetic:

``` r
"Hello" %s+% " world"
#> [1] "Hello world"
c("Hello world", "Goodbye world") %s-% " world"
#> [1] "Hello"   "Goodbye"
c("Ha", "Ho", "Hi", "Hu", "He", "Ha") %s*% 10
#> [1] "HaHaHaHaHaHaHaHaHaHa" "HoHoHoHoHoHoHoHoHoHo" "HiHiHiHiHiHiHiHiHiHi"
#> [4] "HuHuHuHuHuHuHuHuHuHu" "HeHeHeHeHeHeHeHeHeHe" "HaHaHaHaHaHaHaHaHaHa"
c("HaHa", "Ho", "Hi", "Hu", "He", "Ha") %s/% "Ha"
#> [1] 2 0 0 0 0 1
```

And string sub-setting:

``` r
x <- c("yeay nay or nothing to say", "Goodmorning, goodevening and goodnight")
print(x)
#> [1] "yeay nay or nothing to say"            
#> [2] "Goodmorning, goodevening and goodnight"
p <- "a|e|i|o|u" # pattern for all vowels.

# replace the second vowel with question mark:
s_repl_ith(x, 2, p, "?")
#> [1] "ye?y nay or nothing to say"            
#> [2] "Go?dmorning, goodevening and goodnight"

# replace the second-last vowel with question mark:
s_repl_ith(x, -2, p, "?")
#> [1] "yeay nay or nothing t? say"            
#> [2] "Goodmorning, goodevening and go?dnight"
```

If you’re still interested, I invite you to read the rest of this
Read-Me and perhaps try out the package yourself.

 

# Simple additional logicals

The tidyoperators package adds a few basic logical operators:

- `%xor%`: Exclusive OR
- `%n&%`: NOT AND (i.e. `(!x) & (!y)`)
- `%out%`: the opposite of `%in%` (i.e. `!x %in% y`)
- `%?=%`: checks if both `x` and `y` are unknown or unreal (NA, NaN,
  Inf, -Inf)

Here are some examples:

``` r
x <- c(TRUE, FALSE, TRUE, FALSE, NA, FALSE, TRUE)
y <- c(FALSE, TRUE, TRUE, FALSE, NA, NA, NA)
cbind(x, y, "x %xor% y"=x %xor% y, "x %n&% y" = x %n&% y, "x %?=% y" = x %?=% y)
#>          x     y x %xor% y x %n&% y x %?=% y
#> [1,]  TRUE FALSE      TRUE    FALSE    FALSE
#> [2,] FALSE  TRUE      TRUE    FALSE    FALSE
#> [3,]  TRUE  TRUE     FALSE    FALSE    FALSE
#> [4,] FALSE FALSE     FALSE     TRUE    FALSE
#> [5,]    NA    NA        NA       NA     TRUE
#> [6,] FALSE    NA        NA       NA    FALSE
#> [7,]  TRUE    NA        NA    FALSE    FALSE

1:3 %out% 1:10
#> [1] FALSE FALSE FALSE
1:10 %out% 1:3
#>  [1] FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
```

Numbers can have many different sub-types whilst still being `numeric`.
The `n %=numtype% numtype` operator will check for every value of
numeric vector `n` if it can be considered a number belonging to type
`numtype`. The following values for `numtype` are allowed:

- “\~0”: zero, or else a number whose absolute value is smaller than the
  Machine tolerance (`sqrt(.Machine$double.eps)`);
- “B”: binary numbers (0 or 1);
- “prop”: proportions;
- “N”: Natural numbers (non-negative integers including zero);
- “I”: Integers;
- “odd”: odd integers;
- “even”: even integers
- “R”: Real numbers;
- “unreal”: infinity, NA, or NaN;

The string counterpart for `%=numtype%` is `s %=strtype% strtype`, which
checks for every value of character vector `s` if it can seen as a
certain `strtype`. The following values for `strtype` are allowed:

- “empty”: checks if the string only consists of empty spaces.
- “unreal”: checks if the string is NA, or if it has literal string
  “NA”, “NaN” or “Inf”, regardless if it has leading or trailing spaces.
- “numeric”: checks if the string can be converted to a number,
  disregarding leading and trailing spaces. I.e. the string “5.0” can be
  converted to the the actual number 5.0.
- “special”: checks if the string consists of only special characters.

Here are some examples:

``` r

1e-20 %=numtype% "~0"
#> [1] TRUE
n <- c(0:5, 0:-5, 0.1, -0.1, 0, 1, Inf, -Inf, NA, NaN)
n[n %=numtype% "B"]
#> [1] 0 1 0 0 1
n[n %=numtype% "prop"]
#> [1] 0.0 1.0 0.0 0.1 0.0 1.0
n[n %=numtype% "B"]
#> [1] 0 1 0 0 1
n[n %=numtype% "N"]
#> [1] 0 1 2 3 4 5 0 0 1
n[n %=numtype% "I"]
#>  [1]  0  1  2  3  4  5  0 -1 -2 -3 -4 -5  0  1
n[n %=numtype% "odd"]
#> [1]  1  3  5 -1 -3 -5  1
n[n %=numtype% "even"]
#> [1]  0  2  4  0 -2 -4  0
n[n %=numtype% "R"]
#>  [1]  0.0  1.0  2.0  3.0  4.0  5.0  0.0 -1.0 -2.0 -3.0 -4.0 -5.0  0.1 -0.1  0.0
#> [16]  1.0
n[n %=numtype% "unreal"]
#> [1]  Inf -Inf   NA  NaN

s <- c(" AbcZ123 ", " abc ", " 1.3 ", " !#$%^&*() ", "  ", "  NA  ", "  NaN  ", " Inf ")
s[s %=strtype% "empty"]
#> [1] "  "
s[s %=strtype% "unreal"]
#> [1] "  NA  "  "  NaN  " " Inf "
s[s %=strtype% "numeric"]
#> [1] " 1.3 " " Inf "
s[s %=strtype% "special"]
#> [1] " !#$%^&*() "
```

 

# Safer float (in)equality operators

This package adds the `%f==%, %f!=% %f<%, %f>%, %f<=%, %f>=%` operators,
which perform “float logic”. They are virtually equivalent to the
regular (in)equality operators, `==, !=, <, >, <=, >=`, except for one
aspect. The float logic operators assume that if the absolute difference
between `x` and `y` is smaller than the Machine tolerance,
`sqrt(.Machine$double.eps)`, then `x` and `y` ought to be consider to be
equal. Thus these provide safer float (in)equality operators. For
example: `(0.1*7) == 0.7` returns `FALSE`, even though they are equal,
due to the way floating numbers are stored in programming languages like
R. But `(0.1*7) %f==% 0.7` returns `TRUE`.

Some examples:

``` r
x <- c(0.3, 0.6, 0.7)
y <- c(0.1*3, 0.1*6, 0.1*7)
print(x); print(y)
#> [1] 0.3 0.6 0.7
#> [1] 0.3 0.6 0.7
x == y # gives FALSE, but should be TRUE
#> [1] FALSE FALSE FALSE
x!= y # gives TRUE, should be FALSE
#> [1] TRUE TRUE TRUE
x > y # not wrong
#> [1] FALSE FALSE FALSE
x < y # gives TRUE, should be FALSE
#> [1] TRUE TRUE TRUE
x %f==% y # here it's done correctly
#> [1] TRUE TRUE TRUE
x %f!=% y
#> [1] FALSE FALSE FALSE
x %f<% y # correct
#> [1] FALSE FALSE FALSE
x %f>% y # correct
#> [1] FALSE FALSE FALSE
x %f<=% y # correct
#> [1] TRUE TRUE TRUE
x %f>=% y # correct
#> [1] TRUE TRUE TRUE
```

Although designed for objects (vectors, matrices, arrays) of class
`double` (floating numbers), these operators also work correctly for
integers. These operators do not work for non-numeric objects.

 

# In-place modifying mathematical arithmetic

## The problem

This R package includes infix operators for in-place modifying
mathematical arithmetic. But first: what is an in-place modifier?
Consider the following line of code:

``` r
mtcars$mpg[mtcars$cyl>6] <- mtcars$mpg[mtcars$cyl>6]^2
```

The same expression, `mtcars$mpg[mtcars$cyl>6]`, is written twice,
making this code rather long and cumbersome, even though we’re just
squaring the expression. The well-known `magrittr` R-package has an
in-place modifier pipe, `%<>%`. This works excellent for actual
functions, like so:

``` r
mtcars$mpg[mtcars$cyl>6] %<>% mean() # same as mtcars$mpg[mtcars$cyl>6] <- mean(mtcars$mpg[mtcars$cyl>6])
```

But for arithmetic, one needs to translate the operators into a
function, and then perform the in-place modifier pipe:

``` r
mtcars$mpg[mtcars$cyl>6] %<>% raise_to_power(2)
```

This is better, but still not truly tidy.

 

## The solution from this R package

This R package solves the above laid-out problem by implementing
in-place modifying mathematical arithmetic for all mathematical
operators, excluding matrix operators.

Here is a list of all in-place mathematical modifiers implemented in
this R-package:

- `x %+ <-% y` is the same as`x <- x + y`;
- `x %- <-% y` is the same as`x <- x - y`;
- `x %* <-% y` is the same as`x <- x * y`;
- `x %/ <-% y` is the same as`x <- x / y`;
- `x %^ <-% p` is the same as`x <- x^p`;
- `x %rt <-% p` is the same as`x <- x^(1/p)`;
- `x %logb <-% b` is the same as`x <- log(x, base=b)`;
- `x %alogb <-% b` is the same as`x <- b^x`; if `b=exp(1)`, this is the
  same as`x <- exp(x)`;
- `x %alogb <-% exp(1)` is the same as exp(x)\`.

I realize there doesn’t really need to be a `%logb%` operator, but since
one was needed for the antilogarithm, I added the “regular” logarithm
also, for consistency. Notice that all in-place modifiers end with `<-%`
(notice the space). All infix operators that are in-place modifiers in
this R package end with `<-%`, so that it is clear what it does.

Lets look at the original problem:

``` r
mtcars$mpg[mtcars$cyl>6] <- mtcars$mpg[mtcars$cyl>6]^2
```

With `tidyoperators` one can now make this more tidy with the following:

``` r
mtcars$mpg[mtcars$cyl>6] %^ <-% 2
```

Much tidier, no?

 

## Comparisons

This is not the first or only R package that incorporates in-place
modifiers. Most notably, the `inplace` R package is devoted entirely to
in-place modifying mathematical arithmetic. However, `inplace` has a
nasty side-effect:

If 2 R objects refer to the same values - let’s say `x = 3` and
`y = 3` - using an in-place modifier from the `inplace` package on `x`
will also change `y`. This can be very dangerous.

**The `tidyoperators` R package does not have this problem:** modifying
one object does not affect another object, even if they happen to have
the same value.

 

# Unreal replacement

Another operator added by `tidyoperators` is `x %unreal <-% y`, which
replaces all NA, NaN, Inf and -Inf in `x` with the value given in `y`.

It is the same as `x[is.na(x)|is.nan(x)|is.infinite(x)] <- y`.

 

# Basic string operations

The `tidyoperators` R package implements operators for string arithmetic
and sub-setting, as well some of their in-place modifier equivalents.
For consistency, and to avoid masking other common operators, all
string-related operators start with `%s`, where the “s” stands for
“string”. Likewise, all string-related functions in this package start
with `s_`.

 

## String subsetting

As a first sub-setting operator, we have `x %sget% ss`, which returns a
subset of each string in character vector `x`. Here `ss` is a vector of
length 2, or a matrix with `nrow(ss)=length(x)` and 2 columns. The
object `ss` should consist entirely of non-negative integers (thus 0, 1,
2, etc. are valid, but -1, -2, -3 etc are not valid). The first
element/column of ss gives the number of characters counting from the
left side to be extracted from x. The second element/column of ss gives
the number of characters counting from the right side to be extracted
from x.

Here are 2 examples:

``` r
x <- c(paste0(letters[1:13], collapse=""), paste0(letters[14:26], collapse=""))
print(x)
#> [1] "abcdefghijklm" "nopqrstuvwxyz"
ss <- c(2,3)
x %sget% ss
#> [1] "abklm" "noxyz"

x <- c(paste0(letters[1:13], collapse=""), paste0(letters[14:26], collapse=""))
print(x)
#> [1] "abcdefghijklm" "nopqrstuvwxyz"
ss <- c(1,0)
x %sget% ss
#> [1] "a" "n"
```

Thus `%sget%` “gets” or extracts the given number of characters from the
left and the right, and removes the rest. There is also `%strim%`, which
is the opposite: it trims away the number of characters from the left
and right as defined in `ss`, leaving you with whatever is left.

Here are again 2 examples:

``` r
x <- c(paste0(letters[1:13], collapse=""), paste0(letters[14:26], collapse=""))
print(x)
#> [1] "abcdefghijklm" "nopqrstuvwxyz"
ss <- c(2,3)
x %strim% ss
#> [1] "cdefghij" "pqrstuvw"

x <- c(paste0(letters[1:13], collapse=""), paste0(letters[14:26], collapse=""))
print(x)
#> [1] "abcdefghijklm" "nopqrstuvwxyz"
ss <- c(1,0)
x %strim% ss
#> [1] "bcdefghijklm" "opqrstuvwxyz"
```

 

There are also 3 functions regarding string sub-setting:
`s_extract_ith(x, i, p)`, `s_locate_ith(x, i, p, type)`, and
`s_repl_ith(x, i, p, rp)`.

The first function, `s_extract_ith(x, i, p)`, extracts the $i^{th}$
occurrence of pattern `p` in every string of character vector `x`. When
`i` is positive, the occurrence is counted from left to right. Negative
values for `i` are also allowed, in which case the occurrence is counted
from the right to left. Thus, to get the **last** occurrence, use
`i=-1`. But `i=0` is not allowed though.

The `s_locate_ith(x, i, p, type)` function gets the start position
(`type="start"`), end position (`type="end"`), or the length
(`type="length"`) of the the $i^{th}$ occurrence of pattern `p` in every
string of character vector `x`. Like in `s_extract_ith()`, `i` can be
positive to count the occurrences from left to right, or negative to
count the occurrences from right to left.

The `s_repl_ith(x, i, p, rp)` function replaces the $i^{th}$ occurrence
of pattern `p` with string `rp` in every string of character vector `x`.
As in the previous 2 functions, `i` can be positive to count the
occurrences from left to right, or negative to count the occurrences
from right to left.

Here are some examples:

``` r
x <- c("yeay nay or nothing to say", "Goodmorning, goodevening and goodnight",
       paste0(letters[1:13], collapse=""))
print(x)
#> [1] "yeay nay or nothing to say"            
#> [2] "Goodmorning, goodevening and goodnight"
#> [3] "abcdefghijklm"
p <- "a|e|i|o|u" # pattern for all vowels.

s_extract_ith(x, 1, p) # extract the first vowel
#> [1] "e" "o" "a"
s_extract_ith(x, -1, p) # extract the last vowel
#> [1] "a" "i" "i"
s_extract_ith(x, 2, p) # extract the second vowel
#> [1] "a" "o" "e"
s_extract_ith(x, -2, p) # extract the second-last vowel
#> [1] "o" "o" "e"

rp <- "?"
s_repl_ith(x, 1, p, rp) # replace the first vowel with question mark
#> [1] "y?ay nay or nothing to say"            
#> [2] "G?odmorning, goodevening and goodnight"
#> [3] "?bcdefghijklm"
s_repl_ith(x, -1, p, rp) # replace the last vowel with question mark
#> [1] "yeay nay or nothing to s?y"            
#> [2] "Goodmorning, goodevening and goodn?ght"
#> [3] "abcdefgh?jklm"
s_repl_ith(x, 2, p, rp) # replace the second vowel with question mark
#> [1] "ye?y nay or nothing to say"            
#> [2] "Go?dmorning, goodevening and goodnight"
#> [3] "abcd?fghijklm"
s_repl_ith(x, -2, p, rp) # replace the second-last vowel with question mark
#> [1] "yeay nay or nothing t? say"            
#> [2] "Goodmorning, goodevening and go?dnight"
#> [3] "abcd?fghijklm"


x <- c(paste0(letters[1:13], collapse=""), paste0(letters[14:26], collapse=""))
print(x)
#> [1] "abcdefghijklm" "nopqrstuvwxyz"
# multi-character pattern:
p <- s_pattern_b("AB", fixed=FALSE, ignore.case=TRUE, perl=FALSE)
s_locate_ith(x, -1, p, "start")
#> [1]  1 NA
s_locate_ith(x, -1, p, "end")
#> [1]  2 NA
s_locate_ith(x, -1, p, "length")
#> [1]  2 NA
```

Another fun string operator is `x %ss%  s`. This essentially splits
character vector `x` into a vector containing only individual
characters; then this vector is subsetted by the number given in `s`.
Honestly, I do not think one would need this often, but it can be handy
sometimes. For example:

``` r
x <- "Tom Marvolo Riddle"
toupper(x) %ss% c(14, 4, 6:5, 12, 10:11, 13, 15, 8:9, 17:16, 18, 3:2, 7, 1) |>
  paste0(collapse = "")
#> [1] "I AM LORDVOLDEMORT"
```

 

## String arithmetic

Certainly, the `tidyoperators` package is not the first R package to
introduce string arithmetic. But I do hope these operators have more
consistent naming and functionality.

`x %s+% y` is the same as `paste0(x, y)`:

``` r
x <- "Hello"
y <- " world"
x %s+% y
#> [1] "Hello world"
```

`x %s-% p` removes pattern `p` from each string in character vector `x`:

``` r
x <- c("Hello world", "Goodbye world")
p <- " world"
x %s-% p
#> [1] "Hello"   "Goodbye"
```

`x %s*% n` repeats each string in character vector `x` for `n` times:

``` r
x <- c("Ha", "Ho", "Hi", "Hu", "He", "Ha")
n <- 2:7
x %s*% n
#> [1] "HaHa"           "HoHoHo"         "HiHiHiHi"       "HuHuHuHuHu"    
#> [5] "HeHeHeHeHeHe"   "HaHaHaHaHaHaHa"
```

Finally if, “ha” times 10 equals “HaHaHaHaHaHaHaHaHaHa”, then it follows
intuitively than “HaHaHaHaHaHaHaHaHaHa” divided by “Ha” equals 10. Thus,
string division, `x %s/% p` counts how often pattern `p` occurs in
string or vector `x`:

``` r
x <- c("Ha", "Ho", "Hi", "Hu", "He", "Ha") %s*% 10
p <- "Ha"
x %s/% p
#> [1] 10  0  0  0  0 10
```

It is important to note that the right-side arguments `p`, `y`, and `n`
can be a single value, or a vector of the same length as `x`.

 

## In-place modifying string arithmetic and subsetting

With the exception of `%ss%`, all infix operators (notice: operators,
not functions) have their in-place modifying equivalent:

- `x %s+ <-% y` is the same as `x <- x %s+% y`
- `x %s- <-% p` is the same as `x <- x %s-% p`
- `x %s* <-% n` is the same as `x <- x %s*% n`
- `x %s/ <-% p` is the same as `x <- x %s/% p`
- `x %sget <-% ss` is the same as `x <- x %sget% ss`
- `x %strim <-% ss` is the same as `x <- x %strim% ss`

The `s_extract_ith()` and `s_repl_ith()` functions obviously do not
require an in-place modifier version, as they can easily be used in
combination with `magrittr`’s in-place pipe modifier ( `%<>%` ), just
like any other function.

 

# More advanced string operations

## Pattern attributes in strings

In all string arithmetic and sub-setting operators/functions given
above, the pattern `p` was taken as a case-sensitive, non-fixed, regular
expression. But I can imagine one wants to change that. For example, one
may explicitly want to ignore the distinction between capital and lower
characters. Or one may want to use fixed expressions. Or Perl
expressions. Or a combination of these.

Well, fret not, for the `tidyoperators` package can also help in those
cases. To use more refined pattern definition, simply replace the
argument/right-hand-side expression `p` in all previous
functions/operators with `s_pattern_b(p, ...)`.

The `s_pattern_b(p, fixed, ignore.case, perl, useBytes)` function allows
the user to specify how exactly the pattern should be interpreted by the
other functions/operators in the `tidyoperators` package. The `fixed`,
`ignore.case` and `perl` arguments have exactly the same meaning as they
do in the base R grep, gregexpr, etc. functions.

On a technical level, what the `s_pattern_b()` function actually does is
it simply assigns attributes to `p`, and all `tidyoperators` functions
that use `p` can read those attributes and adjust their functionality
accordingly. That’s all. As implied earlier, the default arguments for
`s_pattern_b` are the same as in base R:
`fixed=FALSE, ignore.case=FALSE, perl=FALSE, useBytes=FALSE`.

Now lets give some examples on how to use `s_pattern_b()`.

First an example where the distinction between capital characters and
lower characters is ignored:

``` r

x <- c(tolower(letters)[1:13] |> paste0(collapse=""),
       toupper(letters)[14:26] |> paste0(collapse=""))
print(x)
#> [1] "abcdefghijklm" "NOPQRSTUVWXYZ"
p <- s_pattern_b("a|E|i|O|u", fixed=FALSE, ignore.case=TRUE, perl=FALSE)
s_extract_ith(x, -1, p) # extracts the last vowel in each element of x.
#> [1] "i" "U"
s_repl_ith(x, -1, p, "?") # replace last vowel in each element of x with a question mark ("?").
#> [1] "abcdefgh?jklm" "NOPQRST?VWXYZ"
x %s-% p # remove all vowels in each string of vector x.
#> [1] "bcdfghjklm"  "NPQRSTVWXYZ"
x %s/% p # count how often vowels appear in each string of vector x.
#> [1] 3 2
```

Second, an example using Perl expression

``` r
x <- "line1 \n line2"
print(x)
#> [1] "line1 \n line2"
p <- s_pattern_b("\\v+", perl=TRUE) # Perl expression; only works with perl=TRUE
s_repl_ith(x, 1, p, " - ") # replace vertical line break with a minus line.
#> [1] "line1  -  line2"
```

But wait, there’s more.

As stated in the introduction section of this Read me, this R package
can also understand the options given by the `stringi` R package. By
default, `tidyoperators` uses base R. To use `stringi` functionality for
its pattern matching, use `s_pattern_stri` instead of `s_pattern_b`().
The next section will give lots of examples with stringi.

 

## Using stringi with tidyoperators

The `%s-%` and `%s/%` operators, and the `s_extract_ith()` and
`s_repl_ith()` functions perform pattern matching for subtracting,
counting, extracting, and replacing patterns, respectively.

By default, `tidyoperators` uses base R for its pattern matching
functions. But `tidyoperators` also supports `stringi` pattern matching,
provided `stringi` version 1.7.12+ is installed. To use `stringi`
functionality for its pattern matching, use `s_pattern_stri()` instead
of `s_pattern_b()`.

The `s_pattern_stri()` uses the exact same argument convention as
`stringi`. So you don’t run `s_pattern_stri(p=p, fixed=TRUE)`; instead
you run `s_pattern_stri(fixed=p)`. Some more examples:

- `s_pattern_stri(regex=p, case_insensitive=FALSE, ...)`
- `s_pattern_stri(fixed=p, ...)`
- `s_pattern_stri(coll=p, ...)`
- `s_pattern_stri(boundary=p, ...)`
- `s_pattern_stri(charclass=p, ...)`

Why would one use `stringi` patterns instead of base R patterns? Well, 3
main reasons I can think of:

- First, if you used `stringi` - or a `stringi`-based package like
  `tidyverse`’s `stringr` - in your script, you may wish to keep your
  string-related code consistent to avoid confusing anyone (or even
  confusing yourself).
- Second, some of `stringi`’s functions are faster than base R, and
  using `stringi` patterns in `tidyoperators` doesn’t simply change the
  expression, it actually uses `stringi` functions behind the scenes.
  **This may improve the speed of your code**.
- Thirdly, `stringi` has some pattern features not present in base R,
  that `tidyoperators` can use.

This section will give some examples of using `stringi` with
`tidyoperators`. `stringi` does **not need to be loaded, only
installed**. For the sake of this read-me, however, let’s explicitly
load `stringi`:

``` r
require(tidyoperators)
require(stringi)
#> Loading required package: stringi
#> Warning: package 'stringi' was built under R version 4.2.2
#> 
#> Attaching package: 'stringi'
#> The following objects are masked from 'package:tidyoperators':
#> 
#>     %s*%, %s+%
```

You’ll notice that `stringi` will overwrite `tidyoperator`’s `%s*%` and
`%s+%` operators; that’s completely fine, the operators do the same
thing. Use whichever one you prefer.

Now then, some examples using `stringi`’s `regex` expression:

``` r
# VOWELS SUBSETTING ====

x <- c("yeay nay or nothing to say", "Goodmorning, goodevening and goodnight",
       paste0(letters[1:13], collapse=""))
print(x)
#> [1] "yeay nay or nothing to say"            
#> [2] "Goodmorning, goodevening and goodnight"
#> [3] "abcdefghijklm"
p <- s_pattern_stri(regex = "a|e|i|o|u", case_insensitive=TRUE)

s_extract_ith(x, 1, p) # extract the first vowel
#> [1] "e" "o" "a"
s_extract_ith(x, -1, p) # extract the last vowel
#> [1] "a" "i" "i"
s_extract_ith(x, 2, p) # extract the second vowel
#> [1] "a" "o" "e"
s_extract_ith(x, -2, p) # extract the second-last vowel
#> [1] "o" "o" "e"

s_locate_ith(x, 2, p, "start")
#> [1] 3 3 5
s_locate_ith(x, 2, p, "end")
#> [1] 3 3 5
s_locate_ith(x, 2, p, "length")
#> [1] 1 1 1

rp <- "?"
s_repl_ith(x, 1, p, rp) # replace the first vowel with question mark
#> [1] "y?ay nay or nothing to say"            
#> [2] "G?odmorning, goodevening and goodnight"
#> [3] "?bcdefghijklm"
s_repl_ith(x, -1, p, rp) # replace the last vowel with question mark
#> [1] "yeay nay or nothing to s?y"            
#> [2] "Goodmorning, goodevening and goodn?ght"
#> [3] "abcdefgh?jklm"
s_repl_ith(x, 2, p, rp) # replace the second vowel with question mark
#> [1] "ye?y nay or nothing to say"            
#> [2] "Go?dmorning, goodevening and goodnight"
#> [3] "abcd?fghijklm"
s_repl_ith(x, -2, p, rp) # replace the second-last vowel with question mark
#> [1] "yeay nay or nothing t? say"            
#> [2] "Goodmorning, goodevening and go?dnight"
#> [3] "abcd?fghijklm"


# MULTI-CHAR SUBSETTING ====

x <- c(paste0(letters[1:13], collapse=""), paste0(letters[14:26], collapse=""))
print(x)
#> [1] "abcdefghijklm" "nopqrstuvwxyz"
p <- s_pattern_stri(regex="AB", case_insensitive=TRUE)
s_extract_ith(x, -1, p)
#> [1] "ab" NA
s_repl_ith(x, -1, p, "?")
#> [1] "?cdefghijklm"  "nopqrstuvwxyz"
s_locate_ith(x, -1, p, "start")
#> [1]  1 NA
s_locate_ith(x, -1, p, "end")
#> [1]  2 NA
s_locate_ith(x, -1, p, "length")
#> [1]  2 NA


# PATTERN ARITHMETIC ====

x <- c("Hello world", "Goodbye world")
p <- s_pattern_stri(regex=" world")
x %s-% p
#> [1] "Hello"   "Goodbye"


x <- c("Ha", "Ho", "Hi", "Hu", "He", "Ha") %s*% 10
p <- s_pattern_stri(regex="Ha")
x %s/% p
#> [1] 10  0  0  0  0 10
```

And then some fixed expressions:

``` r
x <- c("yeay yeay yeay yeay", "nay nay nay nay")
p <- s_pattern_stri(fixed = "ye")

rp <- "?"
s_repl_ith(x, 1, p, rp) # replace the first vowel with question mark
#> [1] "?ay yeay yeay yeay" "nay nay nay nay"
s_repl_ith(x, -1, p, rp) # replace the last vowel with question mark
#> [1] "yeay yeay yeay ?ay" "nay nay nay nay"
s_repl_ith(x, 2, p, rp) # replace the second vowel with question mark
#> [1] "yeay ?ay yeay yeay" "nay nay nay nay"
s_repl_ith(x, -2, p, rp) # replace the second-last vowel with question mark
#> [1] "yeay yeay ?ay yeay" "nay nay nay nay"

s_locate_ith(x, -1, p, "start")
#> [1] 16 NA
s_locate_ith(x, -1, p, "end")
#> [1] 17 NA
s_locate_ith(x, -1, p, "length")
#> [1]  2 NA

x <- c("Hello world", "Goodbye world")
p <- s_pattern_stri(fixed=" world")
x %s-% p
#> [1] "Hello"   "Goodbye"


x <- c("Ha", "Ho", "Hi", "Hu", "He", "Ha") %s*% 10
p <- s_pattern_stri(coll="Ha")
x %s/% p
#> [1] 10  0  0  0  0 10
```

And so on. I’m sure you get the idea.

 

## More string flexibility: s_strapply

The string arithmetic and sub-setting operators and functions given so
far - in combination with the string function already available in base
R and R packages such as `stringi` - can do a lot, but it’s not always
flexible enough. To add extra flexibility, there is also the
`s_strapply(x, fun, w=F, clp=NULL, ...)` function. This function applies
the following steps to every element (every string) of character vector
x:

1)  the string is split into a vector of single characters (`w=F`), or a
    vector of space-delimited words (`w=T`).
2)  the function `fun()` is applied to the vector from step 1.
3)  the result from step 2 is pasted together to form a single string
    element again, using `paste0(..., collapse=clp)`. By default,
    `clp=""` if `w=F`, and `clp=" "` if `w=T`.

In other words, this function turns every string in character vector `x`
into its own little vector, applies a function to this vector, and
pastes the returning vector together into a single string again.

This operator can be used in a very wide variety of ways.

One obvious use of this function is for re-arranging the characters or
words in every string in character vector `x`, or find the occurrence of
the character on the alphabet:

``` r
x <- c("Hello World", "Goodbye World")
s_strapply(x, sort) # sort letters
#> [1] " deHllloorW"   " bddeGlooorWy"
s_strapply(x, sample) # randomly shuffle letters
#> [1] "rlWHeoldlo "   "GbloyWodr doe"
s_strapply(x, rev, w=T) # reverse words
#> [1] "World Hello"   "World Goodbye"
# find occurrence of characters on alphabet:
s_strapply(x, fun=\(x)match(tolower(x),letters), clp="; ")
#> [1] "8; 5; 12; 12; 15; NA; 23; 15; 18; 12; 4"      
#> [2] "7; 15; 15; 4; 2; 25; 5; NA; 23; 15; 18; 12; 4"
```

I think this is reasonably easy and tidy.

Lets try something else: capitalize characters but ONLY at certain
indices:

``` r
x <- c("Hello World", "Goodbye World")
# capitalize odd indices:
s_strapply(x, fun=\(x){
  replace(x, seq(1, length(x), 2), toupper(x)[seq(1, length(x), 2)])
})
#> [1] "HeLlO WoRlD"   "GoOdByE WoRlD"

# capitalize random letters:
s_strapply(x, fun=\(x){
  ind <- sample(1:length(x), size=floor(length(x)/2))
  replace(x, ind, toupper(x[ind]))
})
#> [1] "HEllO WoRlD"   "GoODbYe WorLd"

# capitalize only first word:
s_strapply(x, fun=\(x){replace(x, 1, toupper(x[1]))}, w=T)
#> [1] "HELLO World"   "GOODBYE World"
```

Lets try to take the second-last vowel of every word of every string in
some character vector `x`:

``` r
x <- c("Outrageous, egregious, preposterous!", "Pleasant evening everyone")
print(x)
#> [1] "Outrageous, egregious, preposterous!"
#> [2] "Pleasant evening everyone"
p <- s_pattern_b("a|e|i|o|u", ignore.case = TRUE)
s_strapply(x, w=T, fun=\(x)s_extract_ith(x, -2, p))
#> [1] "o o o" "a e o"
```

 

## Parallel computing / multi-threading in string functions

The `s_repl_ith()` and `s_extract_ith()` functions internally use
`mapply()`. I can imagine that sometimes some character vector `x` is so
large that `mapply()` is too slow or inefficient, and one might want to
use parallel computing. Because I wanted `tidyoperators` to have zero
dependencies, I do not provide parallel computing out of the box.
However, some `tidyoperators` functions do allow the internally
`mapply()` function to be replaced with a user provided function.

For example, suppose one wants to speed up `s_extract_ith()` using
parallel computing. One can use the `future.apply::future_mapply()` as a
replacer for the mapply function, like so:

``` r

x <- rep(c("Hello World", "Goodbye World"), 2e5)
p <- s_pattern_b("a|e|i|o|u", ignore.case = TRUE)

s_extract_ith(x, -1, p) # regular way

require(future.apply)
plan(multisession)
s_extract_ith(x, -1, p, custom_mapply = future_mapply) # multi-threaded way.
```

Now you have a multithreaded version of s_extract_ith; that wasn’t so
difficult, right? On my computer, the multi-threaded command was about 3
times faster.

The `s_strapply` function uses `sapply` internally. As above, the user
can multi-thread this function by replacing sapply:

``` r
x <- rep(c("Hello World", "Goodbye World"), 2e5)

s_strapply(x, sort) # regular way

require(future.apply)
plan(multisession)
s_strapply(x, sort, custom_sapply = future_sapply) # multi-threaded way
```

Now you have a multi-threaded version of `s_strapply`.

If you combine `s_strapply` and `s_extract_ith` / `s_repl_ith`, I advice
the user to only make `s_strapply` multi-threaded; making multiple
layers of parallel computing seems like asking for problems.

An example of combining `s_strapply` and `s_extract_ith` is:

``` r
x <- rep(
  c("Outrageous, egregious, preposterous!", "Pleasant evening everyone"),
  2e5
)
# s_strapply combined with s_extract_ith:
p <- s_pattern_b("a|e|i|o|u", ignore.case = TRUE) 
s_strapply(x, w=T, fun=\(x)s_extract_ith(x, -2, p), custom_sapply = future_sapply)
```

It should be noted that the speed is also very much dependent on the
function used for the `fun` argument. So using `stringi` pattern might
make this even faster. For example:

``` r
x <- rep(
  c("Outrageous, egregious, preposterous!", "Pleasant evening everyone"),
  2e5
)
# stringi regex + multi-threaded:
p <- s_pattern_stri(regex="a|e|i|o|u", case_insensitive = TRUE)
s_strapply(x, w=T, fun=\(x)s_extract_ith(x, -2, p), custom_sapply = future_sapply)
```

Notice that only `s_strapply()` is multi-threaded, and not
`s_extract_ith`. Multi-threading both would actually be slower, and may
even create other problems.

 

# Recommended R packages

I highly recommend using this R package alongside the 2 major
operator-related R-packages, namely `magrittr` and `zeallot`. Since
about half the operators, and almost all functions concern string
manipulation, I would also recommend the R-packaes `stringi` (or
`stringr`, though that is kind-of “stringi-light”), and `stringfish`.

 

# Conclusion

I hope this R package will make your life a little bit tidyr.
