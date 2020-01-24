# Code quality

Your program may be functional, but that is not sufficient. If you cannot convince users that your program does what you say it does, it is as good as non-functional. [Having a transparent and robust workflow](project_setup.md) will help instill that trust, but what may be more important is the quality and readability of the code itself.

Of course, you yourself will benefit the most from well-written code as you develop the project. After all, the most likely person who has to read your code is you...

Some tenets of quality are that code should be:

- Readable,
- Reusable, and
- Robust.


## Readable code

Writing readable code is a real skill. Here are just a few hints to help you reflect about your current coding practices and see whether you should adapt your coding habits to increase readability.

### Use line breaks and whitespace strategically

Sure, code may be written for a computer, but humans have to read it too. The fact that your code still runs if you never enter a line break does not mean that you should do so. There are choices you make on how you style your code lay-out that will not affect the actual functionality, but they will affect how your code looks, and how easy it is to understand and maintain. 

_Compare:_
```r
this <- function(arg1,arg2) res<-arg1*arg2;return(res)
hurts <- mean(c(this(3,4),this(3,1),this(9,9))); print(hurts)
```
_with_
```r
this <- function(arg1,arg2){
  res <- arg1 * arg2
  return(res)
}

hurts <- mean(
  c(
    this(3,4),
    this(3,1),
    this(9,9)
    )
  )
print(hurts)
```

### Use descriptive names for functions and variables

While it might be convenient to use variable names such as `test`, `temp`, `[a,b,c,d]` it is difficult to understand later what `temp` contains or what exactly `a` does at a certain point in your script. Therefore, it is better to use expressive variable names with reasonable limits regarding length and information.

Why not `sortedResults` instead of `res`, `proteinData` instead of `data`. (Of course, the kind of protein data can be derived from the documentation of your script, data sources used etc. So, do not try to work with extremely detailed variable names, which reduce the readability instead of improving it.)

The same can be said for function names: make them descriptive, but not too specific. For functions, starting the name with a verb will (1) clarify that it is a function, and (2) indicate what it _does_. For example: `exportDataAsCSV`, `createEmptyDataFrame`, ...

_Compare:_

```python
for i in my_shopping_basket:
  if(test(i)) > 10:
    purch(i)
  else:
    disc(i)
```
_with_
```python
for item in basket:
  if(testNecessity(item)) > 10:
    purchase(item)
  else:
    discard(item)
```

**Note**: the code style is determined by the language you are using and its conventions. Code editors and IDEs integrate style checking and might suggest more suitable names of variables and functions if your naming convention differs from language-specific standards.

### Adhere to a coding style

There is right and wrong code, and there is style. 

Adhering to a coding style will drive your choices and improve your consistency. Having consistent code will make it easier to understand and maintain. Moreover, these conventions are often the result of experience on what makes good code. Sticking to them means you will not have to reinvent the wheel.

Often, these style choices are described in a style manual. For Python, for example, 
there is [Pep-8](https://www.python.org/dev/peps/pep-0008/). 
For R, an example would be the [Tidyverse style guide](https://style.tidyverse.org).

![codequality](https://imgs.xkcd.com/comics/code_quality.png)




## Robust code

Writing robust code is not only a matter of testing it but also about properly managing errors, use of variables
and so on. 

* _dealing with possible errors at running time_ is done with checking the content of variables before making use of them
Is the variable empty, does it has the expected range of values or not. 
* _use of variables_ avoid (certainly with languages such as R and Python that allows that) to reuse variables if their
meaning and type change through your script. For instance, a variable that was initialized with a numeric value should
not be reused to store text later on. It is confusing and might create unforeseen troubles at running time.
* _testing_ running code chunks with dummy data, putting your code under pressure with data that might be entered by
users of your package (think about someone calling a function from your R package and sending wrong values). This is 
the purpose of unit testing.

### Functional comments

Comments can be helpful to explain some decision (e.g., if...else) present in your code, what you try to do if it is not
explicit from the code itself and the purpose/behavior of functions.

What comments should not be used for is
* disabling some part of your code from running (at least for code you publish)
* explain in plain old language what is understandable from reading your code
* warn users of your package for values or use of your code which are not handle by your code, there is error management
for that.

#### Documentation generation
Comments are useful when you are reading the source code. But reading the source code is not mandatory to use a 
package, therefore users might miss important information if you rely on comments exclusively, even if you feel
you did a good job at documenting everything.

Documentation is generation is part of the quality of the code as it relies upon parts of your code where you use
specific syntax and keywords that can be interpreted by documentation generators. The reason to use them is that you
produce documentation from one reliable source (your code) and can adapt the "manual" of your software right from the 
code !



## Documentation

#### Notebooks

Notebooks are a good way to demonstrate your package in action. 

#### Documentation generators

For R with [Roxygen](https://roxygen2.r-lib.org/)

For Python with [Sphinx](http://www.sphinx-doc.org/en/master/usage/quickstart.html)

#### Packaging and distributing

- Building packages
    - necessary files
- Distributing packages


# Unit tests (packages)

Setup unit test with the `testthat` package:

```r
usethis::use_testthat()
```

This creates a new dir `tests/testthat` in your R package. To run the tests we use:

```r
devtools::test()
```


# Roxygen (packages only)

