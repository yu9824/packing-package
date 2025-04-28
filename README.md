# PACKING-PACKAGES

[![CI](https://github.com/yu9824/packing-packages/actions/workflows/CI.yml/badge.svg)](https://github.com/yu9824/packing-packages/actions/workflows/CI.yml)
[![docs](https://github.com/yu9824/packing-packages/actions/workflows/docs.yml/badge.svg)](https://github.com/yu9824/packing-packages/actions/workflows/docs.yml)
[![release-pypi](https://github.com/yu9824/packing-packages/actions/workflows/release-pypi.yml/badge.svg)](https://github.com/yu9824/packing-packages/actions/workflows/release-pypi.yml)

[![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json)](https://github.com/astral-sh/ruff)
[![mypy](https://www.mypy-lang.org/static/mypy_badge.svg)](https://github.com/python/mypy)

[![python_badge](https://img.shields.io/pypi/pyversions/packing-packages)](https://pypi.org/project/packing-packages/)
[![license_badge](https://img.shields.io/pypi/l/packing-packages)](https://pypi.org/project/packing-packages/)
[![PyPI version](https://badge.fury.io/py/packing-packages.svg)](https://pypi.org/project/packing-packages/)
[![Downloads](https://static.pepy.tech/badge/packing-packages)](https://pepy.tech/project/packing-packages)

<!-- [![Conda Version](https://img.shields.io/conda/vn/conda-forge/packing-packages.svg)](https://anaconda.org/conda-forge/packing-packages)
[![Conda Platforms](https://img.shields.io/conda/pn/conda-forge/packing-packages.svg)](https://anaconda.org/conda-forge/packing-packages) -->

This module provides functionality to pack conda environments and their dependencies into a specified directory.

You can use this package to migrate a conda environment to another offline machine with the same operating system.


## Install

```bash
# Need python, setuptools, pip package
pip install packing-packages

```

## How to use

see `--help`.

### Pack

```bash
packing-package pack -d .

```

### Install

```bash
packing-package install .

```

## Example

### Source device

```bash
conda activate <envname>
python -m pip install packing-packages
python -m packing_packages pack -d .

```

### Destination device (offline)

```bash
conda create -yn <envname> --offline
conda activate <envname>
conda install --use-local --offline ./conda/*
python -m pip install --no-deps --no-build-isolation ./pypi/*

```

## Notes

If you want to use pip to download from a source other than pypi.org (e.g. pytorch), the above will not completely collect. You will see packages that failed to download, so you will have to download them manually.
This is very complicated and we do not plan to support it.

```bash
# install command
pip install torch==2.5.1 --index-url https://download.pytorch.org/whl/cu124

# download command
# --no-deps: only target package
# -d: destination directory to download
pip download torch==2.5.1 --index-url https://download.pytorch.org/whl/cu124 --no-deps -d .
```

Some older packages may not install with `pip install`. In this case, you may succeed by installing only the failed packages with the `--use-pep517` option.
Please perform this manually.

There are two methods of installation: one is to use the standard functions of conda and pip, and the second is to use the `paciking-package install` command, which wraps them. The advantages of each are summarized in the table below.


| Commands                          | Advantages                                      | Disadvantages                     |
| --------------------------------- | ----------------------------------------------- | --------------------------------- |
| `conda install`  or `pip install` | Fast                                            | Stop immediately when cause error |
| `packing-package install`         | Skip when error occurs and tell you the package | Slow                              |


If you have an env file (.yaml), please create a virtual environment from it and then pack it.

```bash
# on online device with same OS
conda env create -f=env_name.yml
conda activate <env_name>
python3 -m pip install packing-packages
packing-packages pack -d .
```
