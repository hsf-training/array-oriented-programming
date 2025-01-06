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

Note that the nesting is indicated by indenting, rather than curly brackets (`{` `}`), an `end` token, or any other syntax. The rules are:

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

Python's `is` keyword tells you whether two varible names refer to the same object (as opposed to just being equal to the same value).

```{code-cell} ipython3
mag3d is euclidean
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

+++

One of the reasons to prefer a popular language like Python is that much of the functionality you need already exists as free software. You can just install the packages you need, import them into your session, and then use them.

It's important to understand the distinction between

* installing: downloading the packages onto the computer you're using (with pip, conda, uv, pixi, ...), so that they become available as modules
* importing: assigning variable names to the functions in those modules

Python comes with some modules built into its [standard library](https://docs.python.org/3/library/index.html). If you have Python, these modules are already installed.

One of these pre-installed modules is called `math`.

```{code-cell} ipython3
import math
```

The above statement has assigned the module as a new variable name, `math`.

```{code-cell} ipython3
math
```

You can access functions in the module with a dot (`.`).

```{code-cell} ipython3
math.sqrt
```

We can now use `math.sqrt` instead of `**(1/2)`.

```{code-cell} ipython3
math.sqrt(E**2 - px**2 - py**2 - pz**2)
```

[NumPy](https://numpy.org/) is a package that needs to be installed before you can use it. (In the [intro](0-intro.md), I described a few different ways to install it.)

```{code-cell} ipython3
import numpy
```

```{code-cell} ipython3
numpy
```

The NumPy package also defines a function called `sqrt`. The dot-syntax ensures that they can be distinguished.

```{code-cell} ipython3
numpy.sqrt(E**2 - px**2 - py**2 - pz**2)
```

```{code-cell} ipython3
math.sqrt is numpy.sqrt
```

The variable name that you use for the module doesn't have to be the same as its package name. In fact, some packages have conventional "short names" that they encourage you to use.

```{code-cell} ipython3
import numpy as np
```

```{code-cell} ipython3
numpy is np
```

```{code-cell} ipython3
del numpy
```

```{code-cell} ipython3
np.sqrt(E**2 - px**2 - py**2 - pz**2)
```

Sometimes, you'll want to extract only one or a few objects from a module, but not assign the module itself to a variable name.

```{code-cell} ipython3
from hepunits import GeV
from particle import Particle
```

```{code-cell} ipython3
muon = Particle.from_name("mu+")
muon
```

```{code-cell} ipython3
muon.mass / GeV
```

## Data types

+++

Python has data types, but unlike "compile-first" languages, it only verifies whether you're using the types correctly right before a computation, rather than in a compilation phase.

```{code-cell} ipython3
1 + "2"
```

You can determine an object's type with the `type` function.

```{code-cell} ipython3
type(1)
```

```{code-cell} ipython3
type("2")
```

```{code-cell} ipython3
type(True)
```

```{code-cell} ipython3
type(False)
```

Also unlike "compile-first" languages, these types are objects that can be assigned to variables, like any other object.

```{code-cell} ipython3
t1 = type(1)
t2 = type("2")
```

```{code-cell} ipython3
t1
```

```{code-cell} ipython3
t2
```

Most type objects are also functions that create or convert data to that type.

```{code-cell} ipython3
int("2")
```

```{code-cell} ipython3
t1("2")
```

**Mini-quiz:** Before you run the following, what will it do?

Hint: break it down in parts and test each part.

```python
type(type(1)("2"))
```

+++

## Type hierarchies

+++

Types are like sets: the number `1` is a member of the set `int`, and the string `"2"` is a member of the set `str`. Sets can have subsets, such as "whole numbers are a subset of real numbers."

Let's see this using NumPy's numeric types. `np.int32` is a type (and constructor of) integers using 32 [bits](https://en.wikipedia.org/wiki/Bit) of memory.

```{code-cell} ipython3
np.int32(1)
```

```{code-cell} ipython3
np_one = np.int32(1)
```

```{code-cell} ipython3
type(np_one)
```

```{code-cell} ipython3
type(np_one) is np.int32
```

This 32-bit integer type is a subtype of `np.integer`, which we can see by asking if 32-bit integer instances are instances of `np.integer`,

```{code-cell} ipython3
isinstance(np_one, np.integer)
```

or by asking if the `np.int32` type is a subtype ("subclass") of `np.integer`.

```{code-cell} ipython3
issubclass(np.int32, np.integer)
```

NumPy has a large hierarchy of subtypes within supertypes, which is independent of the Python number hierarchy.

![](img/numpy-type-hierarchy.svg){. width="100%"}

Notice that NumPy and Python have different opinions about whether booleans are integers.

+++

**Mini-quiz:** Write two expressions to show that booleans in Python are integers and booleans in NumPy are not.

+++

## Collection types

+++

Collections are Python objects that contain objects. The two most basic collection types in Python are `list` and `dict`.

```{code-cell} ipython3
some_list = [0.0, 1.1, 2.2, 3.3, 4.4, 5.5, 6.6, 7.7, 8.8, 9.9]
some_list
```

```{code-cell} ipython3
type(some_list)
```

```{code-cell} ipython3
len(some_list)
```

```{code-cell} ipython3
some_dict = {"one": 1.1, "two": 2.2, "three": 3.3}
some_dict
```

```{code-cell} ipython3
type(some_dict)
```

```{code-cell} ipython3
len(some_dict)
```

You can get an object out of a `list` or `dict` using square brackets (`[` `]`).

```{code-cell} ipython3
some_list[3]
```

```{code-cell} ipython3
some_dict["two"]
```

You can also change the data in a collection if the square brackets are on the left of an assignment (`=`).

```{code-cell} ipython3
some_list[3] = 33333
```

```{code-cell} ipython3
some_list
```

```{code-cell} ipython3
some_dict["two"] = 22222
```

```{code-cell} ipython3
some_dict
```

And you can extend them beyond their original length, as well as mix different data types in the same collection.

```{code-cell} ipython3
some_list.append("mixed types")
```

```{code-cell} ipython3
some_list
```

```{code-cell} ipython3
some_dict[123] = "mixed types"
```

```{code-cell} ipython3
some_dict
```

Ranges within a `list` can be "sliced" with a colon (`:`).

```{code-cell} ipython3
some_list[2:8]
```

**Mini-quiz:** Before you run it, what will the following do?

```python
some_list[2:8][3]
```

+++

(We'll see more about slices in the next lesson on arrays.)

+++

## A little data analysis

+++

Now let's use Python collections to perform some steps in a data analysis. Suppose you have this (small!) collection of observed particles,

```{code-cell} ipython3
particles = [
    {"type": "electron", "E": 171.848714, "px": 38.4242935, "py": -28.779644, "pz": 165.006927, "charge": 1,},
    {"type": "electron", "E": 138.501266, "px": -34.431419, "py": 24.6730384, "pz": 131.864776, "charge": -1,},
    {"type": "muon", "E": 68.1289790, "px": -17.945541, "py": 13.1652603, "pz": 64.3908386, "charge": 1,},
    {"type": "muon", "E": 18.8320473, "px": -8.1843795, "py": -7.6400470, "pz": 15.1420097, "charge": -1,},
]
```

and you want to infer the energy and momentum of the Higgs and Z bosons that decayed into electrons and muons through this mechanism:

![](img/higgs-to-four-leptons-diagram.png){. width="60%"}

A particle that decays into `particle1` and `particle2` has the sum of energy, momentum, and charge of these observed particles, so let's make a helper function for that.

```{code-cell} ipython3
def particle_decay(name, particle1, particle2):
    return {
        "type": name,
        "E": particle1["E"] + particle2["E"],
        "px": particle1["px"] + particle2["px"],
        "py": particle1["py"] + particle2["py"],
        "pz": particle1["pz"] + particle2["pz"],
        "charge": particle1["charge"] + particle2["charge"],
    }
