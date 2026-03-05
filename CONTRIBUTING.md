## I. About
Crocoddyl has been written in C++ for efficiency and uses the  Eigen  library  for  linear  algebra  routines.  It  comes  with Python bindings for easy prototyping. Crocoddyl is currently supported  for  most  Linux  distributions,  with  plans  to  release on Mac OS X and Windows. The project is fully open-source under  the  permissive  BSD-3-Clause  license  and  is  hosted on  GitHub.

## II. Design pattern
Crocoddyl is designed using virtualization pattern for easy prototyping and yet efficient implementation. In early benchmark, we have shown that virtualization is as efficient as static polymorphism (i.e. CRTP design) for system dynamics higher than 16. The real benefits of static polymorphism are in very small system (with dimension less than 6), however, virtualization is still competitive.

One of the main design concepts of Crocoddyl is the strict separation between _model_ and _data_.  A model describes a system or procedure, e.g. action, cost, and activation models. Any model has been abstracted to  easily derive new systems in both C++ and Python. By data, we refer to a container that stores computed and intermediate values used during the calculation routine performed by a model. Each model creates its own data, and with this, we avoid any temporary memory allocation produced by algebraic expressions in Eigen. To improve  efficiency, especially in systems with dimension slower than 16, the Eigen members inside a data object can be  defined with fixed dimensions. It allows for efficient use of modern CPU features by using vectorization and  Same Instruction Multiple Data (SIMD) operations.



### 1. Direct use of the provided docker image

```bash
docker run --rm -v $PWD:/app -w /app -it gepetto/linters
```

This will not work if the root user doesn't have the right to go into your current working directory.
And if your current working directory is accessed through symlinks, you might need to replace `$PWD` by `$(pwd -P)`.

### 2. Manual use of each tool
Before starting with it, you need to clone **Gepetto/linter** repository, i.e.

```bash
git clone git@github.com:Gepetto/linters.git
sudo apt install clang-format-6.0
pip install --user flake8 isort yapf
```

**Please note yapf is in a beta version, we tested our system with v0.27.0.**

*Please note that with the following instructions your current code will be updated*

#### 2.1. Formatting your c++ code

For c++ testing through clang-format. We used the configuration file defined in the Gepetto linter. Then, you need to install the configuration files in the current working directory (beware of git if doing so) or in a parent directory. Your home might be a good choice:

```bash
cd ${YOUR_CROCCODDYL_SOURCE_PATH}
ln -s ${YOUR_GEPETTO_LINTER_PATH}/.clang-format .clang-format
```

And for formatting your c++ code, you just need to run the follows:

```bash
cd ${YOUR_CROCCODDYL_SOURCE_PATH}
clang-format-6.0 -i $(find . -path ./cmake -prune -o -iregex '.*\.\(h\|c\|hh\|cc\|hpp\|cpp\|hxx\|cxx\)$' -print)
```

#### 2.2. Formatting your Python code

```bash
cd ${YOUR_CROCCODDYL_SOURCE_PATH}
flake8 .
yapf -ri .
```
