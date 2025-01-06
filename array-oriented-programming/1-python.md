---
jupytext:
  cell_metadata_filter: -all
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.4
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

# Lesson 1: Python

+++

If you're following this course, you're probably already familiar with the Python programming language—at least a little bit. Nevertheless, I'll start with a review of the language for three reasons:

* to make sure there aren't any gaps in your knowledge,
* so that you can be confident that there isn't something extra you need to know,
* array-oriented programming emphasizes a different subset of the language, as you'll see.

On the last point, this review of Python covers `for` and `if` statements _last_, rather than _first_, and doesn't cover classes. Defining classes is an important part of Python programming, but we won't be using it.

+++

## Why Python?

+++

Python is now the most popular programming language for many applications. For our purposes, what's relevant is that it is the most popular language for data analysis. Below are plots of search volume from [Google Trends](https://trends.google.com/trends/) that show how often people search for "data analysis" and "machine learning" in the same query with "Python", "R", and "Java".

![](img/analytics-by-language.svg){. width="100%"}

"Popularity" means more tools are available, more attention has been drawn to their shortcomings, and you can find more information about how to use them online. It also means that Python skills are transferable skills.

+++

## Python as an interactive calculator

+++

The most important thing about Python, especially for data analysis, is that it is an interactive language. Unlike "compile-first" languages such as C++, Fortran, and Rust, you can partially run a program and type arbitrary commands into a terminal or a Jupyter notebook, while it runs.

The cost of this interactivity is speed. Python has a severe performance penalty compared to "compile-first" languages, so if you're not _using_ this interactivity (for instance, by always running whole scripts at a time), you're missing out on a feature you're paying for!

Any expression typed on the Python prompt or in a Jupyter cell computes the expression and returns the result:

```{code-cell} ipython3
2 + 2
```

You can also define variables,

```{code-cell} ipython3
E = 68.1289790
px = -17.945541
py = 13.1652603
pz = 64.3908386
```

and use them in expressions.

For instance, calculate ${p_x}^2 + {p_y}^2$.

```{code-cell} ipython3
px**2 + py**2
```

Now the transverse momentum $p_T = \sqrt{{p_x}^2 + {p_y}^2 + {p_z}^2}$.

```{code-cell} ipython3
(px**2 + py**2 + pz**2)**(1/2)
```

The examples in this introduction to Python are inspired by particle physics, so we'll be using the momentum-energy relations a lot:

$$p = \sqrt{{p_x}^2 + {p_y}^2 + {p_z}^2}$$

$$m = \sqrt{E^2 - p^2}$$

+++

**Mini-quiz:** Fix the mistake!

```{code-cell} ipython3
m = (E**2 - px**2 + py**2 + pz**2)**(1/2)
m
```

(The result above is wrong.)

+++

## Defining functions

+++

The syntax for defining a function is:

```{code-cell} ipython3
def euclidean(x, y, z):
    return (x**2 + y**2 + z**2)**(1/2)

def minkowski(time, space):
    return (time**2 - space**2)**(1/2)
```

We can call them using arguments identified by position,

```{code-cell} ipython3
euclidean(px, py, pz)
```

or by name,

```{code-cell} ipython3
euclidean(z=pz, y=py, x=px)
```

The arguments of a function call can be the return values of other functions.

```{code-cell} ipython3
minkowski(E, euclidean(px, py, pz))
```

You can also define functions inside of functions.

```{code-cell} ipython3
def mass(E, px, py, pz):
    def euclidean(x, y, z):
        return (x**2 + y**2 + z**2)**(1 / 2)

    def minkowski(time, space):
        return (time**2 - space**2)**(1 / 2)

    return minkowski(E, euclidean(px, py, pz))

mass(E, px, py, pz)
```

Note that the nesting is indicated by indenting, rather than curly brackets `{` `}`, an `end` token, or any other syntax. The rules are:

* the first statement must have no whitespace (no tabs or spaces),
* nesting is opened by increasing the whitespace relative to the previous line,
* nesting is closed by decreasing the whitespace to a previously established level.

"Whitespace" can be tabs or spaces, but since both are invisible, it can be easy to confuse them. Most codebases use a standard of 4 spaces (and never use tabs), which is what we'll do here.

+++

## Functions are objects

+++

The functions that we defined above are objects that can be referred to by variables, just like numbers.

```{code-cell} ipython3
mag3d = euclidean
```

Now `mag3d` can be used in exactly the same ways as `euclidean`, because they're just two names for the same thing.

```{code-cell} ipython3
mag3d(px, py, pz)
```

In fact, we're free to delete (`del`) the original and keep using it with the new name.

```{code-cell} ipython3
del euclidean
```

```{code-cell} ipython3
euclidean(px, py, pz)
```

```{code-cell} ipython3
mag3d(px, py, pz)
```

We can think of the `def` syntax as doing two things:

1. defining the function,
2. assigning it to a variable name.

+++

## Importing functions into Python

```{code-cell} ipython3

```
