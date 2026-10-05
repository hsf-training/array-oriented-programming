---
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

# Lesson 3: Ragged and nested arrays

So far, all the arrays we've dealt with have been rectangular (in $n$ dimensions; "rectilinear").

![](img/8-layer_cube.jpg){width="50%"}

What if we had data like this?

```json
[
  [[1.84, 0.324]],
  [[-1.609, -0.713, 0.005], [0.953, -0.993, 0.011, 0.718]],
  [[0.459, -1.517, 1.545], [0.33, 0.292]],
  [[-0.376, -1.46, -0.206], [0.65, 1.278]],
  [[], [], [1.617]],
  []
]
[
  [[-0.106, 0.611]],
  [[0.118, -1.788, 0.794, 0.658], [-0.105]]
]
[
  [[-0.384], [0.697, -0.856]],
  [[0.778, 0.023, -1.455, -2.289], [-0.67], [1.153, -1.669, 0.305, 1.517, -0.292]]
]
[
  [[0.205, -0.355], [-0.265], [1.042]],
  [[-0.004], [-1.167, -0.054, 0.726, 0.213]],
  [[1.741, -0.199, 0.827]]
]
```

Or like this?

```json
[
  {"fill": "#b1b1b1", "stroke": "none", "points": [{"x": 5.27453, "y": 1.03276},
    {"x": -3.51280, "y": 1.74849}]},
  {"fill": "#b1b1b1", "stroke": "none", "points": [{"x": 8.21630, "y": 4.07844},
    {"x": -0.79157, "y": 3.49478}, {"x": 16.38932, "y": 5.29399},
    {"x": 10.38641, "y": 0.10832}, {"x": -2.07070, "y": 14.07140},
    {"x": 9.57021, "y": -0.94823}, {"x": 1.97332, "y": 3.62380},
    {"x": 5.66760, "y": 11.38001}, {"x": 0.25497, "y": 3.39276},
    {"x": 3.86585, "y": 6.22051}, {"x": -0.67393, "y": 2.20572}]},
  {"fill": "#d0d0ff", "stroke": "none", "points": [{"x": 3.59528, "y": 7.37191},
    {"x": 0.59192, "y": 2.91503}, {"x": 4.02932, "y": -1.13601},
    {"x": -1.01593, "y": 1.95894}, {"x": 1.03666, "y": 0.05251}]},
  {"fill": "#d0d0ff", "stroke": "none", "points": [{"x": -8.78510, "y": -0.00497},
    {"x": -15.22688, "y": 3.90244}, {"x": 5.74593, "y": 4.12718}]},
  {"fill": "none", "stroke": "#000000", "points": [{"x": 4.40625, "y": -6.953125},
    {"x": 4.34375, "y": -7.09375}, {"x": 4.3125, "y": -7.140625},
    {"x": 4.140625, "y": -7.140625}]},
  {"fill": "none", "stroke": "#808080", "points": [{"x": 0.46875, "y": -0.09375},
    {"x": 0.46875, "y": -0.078125}, {"x": 0.46875, "y": 0.53125}]}
]
```

Or like this?

```json
[
  {"movie": "Evil Dead", "year": 1981, "actors":
    ["Bruce Campbell", "Ellen Sandweiss", "Richard DeManincor", "Betsy Baker"]
  },
  {"movie": "Darkman", "year": 1900, "actors":
    ["Liam Neeson", "Frances McDormand", "Larry Drake", "Bruce Campbell"]
  },
  {"movie": "Army of Darkness", "year": 1992, "actors":
    ["Bruce Campbell", "Embeth Davidtz", "Marcus Gilbert", "Bridget Fonda",
     "Ted Raimi", "Patricia Tallman"]
  },
  {"movie": "A Simple Plan", "year": 1998, "actors":
    ["Bill Paxton", "Billy Bob Thornton", "Bridget Fonda", "Brent Briscoe"]
  },
  {"movie": "Spider-Man 2", "year": 2004, "actors":
    ["Tobey Maguire", "Kristen Dunst", "Alfred Molina", "James Franco",
     "Rosemary Harris", "J.K. Simmons", "Stan Lee", "Bruce Campbell"]
  },
  {"movie": "Drag Me to Hell", "year": 2009, "actors":
    ["Alison Lohman", "Justin Long", "Lorna Raver", "Dileep Rao", "David Paymer"]
  }
]
```

