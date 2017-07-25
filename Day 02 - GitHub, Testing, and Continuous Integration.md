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
cd friendly_computing_machine
```
Now, we begain creating a file called `math.py`.

At the top of the header, write a docstring describing what your file does:
```
"""
A small set of functions for doing math operations.
"""
```
Now we can start writing functions.
