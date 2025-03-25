# 6. Document

Without documentation, no one will know what your project does, how it works, and how to use it. Even without a large user base, having good documentation is an advantage. Your future self will thank you for it. Documentation is not only important for users, but also for developers.

!!! example

    See demo project:
    
    :material-ray-start: [Before](https://github.com/lkstrp/python-package-demo/commit/51b766c7d78c44085950ef487d2f59937492e44c) | :material-ray-end: [After](https://github.com/lkstrp/python-package-demo/commit/abc44f95c0d5573661e6e90884ea7a4823eb2448) | :octicons-diff-16: [Diff](https://github.com/lkstrp/python-package-demo/compare/51b766c7d78c44085950ef487d2f59937492e44c...abc44f95c0d5573661e6e90884ea7a4823eb2448)


## Types of Documentation
To provide documentation, there are generally two types of documentation: 

- **Inline documentation**: Comments, docstrings and also type hints in your code. This helps developers and yourself to understand the code, and can be used to generate documentation automatically.
- Dedicated documentation site: A separate documentation site that provides a more detailed overview of your software and how to use it.

### Inline Documentation
There is really no reason not to have good inline documentation with comments, docstrings and type hints. As discussed in the [type](type.md) section, type hints are self-documenting. Even if no one else ever uses or develops your project, good inline documentation will help keep your code readable and maintainable for your future self.

#### Docstrings
[PEP 257 - Docstring Conventions](https://peps.python.org/pep-0257/):material-file-document: defines the conventions for docstrings. Docstrings are used to document modules, classes, methods and functions. They can also be used to generate API reference documentation, which makes them very powerful. There are several styles, all of which follow the defined convention. Which one you choose is a matter of taste. But if you choose one, tools like ruff can lint your docstrings, and static site generators (see below) can generate a nice API reference without any further effort. This way, you get good inline documentation and a dedicated docs page at the same time.

##### numpy-style
[numpydoc](https://github.com/numpy/numpydoc/):material-link: is a docstring format used by the NumPy and SciPy projects, and is very popular in the scientific Python community. It is also the default format for many tools such as [Sphinx](#sphinx) and [MkDocs](#mkdocs).

```python
def add(a: float, b: float) -> float:
    """
    Add two numbers.

    Longer description of the function.

    Parameters
    ----------
    a : float
        The first number.
    b : float
        The second number.
    Returns
    -------
    float
        The sum of the two numbers.
    """
    return a + b
```

There are many [more sections](https://numpydoc.readthedocs.io/en/latest/format.html#sections):material-file-document: in numpydoc format. If you want to use the [mentioned](test.md) doctests, you can use the `Examples` section:

```python
def add(a: float, b: float) -> float:
    """
    ...

    Examples
    --------
    >>> add(1, 2)
    3.0
    >>> add(1.5, 2.5)
    4.0
    """
```

Check the [numpydoc style guide](https://numpydoc.readthedocs.io/en/latest/format.html):material-file-document: for more information.

##### Google-style
Another popular docstring format is the [Google style](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings):material-link:. It is used by the Google Python Style Guide, also used by many other projects and also supported by Sphinx and MkDocs.

```python
def add(a: float, b: float) -> float:
    """
    Add two numbers.

    Longer description of the function.

    Args:
        a (float): The first number.
        b (float): The second number.

    Returns:
        float: The sum of the two numbers.
    
    Examples:
        >>> add(1, 2)
        3.0
        >>> add(1.5, 2.5)
        4.0
    """
    return a + b
```

##### Sphinx-style
[Sphinx](#sphinx) also provides its own docstring format. It is not as popular as the other two formats and is not used by many projects. You need another extension installed, if you wanna use Sphinx with other formats, but besides that, there are no real advantages to use it.

```python
def add(a: float, b: float) -> float:
    """
    Add two numbers.

    Longer description of the function.

    :param a: The first number.
    :type a: float
    :param b: The second number.
    :type b: float
    :return: The sum of the two numbers.
    :rtype: float
    
    .. doctest::

        >>> add(1, 2)
        3.0
        >>> add(1.5, 2.5)
        4.0
    """
    return a + b
```


### Dedicated Documentation

Docstrings can be used to automatically generate an API reference. But for a complete documentation site, you need more than just the API reference. You can add a landing page, a getting started guide, tutorials, examples and more. The structure of always linking a user guide to an API reference and back is a good practice. This way a user can easily find information about the function signature, but also get an explanation of how to use it. See the [pydantic documentation](https://docs.pydantic.dev/latest/api/aliases/):material-file-document: for a good example of this structure.

Your additional documentation will live in `docs/` (or similar, but in its own directory):
```
python-package-demo/
├── docs/
│   ├── index.rst
│   └── ...
├── test/
│   ├── conftest.py  # when using pytest
│   └── test_example.py
├── src/
│   └── python_package_demo/
│       ├── __init__.py
│       └── example.py
├── LICENSE
├── pyproject.toml
├── README.md
└── ...
```

#### Readme
This starts with the README.md, which is usually the first thing your users will see. The documentation landing page is usually very similar to the README.md, or you can provide a custom landing page. Although there are no hard and fast rules about how a README.md should be structured, it is a good idea to stick to the standard. Tools like [makeareadme.com](https://www.makeareadme.com/):material-link: can help you create a good README.md with a good structure. Or check out [this guide](https://realpython.com/python-comments-guide/):material-file-document: or just browse through some [awesome readme's](https://github.com/matiassingers/awesome-readme):material-link: for inspiration.

#### Developer documentation
A dedicated developer page is also a good idea. It should contain information on how to contribute to the project, how to set up the development environment, how to run tests, and how to build the project. This is particularly important for open source projects.

## Tools
To create a dedicated website for your documentation, you need a static website generator and somewhere to host it.

### Static Site Generators
A static site generator is a tool that takes your documentation files and generates HTML files from them. There are different tools for this, and they all allow you to use different themes and plugins.

#### Sphinx
[Sphinx](https://www.sphinx-doc.org/en/master/):material-link: is a static page generator based on the [reStructured](https://docutils.sourceforge.io/rst.html):material-link: markup language. It also supports other formats (such as [Markdown](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#markdown)), but is designed for reStructured text. Sphinx is very powerful and flexible, and is probably the most popular tool used. As it has been around for a long time, there are a lot of plugins and themes available.

##### `.rst` vs `.md`
On the other hand, reStructured text is less intuitive to use and has a higher learning curve with a more challenging syntax than simple markdown. If you want to encourage community contributions to your documentation, markdown is the better choice, as it's easier to read and write. .rst` simply offers more features, but this is less of a problem with other more modern static site generators. Using Markdown also has the advantage that other viewers (such as your IDE or git hosting platform) are more likely to render it correctly.

`.rst` syntax:
```rst
Title
=====

A simple example with a `link <https://www.google.com>`_.

.. warning::

    This is a warning.

.. code-block:: python

    def add(a: float, b: float) -> float:
        """
        Add two numbers.

        Longer description of the function.

        :param a: The first number.
        :type a: float
        :param b: The second number.
        :type b: float
        :return: The sum of the two numbers.
        :rtype: float
        """
        return a + b
```

`.md` syntax:
```md

# Title

A simple example with a [link](https://www.google.com).

!!! warning

    This is a warning.

```python
def add(a: float, b: float) -> float:
    """
    Add two numbers.

    Longer description of the function.

    :param a: The first number.
    :type a: float
    :param b: The second number.
    :type b: float
    :return: The sum of the two numbers.
    :rtype: float
    """
    return a + b
```


#### MkDocs

[mkdocs](https://www.mkdocs.org/):material-link: is a static site generator based on Markdown and only supports Markdown. It is very easy to use and has a lot of themes and plugins available. The documentation you are reading is built with the [Material Theme MkDocs](https://squidfunk.github.io/mkdocs-material/getting-started/)[:material-link:]. It also provides plugins and most of the features missing from the standard markdown syntax. If you are not sure which one to use, mkdocs is a good choice.

#### GitBook
[GitBook](https://www.gitbook.com/):material-link: is another page generator based on Markdown that produces nice looking [docs](https://docs.gitbook.com/):material-link:. The main pro and con is that it is proprietary and not open source. Which has the advantage that they also provide a hosting solution.

### Hosting
The final step is to host your documentation somewhere so that it is accessible to the public.

#### readthedocs
The most popular tool used by many small to medium sized projects is [readthedocs](https://readthedocs.org/):material-link:. It is free for open source projects and has many features. It can automatically build your documentation and host it for you. It also has many integrations with other tools such as GitHub, GitLab, Bitbucket and many more. It is very easy to set up and you do not need to provide your own CI/CD pipeline. You simply connect your account, which adds a web hook to your repository. It is so popular that the generated url of "https://<package_name>.readthedocs.io/" is used as standard for many projects. Sphinx and MkDocs are both supported, and there are only a few [configurations](https://docs.readthedocs.com/platform/stable/config-file/v2.html):material-file-document: that you can set up in the `readthedocs.yml` file.

#### GitHub Pages
[GitHub Pages](https://pages.github.com/):material-link: is another free hosting solution for open source projects. Since many projects are already hosted on GitHub, integration with the GitHub ecosystem is very easy. You can use it with Sphinx, MkDocs or just plain Markdown and choose a theme via GitHub Pages.

## Other Resources
- [Markdown Guide](https://www.markdownguide.org/)
- [Documenting Python Code: A Complete Guide – Real Python](https://realpython.com/documenting-python-code/)
- [Python docstring formats (styles) and examples | note.nkmk.me](https://note.nkmk.me/en/python-docstring/)
- [Writing Comments in Python (Guide) – Real Python](https://realpython.com/python-comments-guide/)
- [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
- [Sphinx Documentation](https://www.sphinx-doc.org/en/master/)
- [MkDocs Documentation](https://www.mkdocs.org/)
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
- [Read the Docs Documentation](https://docs.readthedocs.io/)
- [Awesome Documentation Tools](https://github.com/unicodeveloper/awesome-documentation-tools)
- [Python Documentation Best Practices](https://docs.python-guide.org/writing/documentation/)