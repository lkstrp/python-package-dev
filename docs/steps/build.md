# 2. Build

Now that you have written your code, it is time to build a package. This is the first step in making your code available to the world. It is basically the process of going from a directory containing your code to a package that can be installed using a package manager such as `pip`, `conda` or `uv`.

!!! example

    See demo project:
    
    :material-ray-start: [Before](https://github.com/lkstrp/python-package-demo/commit/ff576bf0d8b3ec3cdff19477e2c12c7cc72c834b) | :material-ray-end: [After](https://github.com/lkstrp/python-package-demo/commit/2b316bbf257d398ec948d4d7a95202b3b41ae49c) | :octicons-diff-16: [Diff](https://github.com/lkstrp/python-package-demo/compare/ff576bf0d8b3ec3cdff19477e2c12c7cc72c834b...2b316bbf257d398ec948d4d7a95202b3b41ae49c)

## Packaging
To package your code, you need to choose a build system. The most common ones are `setuptools` and `poetry`. Each has its own advantages and disadvantages. For a detailed guide, see the official [packaging guide](https://packaging.python.org/en/latest/tutorials/packaging-projects/):material-file-document:.

### Project Structure

In short, you want to create a couple of files and restructure your code:

```
python-package-demo/
├── src/
│   └── python_package_demo/
│       ├── __init__.py
│       └── example.py
├── LICENSE
├── pyproject.toml
├── README.md
└── ...
```

- `LICENSE`: The license file for your project. This is important to let people know how they can use your code.
- `pyproject.toml`: The configuration file for your package. This is where you define the build system and other metadata for your package. It can also be used to configurate other tools like `ruff`, `uv`, `mypy` and `black`.
- `README.md`: The readme file for your project. This is where you provide information about your project and how to use it.
- `src/`: The source code of your project.

!!! info

    The `src/` directory is not required as a parent directory, but it is recommended to use it. It helps to avoid naming conflicts with other packages, and makes it easier to manage your code. For more details, see [src layout vs flat layout](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/):material-file-document:.

!!! info

    While `setup.py` is still supported and [not deprecated](https://packaging.python.org/en/latest/discussions/setup-py-deprecated/):material-file-document:, using a single `pyproject.toml` to configure your package is the recommended way to go.

### `pyproject.toml` file
As a simple example, we will use `setuptools` as the build system. The `pyproject.toml` file is the configuration file for your package. It contains all the metadata and configuration for your package. Here is a simple example:

```toml
[build-system]
requires = ["setuptools", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "python-package-demo"
version = "0.1.0"
description = "A demo project for packaging python code"
readme = "README.md"
license = { file = "LICENSE" }
authors = [{name = "Your Name", email = "your-email@mail.org"}]
requires-python = ">=3.8"
dependencies = [
    "numpy>=1.21.0",
    "pandas>=1.3.0",
]
```

This will already be enough to install your package via `pip`:

```bash
pip install .
# or 
pip install -e . # for editable mode
```

You can also use `pip` to build your package:

```bash
pip wheel .
```

This is the way how you publish your package to the world. It is discussed in the [publish](publish.md) section.

!!! info

    As you can see, we hardcode the current version into the `pyproject.toml` file. This means you have to bump the version on each release. [`setuptools_scm`](https://setuptools-scm.readthedocs.io/en/latest/):material-file-document: is another build system which can retrieve the version from your git tags. This is how you would manage your releases and trigger a CI pipeline anyway, so you don't have to worry about updating the version in your `pyproject.toml` file.

### More metadata

You can add more metadata to your package. See [Writing your pyproject.toml](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#writing-pyproject-toml):material-file-document: for more information. Here we add some classifiers and keywords to the package. They are picked up by PyPI, for a full list of available classifiers see [PyPI Classifiers](https://pypi.org/classifiers/):material-file-document:.

```toml
[project]
# ...
classifiers = [
    # Specify the Python versions you support here
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
    "Programming Language :: Python :: 3.12",
    "Programming Language :: Python :: 3 :: Only",
    # License
    "License :: OSI Approved :: MIT License",
    # Operating System
    "Operating System :: OS Independent",
]
keywords = ["python", "package", "demo"]

[project.urls]
Homepage = "https://example.com"
```

### License
The licence file is important to let people know how they can use your code. Tools like [choosealicense.com/](https://choosealicense.com):material-link: can help you choose a licence. There is also an [OpenMod Guide](https://wiki.openmod-initiative.org/wiki/Choosing_a_license):material-file-document: for choosing a licence.

## Resources
- [Python Packaging Authority (PyPA)- Python Packaging User Guide](https://packaging.python.org/en/latest/tutorials/packaging-projects/)
- [Python Packaging Authority (PyPA) - pyproject.toml specification](https://packaging.python.org/en/latest/specifications/pyproject-toml/)
- [Setuptools documentation](https://setuptools.pypa.io/en/latest/)
- [The Hitchhiker's Guide to Packaging](https://the-hitchhikers-guide-to-packaging.readthedocs.io/en/latest/)
- [Packaging Python Libraries - Real Python tutorial](https://realpython.com/pypi-publish-python-package/)
- [Scientific Python Library Development Guide](https://learn.scientific-python.org/development/)