Or like this? (Hint: we do!)

```json
[
  {"run": 1, "luminosityBlock": 156, "event": 46501,
   "PV": {"x": 0.243, "y": 0.393, "z": 1.451},
   "electron": [],
   "muon": [
     {"pt": 63.043, "eta": -0.718, "phi": 2.968, "mass": 0.105, "charge": 1},
     {"pt": 38.120, "eta": -0.879, "phi": -1.032, "mass": 0.105, "charge": -1},
     {"pt": 4.048, "eta": -0.320, "phi": 1.038, "mass": 0.105, "charge": 1}
   ],
   "MET": {"pt": 21.929, "phi": -2.730}
  },
  {"run": 1, "luminosityBlock": 156, "event": 46502,
   "PV": {"x": 0.244, "y": 0.395, "z": -2.879},
   "electron": [
     {"pt": 21.902, "eta": -0.702, "phi": 0.133, "mass": 0.005, "charge": 1},
     {"pt": 42.632, "eta": -0.979, "phi": -1.863, "mass": 0.008, "charge": 1},
     {"pt": 78.012, "eta": -0.933, "phi": -2.207, "mass": 0.018, "charge": -1},
     {"pt": 23.835, "eta": -1.362, "phi": -0.621, "mass": 0.008, "charge": -1}
   ],
   "muon": [],
   "MET": {"pt": 16.972, "phi": 2.866}}
]
```

It might be possible to turn these datasets into tabular form using surrogate keys and database normalization, but

 * they could be inconvenient or less efficient in that form, depending on what we want to do,
 * they were very likely _given_ in a ragged/untidy form. You can't ignore the data-cleaning step!

Dealing with these datasets as JSON or Python objects is inefficient for the same reason as for lists of numbers.

We want arbitrary data structure with array-oriented interface and performance...

![](img/awkward-motivation-venn-diagram.svg){width="40%"}

## Libraries for irregular arrays

This is still a rather new field, but there are a few array libraries for arbitrary data structures.

