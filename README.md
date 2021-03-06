# PyWFC
## Overview
PyWFC is an implementation of the [Wave Function Collapse Algorithm](https://github.com/mxgmn/WaveFunctionCollapse).
## Installation
Use `pip install pywfc` to install via pip

Or use `git clone https://github.com/FoxNerdSaysMoo/PyWFC && python PyWFC/setup.py install`

## Usage

Contains 3 main classes: `Wave`, `State`, `Rule`
```py
from wfc import Wave, State, Rule
```

`Rule` determines where a state can be, and `State` contains a `Rule` and a name.
```py
grass = State(
    "grass",
    Rule(
        lambda x, y : {  # Rule uses a lambda
            (x, y+1): ["air"],  # Which takes the x and y coordinate of the state
            (x, y-1): ["dirt", "stone"]  # And returns what the states relative to it must be
        }
    )
)

dirt = State(
    "dirt",
    Rule(
        lambda x, y: {
            (x, y+1): ["dirt", "grass"],
            (x, y-1): ["stone", "dirt"]
        }
    )
)

stone = State(
    "stone",
    Rule(
        lambda x, y : {
            (x, y+1): ["stone", "dirt"]
        }
    )
)

air = State(
    "air",
    Rule(
        lambda x, y : {
            (x, y+1): ["air"]
        }
    )
)
```

A `Wave` takes in the dims of the output grid, along with the states that will populate it.
```py
landscape_wave = Wave(
    (10, 40),
    [grass, dirt, stone, air]
)
```
Now all you need to do is collapse the wave!
```py
landscape = landscape_wave.collapse()
```
Now you have a multidimensional list of `Potential`s that have one single state now.
> `Potential` is a class used inside of wfc, which represents the potentials states before, during, and after the wave function collapse.

```py
import colorama

cmap = {
    "air": colorama.Fore.WHITE,
    "grass": colorama.Fore.GREEN,
    "dirt": colorama.Fore.YELLOW,
    "stone": colorama.Fore.BLACK
}

for row in landscape:
    for item in row:
        print(cmap[item.state.name] + "█" + colorama.Fore.RESET, end="")
    print()
```
There you go! Those are the current classes and example usages of them. There are many planned features and expansions to be added to PyWFC, however we are taking our time to ensure they are the best possible they can be.

### Thanks for checking out PyWFC, and we hope you like it ;)
