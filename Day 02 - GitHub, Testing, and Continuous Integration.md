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

def mult(arg1, arg2):
    """
    Function that multiplies two arguments.
    """
    return 2 * 5    
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
What if we also want to be able to call this as `fcm.add` instead of `fcm.math.add`?
In the `__init__.py` file, we can add
```python
from . import math
from .math import add, mult
```
Now we can type
```
>>> import friendly_computing_machine as fcm
>>> fcm.add(2,3)
6
```

Note that there's a `__pycache__` directory sitting around.
What happens if we `git add __pycache__`?
```bash
$ git add __pycache__
$ git status
```
Nothing happened! That's because that directory is excluded by our `.gitignore`.

### Testing

Tests are important to make sure our code works as expected.

Now, we need a testing framework.
In this case, we'll use [pytest](https://docs.pytest.org/en/latest/).
To check if you have `pytest` correctly installed, we can check from our shell
```bash
$ python -c "import pytest"
```
If that gives an error, we can install via `conda`:
```
$ conda install --yes pytest
```
It's best to stay within the [conda](https://conda.io/docs/intro.html) framework if you have it installed!

In our base git folder `fcm/` create a directory called `tests/`:
```bash
$ mkdir tests
```
Now create a test file `tests/test_math.py`
```python
"""
Testing for the math.py module
"""

import friendly_computing_machine as fcm
import pytest

def test_add():
    assert fcm.add(2, 5) == 7
```
Before we can run `py.test`, we have to install our package:

### Making the Python module installable

We'll start with a preconfigured `setup.py`, since it's much easier that way:
```python
import setuptools

if __name__ == "__main__":
    setuptools.setup(
        name='friendly-computing-machine',
        version="0.1.1",
        description='A starting template for Python programs',
        author='Daniel Smith',
        author_email='dgasmith@vt.edu',
        url="https://github.com/dgasmith/friendly-computing-machine",
        license='BSD-3C',
        packages=setuptools.find_packages(),
        install_requires=[
            'numpy>=1.7',
        ],
        extras_require={
            'docs': [
                'sphinx==1.2.3',  # autodoc was broken in 1.3.1
                'sphinxcontrib-napoleon',
                'sphinx_rtd_theme',
                'numpydoc',
            ],
            'tests': [
                'pytest',
                'pytest-cov',
                'pytest-pep8',
                'tox',
            ],
        },

        tests_require=[
            'pytest',
            'pytest-cov',
            'pytest-pep8',
            'tox',
        ],

        classifiers=[
            'Development Status :: 4 - Beta',
            'Intended Audience :: Science/Research',
            'Programming Language :: Python :: 2.7',
            'Programming Language :: Python :: 3',
        ],
        zip_safe=True,
    )
```

Add to the `README.md` some info on how to install:
```
To install in-place:
pip install -e .
```

### Running the tests

We can run the tests with
```
py.test -v
```
and see that it passes!

Instead of coding up many different values to feed through `fcm.add`, we can use [parameterized tests](https://docs.pytest.org/en/latest/example/parametrize.html) to feed a lot of possibilities through:
```
testdata = [
    (2, 5, 10),
    (1, 2, 2),
    (11, 9, 99),
    (11, 0, 0),
    (0, 0, 0)
    ]
@pytest.mark.parameterize("a,b,expected", testdata)
def test_mult(a, b, expected):
    assert fcm.math.mult(a, b) == expected
```

[Test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) is a process in which we first implement the *tests* so we have a way to measure whether the code that we are writing is working during the process of writing and debugging.

### Continuous integration

[Continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) is a process where our code is tested every time we push it to GitHub. This process is immensely useful because it allows us to fix bad code before it gets merged into the main repository branches, and all major software projects that have good programming practices use this principle.

We'll be using the free service called [travis-ci](http://travis-ci.org) to do continuous integration.
(Make sure to use `travis-ci.org` and not `travis-ci.com`, since you have to pay for the `.com` service.)

You'll want to go to http://travis-ci.org to set this up and link it with your GitHub account.
Once you've created your account, go to Travis's view of your repositories, and make sure to "Sync from GitHub" to get a view of the latest versions of your GitHub repos.
We'll switch the `friendly-computing-machine` switch to ON.

To tell Travis how to install and test our software automatically, we will create a file in our base github repo directory called `.travis.yml`:
```YAML
# After changing this file, check it o:
#   http://lint.travis-ci.org/
language: python

# Run obs on container-based infrastructure, can be overridden per job
sudo: false

matrix:
  include:
    - python: 2.7
    - python: 3.5
    - python: 3.6

before_install:
  # These things are really useful for debugging when things go wrong
  - uname -a
  - free -m
  - df -h
  - ulimit -a
  - python -V
  # Install environment via pip
  - pip install --upgrade pip setuptools
  - pip install pytest pytest-cov
  - pip install numpy
  - pip install codecov
  # Install my code
  - pip install -e .
  
script:
  - py.test -v
  
notifications:
    email: false

```
We'll want to commit and push `.travis.yml` to our repository, and then we should see the travis job appear in our account on http://travis-ci.org where we can see a detailed output report of what succeeded and what failed.

We've earned ourselves a badge! Click on the `Build:Passing` graphic from your project and select the "Markdown" format, and copy the Markdown source for your badge. It will look like this:
```
[![Build Status](https://travis-ci.org/dgasmith/friendly-computing-machine.svg?branch=master)](https://travis-ci.org/dgasmith/friendly-computing-machine)
```
Put it in your `README.md` at the top, commit, and push to your repo.

### Code coverage

How do we know how much of our code is actually being tested?

You can check code coverage locally with `codecov`:
```
$ pip install pytest pytest-cov
$ py.test -v --cov=./
```
This gets messy when there are lots of files, so online tools can be very helpful, especially if they test every version of the code we push to github.
We can use a convenient online integration called http://codecov.io/ that will check for us and point out which parts of the code are not covered.
Go to http://codecov.io/ and click "Sign Up", and then link your GitHub account.
Enabe codecov testing of `friendly_compute_machine`.

