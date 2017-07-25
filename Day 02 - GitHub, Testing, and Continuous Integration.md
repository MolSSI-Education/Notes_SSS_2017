# Tue 25 Jul - morning

## GitHub introduction

-- Fill this in later --

# Tue 25 Jul - afternoon 

## GitHub integration

GitHub integrations can help you make sure your software actually works on a large variety of hardware.

### Creating a new GitHub project

We'll start by creating a new repository.

When we create a new repository, we have to make two important choices:

#### Choosing a `.gitignore`

You don't necessarily want temporary files

#### Choosing a license

Clicking on the `?`next to the liceinse chooser will take you to a license browser to help you select a license.

It's a good idea to read through the [GNU GPL Manifesto](https://www.gnu.org/gnu/manifesto.en.html).
There are many nice aspects to the [GNU GPL License](https://www.gnu.org/licenses/gpl-3.0.en.html), but it can get complicated when companies want to make use of it that may not want to contribute their changes/improvements back to the original project. 

If you're concerned about patents, the [Apache License](https://opensource.org/licenses/Apache-2.0) is often a good choice, and corporations are often happy to use it.

The [MIT License](https://opensource.org/licenses/MIT) is a minimal license with few restrictions that works well for academia and industry

There's even a [Do What the Fuck You Want To](http://www.wtfpl.net/) license that is legally valid.

The [Open Source Initiative](https://opensource.org/) has a lot of great information to help guide you.
All [MolSSI](http://molssi.org)-created software must follow the [OSI Open Source Definition](https://opensource.org/osd-annotated).
The OSI maintains a list of [popular OSI-approved licenses](https://opensource.org/licenses).

MolSSI is currently recommending youuse the [BSD 3-Clause license](https://opensource.org/licenses/BSD-3-Clause) for new projects.

Projects like [`scipy`](https://www.scipy.org/scipylib/license.html) are permissively licensed.

**If you do not explicitly provide a license, people cannot use your code since you are not explicitly granting them permission to use it and setting guidelines for how they can use it! Always remember to set a license.**

MolSSI is currently working on guidance documentation 

#### Choose a repo name

Pick a name that you won't be ashamed to have on your professional outward GitHub profile.

We chose `friendly-computing-machine`.

#### Write a basic project description

It's good to write a brief description of your project.

### Clone the repo to work with it

In your `~/SSS_2017/Gits/` folder (not in your `planets/` folder), you can clone the new repository.
```bash
git clone https://github.co/dgasmith/friendly-computing-machine.git
```
Let's rename it:
```
mv friendly-computing-machine fcm
cd fcm
```
Now if I list the directory with `ls`, I see
```
LICENSE   README.md
```
Note that the `.gitignore` file is hidden by default. You'll need to use `ls -a` to see it
```
.         ..         .git        .gitignore LICENSE    README.md
```

### Building a Python module

We'll start by building a [Python module](https://docs.python.org/3/tutorial/modules.html).

Don't put all your Python code in the top level! Instead, create a subdirectory with the same name as your Python module, which should [follow PEP 8 module naming guidelines](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).
```bash
mkdir friendly_computing_machine
```
Now, we begain creating a file called `friendly_computing_machine/math.py`.

At the top of the header, write a docstring describing what your file does:
```python
"""
A small set of functions for doing math operations.
"""
```
Now we can start writing functions. Let's write a function that adds two numbers:
```python
def add(arg1, arg2):
    """
    Function that adds two arguments.
    """
    return 2 + 5
```
Every file in Python is actually an importable script by default:
```
>>> import math
```
You can check where this imported file physically lives on your machine, since we want to be sure we're using our own `math.py` and not the system `math` module:
```
>>> print(math.__file__)
/Users/daniel/SSS_2017/Gits/friendly-computing-machine/friendly-computing-machine/math.py
```
The current working directory (`.`) is the first thing that Python searches when you try to import a module.

To run the function, we can write
```
>>> math.add(2, 5)
7
>>>
```
To make this a module, we need to create an `__init__.py` file in the `friendly-computing-machine/` module directory:
```python
"""
This is the base file of the friendly-computing-machine!
"""
```
Documentation is crucial to any open-source project. Imagine if `numpy` had no documentation with it---who would be able to use it then?
Always create documentation for your files and functions!

Now we can go back to our base git directory `fcm/`, which we can now import
```
>>> import friendly_computing_machine as fcm
>>> fcm.math.add(2,5)
7
```
What if we also want to be able to multiply, using the Python `math` library? 
In the `__init__.py` file, we can add
```python
from . import math
from .math import mult
```
Now we can type
```
>>> import friendly_computing_machine as fcm
>>> fcm.mult(2,3)
6
```