```

Let's call the Z that decayed into electrons `z1`,

```{code-cell} ipython3
z1 = particle_decay("Z boson", particles[0], particles[1])
z1
```

and the Z that decayed into muons `z2`,

```{code-cell} ipython3
z2 = particle_decay("Z boson", particles[2], particles[3])
z2
```

The Higgs boson decayed into the two Z bosons, so we can compute its energy and momentum using `z1` and `z2` as input.

```{code-cell} ipython3
higgs = particle_decay("Higgs boson", z1, z2)
higgs
```

**Mini-quiz:** Define a `particle_mass` function using an equation given earlier in this lesson and use it to compute the masses of `z1`, `z2`, and `higgs`.

```{code-cell} ipython3
def particle_mass(particle):
    ...
```

## `for` loops and `if` branches

+++

Can you believe we got this far without `for` and `if`?

These are the fundamental building blocks of _imperative_ programming, but our focus will be on _array-oriented_ programming.

`for` is used to repeat Python statements. Python runs one statement at a time, and the statements in an indented `for` block are run zero or more times.

```{code-cell} ipython3
for particle in particles:
    print(particle["type"], particle["charge"])
```

It doesn't even look ahead to see if there's trouble coming on the next line.

```{code-cell} ipython3
for particle in particles:
    print(particle["type"])
    print(particle["charge"])
    print(particle["something it does not have"])
```

`if` tells it whether it should enter an indented block or not, depending on whether an expression is `True` or `False`.

```{code-cell} ipython3
for particle in particles:
    if particle["type"] == "electron":
        print(particle)
```

It can switch between two indented blocks if an `else` clause is given.

```{code-cell} ipython3
for particle in particles:
    if particle["type"] == "electron":
        print(particle)
    else:
        print("not an electron")
```

`if` (and `for`) statements can be nested.

```{code-cell} ipython3
for particle in particles:
    if particle["type"] == "electron":
        if particle["charge"] > 0:
            print("e+")
        else:
            print("e-")
    else:
        if particle["charge"] > 0:
            print("mu+")
        else:
            print("mu-")
```

And `elif` works as a contraction of `else if` with less indenting.

```{code-cell} ipython3
for particle in particles:
    if particle["type"] == "electron" and particle["charge"] > 0:
        print("e+")
    elif particle["type"] == "electron" and particle["charge"] < 0:
        print("e-")
    elif particle["type"] == "muon" and particle["charge"] > 0:
        print("mu+")
    elif particle["type"] == "muon" and particle["charge"] < 0:
        print("mu-")
```

## From datum (singular) to data (plural)

```{code-cell} ipython3
import json
```

```{code-cell} ipython3
dataset = json.load(open("data/SMHiggsToZZTo4L.json"))
```
