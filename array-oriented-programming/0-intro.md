# Array oriented programming for particle physicists

This book is an introduction to analyzing particle physics data in Python using arrays.

Before you start reading, open the "notebooks" folder in Jupyter Lab, which gives you an environment in which you can run the examples and sets everything up to do the practice problems.

## In GitHub Codespaces

One way to do that is to launch a Codespace from the [hsf-training/array-oriented-programming](https://github.com/hsf-training/array-oriented-programming) repository:

```{image} img/github-codespaces.png
:width: 60%
```

The first time your Codespace launches, it will take a few minutes to install all of the software. Once it's done, navigate to the `notebooks` directory in the left side-bar and open `lesson-1-workbook.ipynb`.

```{image} img/github-codespaces-2.png
:width: 60%
```

Once you evaluate a cell in the notebook, Codespaces will ask you if you want to install the Python extension (yes, you do) and will ask which kernel to use: you want the recommended option, `venv/bin/python`.

```{image} img/github-codespaces-3.png
:width: 100%
```

```{image} img/github-codespaces-4.png
:width: 100%
```

```{image} img/github-codespaces-5.png
:width: 100%
```

**Note:** you can't run [Lesson 5: Python on GPUs](5-gpu.ipynb) in Codespaces because it doesn't have a GPU. [Google Colab](https://colab.research.google.com/) has GPUs, but you'd have to install the software yourself.

## On your own computer

You can run everything on your own computer if you have the following software installed (this is an [environment.yml](https://github.com/hsf-training/array-oriented-programming/blob/main/environment.yml) file for [conda](https://scikit-hep.org/user/installing-conda)):

```yaml
channels:
  - conda-forge

dependencies:
  - python=3.14
  - jupyter-book>=2,<3
  - nbconvert
  - jupyterlab
  - numpy
  - pandas
  - matplotlib
  - scikit-learn
  - h5py
  - pyarrow
  - pympler
  - uproot
  - awkward
  - vector
  - hist
  - hepunits
  - particle
  - clang-tools
  - cxx-compiler
  - pybind11
  - numba
  - jax
  - dask
  - distributed
  - dask-awkward>=2026
  - graphviz
  - python-graphviz
```

**Note:** to do [Lesson 5: Python on GPUs](5-gpu.ipynb), you'll also need an Nvidia GPU and the following packages: `cupy` and `numba`. See the lesson for hints on installing CUDA.

## Table of contents

The lessons are listed in the navigation sidebar.
