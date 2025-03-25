# 4. Format & Lint

A code formatter is a tool that automatically formats your code according to a set of rules. This helps to keep your code clean and consistent, which makes it easier to read and maintain, and simply more aesthetically pleasing. You do not have to format your code manually, and collaboration with other developers is easier because personal style is removed.

A code linter is a tool that checks your code for style violations and errors. It helps you find bugs and potential problems in your code. A linter can also be used to enforce coding standards and best practices.

The difference between a formatter and a linter is that a formatter automatically formats your code, while a linter checks your code for style violations and errors. Although some linters can fix some of the problems they find, they are not intended to be used as formatters. A linter is more like a code review tool that helps you find potential problems in your code.

[PEP 8 - Style Guide for Python Code](https://peps.python.org/pep-0008/):material-file-document: is the official style guide for Python code. It is a good idea to follow this style guide as it is widely accepted and used by many projects. The tools mentioned here mostly follow this style guide or define additional rules.

!!! example

    See demo project:
    
    :material-ray-start: [Before](https://github.com/lkstrp/python-package-demo/commit/c5bc66b7c5acee9b01f3285cf28960c500474dc2) | :material-ray-end: [After](https://github.com/lkstrp/python-package-demo/commit/51278f4ee1c455b203778e19ed6c26ad9eadfea4) | :octicons-diff-16: [Diff](https://github.com/lkstrp/python-package-demo/compare/c5bc66b7c5acee9b01f3285cf28960c500474dc2...51278f4ee1c455b203778e19ed6c26ad9eadfea4)

## Tools

There are several tools available for formatting your code. You can only use one of them, as a combination may cause conflicts. Linters can be used in combination with formatters and can check for a specific linting task or apply PEP 8 rules in a broader sense. See this [awesome list](https://github.com/life4/awesome-python-code-formatters):material-file-document: for more tools.

### `black`

[Black](https://black.readthedocs.io/en/stable/):material-link: is a code formatter that formats your code according to the PEP 8 style guide. It is opinionated and has a set of rules that it follows.

### `isort`

[isort](https://pycqa.github.io/isort/):material-link: is a code formatter that formats your imports. It sorts your imports according to the PEP 8 style guide and removes unused imports.

### `flake8`

[flake8](https://flake8.pycqa.org/en/latest/):material-link: is a code linter that checks your code for style violations and errors. It is a combination of `pyflakes`, `pycodestyle` and `mccabe`. It is widely used and has a lot of plugins that can be used to extend its functionality.

### `pep8-naming`

[pep8-naming](https://pypi.org/project/pep8-naming/):material-link: is a plugin for `flake8` that checks your code for naming conventions. It checks for PEP 8 naming conventions and can be used to enforce naming conventions in your code.

### `pyflake`

[pyflakes](https://pypi.org/project/pyflakes/):material-link: is a code linter that checks your code for style violations and errors. It is a lightweight linter that checks your code for common mistakes and potential problems.

### `ruff`

There are many other linters and formatters out there, but most of them can be replaced by [Ruff](https://docs.astral.sh/ruff/):material-link:. It is a fairly new code formatter and linter at the same time. It has recently gained a lot of popularity and is being adapted by many projects. The Ruff formatter can be a drop-in replacement for `black`. It is also much faster than other tools. The Ruff linter supports over 800 linting rules and can replace all the above tools. A complete list of supported rules can be found [here](https://docs.astral.sh/ruff/rules/):material-file-document:. It is recommended that you adopt the Ruff formatter first and then start applying the standard linter rules. You can extend from there if you like, but they already cover the most important things.

#### Basic usage

To run the formatter just run the following command:

```bash
ruff format # apply the formatter to all files
ruff format my_file.py # apply the formatter to a single file
```
And to run the linter:

```bash
ruff check # check all files
ruff check --fix # check and fix all rules which can be fixed automatically
```
Some rules cannot be fixed automatically, so you need to fix them manually. What the problem is and why this might be bad practice is always explained in the rule documentation.

!!! warning

    `ruff format` and `ruff check --fix` will modify your files (potentially quite a lot). So make sure to run it on a clean git branch and commit your changes before running it. You can also use `ruff check` to check your files without modifying them.

#### Configuration
The default rules only cover the most common errors. But you can check many more things by simply adding a configuration file. The two most common ways are to add rules to the `pyproject.toml' file (which you created in the [build step](build.md)) or to create a `ruff.toml` file for your project. The configuration documentation can be found [here](https://docs.astral.sh/ruff/configuration/#__tabbed_1_2):material-file-document:.

With additional rules you can check many things. For example, use [`mcabbe`](https://pypi.org/project/mccabe/):material-link: ([rule `C90`](https://docs.astral.sh/ruff/rules/#mccabe-c90):material-file-document:) to check the cyclomatic complexity of your code. This is a good indicator of the readability of your code. You can also use [`pydocstyle`](https://pypi.org/project/pydocstyle/):material-link: ([rule `D`](https://docs.astral.sh/ruff/rules/#pydocstyle-d):material-file-document:) to check the docstrings of your code. This is a good indicator of the readability of your code, and helps to keep your code clean and maintainable. You can use [`Rule NPY`](https://docs.astral.sh/ruff/rules/#numpy-specific-rules-npy):material-file-document: to update your code to Numpy version 2.0.0. Or use [`pandas-vet`](https://pypi.org/project/pandas-vet/):material-link: ([rule `PD`](https://docs.astral.sh/ruff/rules/#pandas-vet-p):material-file-document:) to check your code for common errors when using pandas.


## pre-commit
Running formatters and linters can be set up in the CI/CD pipeline, and most IDEs have plugins for them too. But to allow developers to run the exact tools that are set up in the project, you can also use git hooks. This is a great way to ensure that your code is always formatted and lined up before you commit it.

Git hooks are a feature of git, and you can set them to run any command before certain triggers. This can be a bit tedious, and you need all the tools installed on your machine. But there are tools like [pre-commit](https://pre-commit.com/):material-link: that make it easy to set up git hooks. It is a framework for managing and maintaining multilingual pre-commit hooks. It is a great way to ensure that your code is always formatted and linked before you commit.

All you need to do is create a `.pre-commit-config.yaml` file in your project. Here is a simple example:

```yaml
# .pre-commit-config.yaml
repos:
# Run ruff to lint and format
- repo: https://github.com/astral-sh/ruff-pre-commit
  # Ruff version.
  rev: v0.9.9
  hooks:
    # Run the linter.
  - id: ruff
    args: [--fix]
    # Run the formatter.
  - id: ruff-format
```

Now anyone can clone your project and run `pre-commit install` to install the git hook. pre-commit will automatically run the Ruff linter and formatter before each commit. It will also manage any dependencies, so you do not need to have ruff installed on your machine. You can also run `pre-commit run --all-files` to run the linter and formatter on all files in your project. 

There are many [other hooks](https://pre-commit.com/hooks.html):material-file-document: available. The [ci section](ci.md) also shows how to run pre-commit in the CI/CD pipeline.

## Resources
- [How to Write Beautiful Python Code With PEP 8 – Real Python](https://realpython.com/python-pep8/)
- [life4/awesome-python-code-formatters: A curated list of awesome Python code formatters](https://github.com/life4/awesome-python-code-formatters)
- [Python code formatters • DeepSource](https://deepsource.com/blog/python-code-formatters)
- [Ruff Documentation](https://docs.astral.sh/ruff/)
- [Comparing Python Linting Tools](https://realpython.com/python-code-quality/)