* [Apache Arrow](https://arrow.apache.org/): In-memory format and an ecosystem of tools, an "exploded database" (database functionality provided as interchangeable pieces). Strong focus on delivering data, zero-copy, between processes.
* [Awkward Array](https://awkward-array.org/): Library for array-oriented programming like NumPy, but for arbitrary data structures. Losslessly zero-copy convertible to/from Arrow and Parquet.
* [Parquet](https://parquet.apache.org/): Disk format for storing large datasets and (selectively) retrieving them.
* [ROOT RNTuple](https://root.cern/doc/v622/md_tree_ntuple_v7_doc_README.html): A more flexible disk format than Parquet, in the [ROOT](https://root.cern/) framework.

### Apache Arrow

Arrow arrays support arbitrary data structures, but they're intended to be used within some other interface. Increasingly, dataframe libraries are adopting Arrow as a column format.

```python
import pyarrow as pa
```

```python
arrow_array = pa.array([
    [{"x": 1.1, "y": [1]}, {"x": 2.2, "y": [1, 2]}, {"x": 3.3, "y": [1, 2, 3]}],
    [],
    [{"x": 4.4, "y": [1, 2, 3, 4]}, {"x": 5.5, "y": [1, 2, 3, 4, 5]}]
])
```

```python
arrow_array.type
```

```python
arrow_array
```

### Awkward Array

Awkward Arrays are fully interchangeable with Arrow arrays, but they're a high-level user interface, intended for data analysts.

```python
import awkward as ak
```

```python
awkward_array = ak.from_arrow(arrow_array)
awkward_array
```

### Parquet

Awkward Arrays don't lose any information when saved as Parquet files.

```python
ak.to_parquet(awkward_array, "/tmp/file.parquet")
```

```python
ak.from_parquet("/tmp/file.parquet")
```

### RNTuple

At the time of writing, RNTuple is very new and I can't provide a similar example.

**TO DO:** Write this section when the tools are ready!

## Array-oriented programming with Awkward Arrays

Let's start with an example to show slicing.

You can find more details in the [Awkward Array documentation](https://awkward-array.org/).

```python
ragged = ak.Array([
    [
      [[1.84, 0.324]],
      [[-1.609, -0.713, 0.005], [0.953, -0.993, 0.011, 0.718]],
      [[0.459, -1.517, 1.545], [0.33, 0.292]],
      [[-0.376, -1.46, -0.206], [0.65, 1.278]],
      [[], [], [1.617]],
      []
    ],
    [
      [[-0.106, 0.611]],
      [[0.118, -1.788, 0.794, 0.658], [-0.105]]
    ],
    [
      [[-0.384], [0.697, -0.856]],
      [[0.778, 0.023, -1.455, -2.289], [-0.67], [1.153, -1.669, 0.305, 1.517, -0.292]]
    ],
    [
      [[0.205, -0.355], [-0.265], [1.042]],
      [[-0.004], [-1.167, -0.054, 0.726, 0.213]],
      [[1.741, -0.199, 0.827]]
    ]
])
```

### Multidimensional indexing

```python
ragged[3, 1, -1, 2]
```

### Range-slicing

```python
ragged[3, 1:, -1, 1:3]
```

### Advanced slicing

```python
ragged[[False, False, True, True], [0, -1, 0, -1], 0, -1]
```

### Awkward slicing

Just as NumPy arrays can be sliced by NumPy arrays, Awkward Arrays can be sliced by Awkward Arrays. This lets you perform physics cuts on ragged data.

See the full [Awkward slicing documentation](https://awkward-array.org/doc/main/reference/generated/ak.Array.html#ak.Array.__getitem__) for more.

```python
ragged > 0
```

```python
ragged[ragged > 0]
```

### Reductions

```python
ak.sum(ragged)
```

```python
ak.sum(ragged, axis=-1)
```

```python
ak.sum(ragged, axis=0)
```

Just as regular arrays can be sliced along different axes,

![](img/example-reducer-2d.svg){width="50%"}

ragged arrays (even with missing values like `None`) can be sliced by defining them to be left-aligned:

![](img/example-reducer-ragged.svg){width="50%"}

```python
array = ak.Array([[   1,    2,    3,    4],
                  [  10, None,   30      ],
                  [ 100,  200            ]])
```

```python
ak.sum(array, axis=0).tolist()
```

```python
ak.sum(array, axis=1).tolist()
```

In most particle physics applications, you'll want to reduce over the deepest/maximum `axis`, which you can get with `axis=-1`.

## Awkward Arrays in particle physics

Here is a bigger version of the JSON dataset, loaded from a ROOT file with [Uproot](https://uproot.readthedocs.io/).

```python
import uproot
```

```python
file = uproot.open("data/SMHiggsToZZTo4L.root")
file
```

```python
file.classnames()
```

```python
tree = file["Events"]
tree
```

```python
tree.arrays()
```

Here it is again (with more JSON-like formatting) from a Parquet file.

```python
events = ak.from_parquet("data/SMHiggsToZZTo4L.parquet")
events
```

View the first event as Python lists and dicts (like JSON).

```python
events[0].to_list()
```

Get one numeric field (also known as "column").

```python
events.electron.pt
```

Compute something ($p_z = p_T \sinh\eta$).

```python
import numpy as np
```

```python
events.electron.pt * np.sinh(events.electron.eta)
```

The [Vector](https://vector.readthedocs.io/) library also has an Awkward Array backend, which is [documented here](https://vector.readthedocs.io/en/latest/src/awkward.html). To use it, call `register_awkward` after importing the library.

```python
import vector
vector.register_awkward()
```

Now records with `name="Momentum4D"` and fields with coordinate names (`px`, `py`, `pz`, `E` or `pt`, `phi`, `eta`, `m`) automatically get Vector properties and methods.

```python
events.electron.type.show()
```

```python
# implicitly computes pz = pt * sinh(eta)
events.electron.pz
```

To make histograms or other plots, we need numbers without structure, so [ak.flatten](https://awkward-array.org/doc/main/reference/generated/ak.flatten.html) the array.

```python
from hist import Hist
```

```python
Hist.new.Regular(100, 0, 100, name="electron pT (GeV)").Double().fill(
    ak.flatten(events.electron.pt)
).plot();
```

Each event has a different number of electrons and muons ([ak.num](https://awkward-array.org/doc/main/reference/generated/ak.num.html) to check).

```python
ak.num(events.electron), ak.num(events.muon)
```

So what happens if we try to compute something with the electrons' $p_T$ and the muons' $\eta$?

```python
:tags: ["raises-exception"]
events.electron.pt * np.sinh(events.muon.eta)
```

This is data structure-aware, array-oriented programming.

### Application: Filtering events with an array of booleans

`events.MET.pt > 20` is a boolean array with the same length as `events`, so applying it as a slice selects events with MET $p_T > 20$ GeV.

```python
events.MET.pt, events.MET.pt > 20
```

After slicing, the number of events is smaller.

```python
len(events), len(events[events.MET.pt > 20])
```

### Application: Filtering particles with an array of lists of booleans

`events.electron.pt > 30` is a _ragged_ boolean array with the same length-per-list as `events.electron`. Applying it as a slice selects electrons with $p_T > 30$ GeV.

```python
events.electron.pt, events.electron.pt > 30
```

After slicing, the number of electrons in each event can be smaller.

```python
ak.num(events.electron), ak.num(events.electron[events.electron.pt > 30])
```

**Mini-quiz 1:** Using the reducer [ak.any](https://awkward-array.org/doc/main/reference/generated/ak.any.html), how would we select _events_ in which any electron has $p_T > 30$ GeV/c$^2$?

### Ragged combinatorics

Awkward Array has two combinatorial primitives:

* [ak.cartesian](https://awkward-array.org/doc/main/reference/generated/ak.cartesian.html) takes a [Cartesian product](https://en.wikipedia.org/wiki/Cartesian_product) of lists from $N$ different arrays, producing an array of lists of $N$-tuples.
* [ak.combinations](https://awkward-array.org/doc/main/reference/generated/ak.combinations.html) takes $N$ [samples without replacement](http://prob140.org/sp18/textbook/notebooks-md/5_04_Sampling_Without_Replacement.html) of lists from a single array, producing an array of lists of $N$-tuples.

```python
numbers = ak.Array([[1, 2, 3], [], [4]])
letters = ak.Array([["a", "b"], ["c"], ["d", "e"]])
```

```python
ak.cartesian([numbers, letters])
```

```python
values = ak.Array([[1.1, 2.2, 3.3, 4.4], [], [5.5, 6.6]])
```

```python
ak.combinations(values, 2)
```

Often, it's useful to separate the separate the left-hand sides and right-hand sides of these pairs with [ak.unzip](https://awkward-array.org/doc/main/reference/generated/ak.unzip.html), so they can be used in mathematical expressions.

```python
electron_muon_pairs = ak.cartesian([events.electron, events.muon])
electron_in_pair, muon_in_pair = ak.unzip(electron_muon_pairs)
```

```python
electron_in_pair.pt, muon_in_pair.pt
```

```python
ak.num(electron_in_pair), ak.num(muon_in_pair)
```

To use Vector's [deltaR](https://vector.readthedocs.io/en/latest/src/vector3d.html#vector._methods.VectorProtocolSpatial.deltaR) method ($\Delta R = \sqrt{\Delta\phi^2 + \Delta\eta^2}$), we need to have the electrons and muons in separate arrays.

```python
electron_in_pair, muon_in_pair = ak.unzip(ak.cartesian([events.electron, events.muon]))
```

```python
electron_in_pair.deltaR(muon_in_pair)
```

Same for combinations from a single collection.

```python
first_electron_in_pair, second_electron_in_pair = ak.unzip(ak.combinations(events.electron, 2))
```

```python
first_electron_in_pair.deltaR(second_electron_in_pair)
```

### Application: Z mass computed from all combinations of electrons

A rough Z mass plot can be made in three lines of code.

```python
first_electron_in_pair, second_electron_in_pair = ak.unzip(ak.combinations(events.electron, 2))
z_mass = (first_electron_in_pair + second_electron_in_pair).mass
```

```python
Hist.new.Reg(120, 0, 120, name="mass (GeV)").Double().fill(
    ak.flatten(z_mass, axis=-1)
).plot();
```

## Lesson 3 project: Higgs combinatorics from arrays

As described in the [intro](0-intro.md), navigate to the `notebooks` directory and open `lesson-3-project.ipynb`, then follow its instructions.
