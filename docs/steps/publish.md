# 8. Publish

We are ready to publish your package. Users don't want to install your package through a git hosting platform, but through a dedicated package manager. In the Python ecosystem, there are two main platforms for making your package available.


## PyPI
PyPI is the Python Package Index. It is a central repository for Python packages and is used by pip to install packages. PyPI is a good place to publish your package as it is the most widely used package manager for Python. There are several ways to publish your package to PyPI. A simple CI/CD integration is discussed in the [CI/CD section](ci.md). PyPI also has a [test instance](https://test.pypi.org/):material-link: which you can use to test your workflow. You could also use it to publish each commit to a test instance and then use the main instance for releases. But this is probably overkill for most projects.

### twine

If you'd like to play around with this process locally, you can use the [twine](https://twine.readthedocs.io/en/stable/) command line tool. Twine is a command line tool for uploading packages directly to PyPI. It is very simple:

[Using twine](https://twine.readthedocs.io/en/stable/#using-twine):material-file-document::

1. Create some distributions in the normal way:
    ```bash
    python -m build
    ```
2. Upload to Test PyPI and verify things look right:
    ```bash
    twine upload --repository testpypi dist/*
    ```
3. Verify the upload:
    ```bash
    pip install --index-url https://test.pypi.org/simple/ <your-package>
    ```
Or to upload to the main PyPI:
```bash
twine upload dist/*
```

!!! note

    This process expects that you have the package setup correctly, as discussed in the [build section](build.md).

## Conda

Conda is a package manager for Python and other languages. It is used to manage packages and environments and has the advantage of being able to handle non-Python packages. While you can install a pip package in a conda environment, it is recommended to use conda packages for conda environments. [Conda-forge](https://conda-forge.org/):material-link: is a community-driven collection of conda packages. It is the usual place to publish your package. You could of course use a different channel. You will need to set up a conda feed for your package once. See the [conda-forge documentation](https://conda-forge.org/docs/maintainer/adding_pkgs/):material-file-document: for more information. The process is a bit more complicated than PyPI. But then condaforge can be triggered by any new PyPI release, so you only need to trigger it once.

## Package manager
While `pip` is the most common package manager for PyPI, there are others worth mentioning:

- [pip](https://pip.pypa.io):material-link:
- [Poetry](https://python-poetry.org/):material-link: 
- [uv](https://docs.astral.sh/uv/):material-link:
- [pipx](https://pipx.pypa.io/stable//):material-link:

Also for conda there are other package managers:

- [conda](https://docs.conda.io):material-link:
- [mamba](https://mamba.readthedocs.io/en/latest/):material-link:

Here are additional resources you could add to the "Resources" section of your guide:

## Resources
- [PyPI Documentation](https://pypi.org/help/)
- [Conda-forge Documentation](https://conda-forge.org/docs/)
- [Twine Documentation](https://twine.readthedocs.io/en/latest/)
- [Packaging and Distributing Projects](https://packaging.python.org/guides/distributing-packages-using-setuptools/)
- [Build Documentation](https://pypa-build.readthedocs.io/en/stable/)
- [TestPyPI](https://test.pypi.org/)
- [Conda Package Maintenance](https://conda.io/projects/conda-build/en/latest/user-guide/tutorials/build-pkgs.html)


