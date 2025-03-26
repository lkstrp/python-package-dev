# 3. Test

> “Never allow the same bug to bite you twice.” - Steve Maguire

Testing is an important part of software development, and it can be a big deal. But it doesn't have to be. You want to make sure that your code works as expected and that you don't introduce bugs into your production code. There are many different testing strategies and tools available. This section will give you a brief overview of the most common ones and how to use them.

!!! example

    See demo project:
    
    :material-ray-start: [Before](https://github.com/lkstrp/python-package-demo/commit/2b316bbf257d398ec948d4d7a95202b3b41ae49c) | :material-ray-end: [After](https://github.com/lkstrp/python-package-demo/commit/c5bc66b7c5acee9b01f3285cf28960c500474dc2) | :octicons-diff-16: [Diff](https://github.com/lkstrp/python-package-demo/compare/2b316bbf257d398ec948d4d7a95202b3b41ae49c...c5bc66b7c5acee9b01f3285cf28960c500474dc2)

## Testing strategies
There are many different testing strategies. And different projects will require different strategies. But the most common ones you will probably want to use are unit tests and integration tests.

Tests live in `test/`:
```
python-package-demo/
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

### Unit tests
Unit tests are the most common type of tests. They test a single unit of code in isolation. This means that you test a single function or class and make sure that it works as expected. Unit tests are usually quick and easy to write. In most projects, they will make up the majority of your tests.

Let's say we wanna test a simple unit of code:
```python
# src/python_package_demo/example.py
def add(a: float, b: float) -> float:
    return a + b
```
We can write a unit test for this function
```python
# test/test_example.py
import pytest
from python_package_demo.example import add
def test_add():
    assert add(1, 2) == 3
    assert add(1.5, 2.5) == 4.0
    assert add(-1, 1) == 0
```
and run it with `pytest`:
```bash
$ pytest
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-8.x.y, pluggy-1.x.y
rootdir: /home/sweet/project
collected 1 item
test/test_example.py .                                             [100%]
============================ 1 passed in 0.12s =============================
```

### Integration tests
Integration testing tests the interaction between different units of code. This means that you test how different functions or classes work together. Integration tests tend to be slower and more complex than unit tests. They are used to make sure that the different parts of your code work together as expected.

!!! info

    In the context of modelling, creating an integration test could be as simple as testing your model framework by running a test configuration of your model.

### Static code anaylsis
Static code analysis tools check code for known "smells" and problems using a database of previously identified problematic code patterns. Tools often provide a suggestion on how to improve the code. Compared to unit tests, static code analysis can be a bit slower, but can also provide insights to the coders and help them improve. There are several static code analysis frameworks available, e.g. [prospector](https://prospector.landscape.io/)

To run a static code analysis install prospector
```bash
pip install prospector
```

and then run it from the project's base folder
```bash
prospector
```

Static code analysis are also easily included in pre-commit setups.

To run prospector before pushing use
```yaml
repos:
  - repo: local
    hooks:
      - id: prospector
        name: prospector
        entry: prospector
        stages: [ push ]
        language: python
        python: "3.9"
        pass_filenames: false
        always_run: true
        additional_dependencies:
          - prospector
```

### Other tests
There are many other types of tests, but you will probably be good at unit and integration testing. 

### Test driven development
In addition, there is the concept of Test Driven Development (TDD). Here you write your tests before you write your code. This means that you first write a test that fails, and then write the minimum amount of code to pass that test. The advantages of TDD are instant feedback and potentially better design of your code, as you are forced to think about the interface of your code before you write it and see where it goes. This may be overkill for small projects, but in the end it is a matter of style and worth a try.

## Tools

There are many different tools in Python that make it easy to test your code.

### `pytest`

[`pytest`](https://docs.pytest.org/en/stable/):material-link: is a testing framework that makes it easy to write simple and scalable test cases. It is the most common testing framework in Python and is used by many projects. It has a lot of features and plugins that make it easy to use.

A [quick example](https://docs.pytest.org/en/stable/#a-quick-example):material-file-document::
```python
# content of test_sample.py
def inc(x):
    return x + 1


def test_answer():
    assert inc(3) == 5
```
To execute it:
```bash
$ pytest
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-8.x.y, pluggy-1.x.y
rootdir: /home/sweet/project
collected 1 item

test_sample.py F                                                     [100%]

================================= FAILURES =================================
_______________________________ test_answer ________________________________

    def test_answer():
>       assert inc(3) == 5
E       assert 4 == 5
E        +  where 4 = inc(3)

test_sample.py:6: AssertionError
========================= short test summary info ==========================
FAILED test_sample.py::test_answer - assert 4 == 5
============================ 1 failed in 0.12s =============================
```

Check the [pytest documentation](https://docs.pytest.org/en/stable/getting-started.html#get-started):material-file-document: for more informations.

### `unittest`

[`unittest`](https://docs.python.org/3/library/unittest.html):material-link: is the built-in testing framework in Python. It is a bit more complex than `pytest`, but it is also very powerful. It is used by many projects and is a good choice if you want to use the built-in tools.

A [Basic example](https://docs.python.org/3/library/unittest.html#basic-example):material-file-document::
```python
import unittest

class TestStringMethods(unittest.TestCase):

    def test_upper(self):
        self.assertEqual('foo'.upper(), 'FOO')

    def test_isupper(self):
        self.assertTrue('FOO'.isupper())
        self.assertFalse('Foo'.isupper())

    def test_split(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        # check that s.split fails when the separator is not a string
        with self.assertRaises(TypeError):
            s.split(2)

if __name__ == '__main__':
    unittest.main()
```

Check the [unittest documentation](https://docs.python.org/3/library/unittest.html):material-file-document: for more informations.

### `coverage`
When you add tests to your project, it is hard to know if you are testing everything, or which parts are missing.

[`coverage`](https://coverage.readthedocs.io/en/7.2.0/):material-link: is a tool for measuring code coverage in Python programs. It monitors which parts of your code are executed by `pytest` or `unittest` and generates a report showing which parts of your code are not covered by tests. This can help you identify areas of your code that need more testing.

An example report looks like this:
```bash
$ coverage report -m
Name                      Stmts   Miss  Cover   Missing
-------------------------------------------------------
my_program.py                20      4    80%   33-35, 39
my_other_module.py           56      6    89%   17-23
-------------------------------------------------------
TOTAL                        76     10    87%
```

Check the [coverage documentation](https://coverage.readthedocs.io/en/7.2.0/):material-file-document: for more informations.

#### Codecov

There are services like [Codecov](https://codecov.io/):material-link: or [Coveralls](https://coveralls.io/):material-link: that can help you visualise your coverage reports. They provide a web interface to see which parts of your code are covered by tests and which are not. This can also be integrated into your CI/CD pipeline to automatically generate coverage reports for each commit or pull request. CI/CD is discussed in the [CI section](ci.md).

### `doctest`
[`doctest`](https://docs.python.org/3/library/doctest.html):material-link: is a module that allows you to test your code by running examples embedded in the documentation. This means you can kill two birds with one stone: you can write documentation and tests at the same time.

A [simple example](https://docs.python.org/3/library/doctest.html#simple-usage-checking-examples-in-a-text-file):material-file-document::
`example.txt`:
```txt
The ``example`` module
======================

Using ``factorial``
-------------------

This is an example text file in reStructuredText format.  First import
``factorial`` from the ``example`` module:

    >>> from example import factorial

Now use it:

    >>> factorial(6)
    120
```
Doctest will search for the `>>>` prompt in any text files (also docstrings) and execute the code.
```bash
$ python -m doctest example.txt
File "./example.txt", line 14, in example.txt
Failed example:
    factorial(6)
Expected:
    120
Got:
    720
```
So you know your documentation is up to date, you provide valuable examples, and you can test your code without writing tests.

Check the [doctest documentation](https://docs.python.org/3/library/doctest.html):material-file-document: for more informations.

### `hypothesis`
[`hypothesis`](https://hypothesis.readthedocs.io/en/latest/):material-link: is another, more flexible way of writing unit tests. Instead of providing actual test data, you provide data specifications, and `hypothesis` will test those specifications. This is a way of catching edge cases that a normal unit test might not catch, but would still be covered by a unit test.

[An example](https://hypothesis.readthedocs.io/en/latest/quickstart.html#an-example):material-file-document::
Let's say you have to functions `encode` and `decode` that encode and decode a string. With `hypothesis` you can test that via:
```python
from hypothesis import given
from hypothesis.strategies import text


@given(text())
def test_decode_inverts_encode(s):
    assert decode(encode(s)) == s
```

Check the [hypothesis documentation](https://hypothesis.readthedocs.io):material-file-document: for more informations.

## Resources
- [Getting Started With Testing in Python – Real Python](https://realpython.com/python-testing/)
- [Testing Your Code — The Hitchhiker's Guide to Python](https://docs.python-guide.org/writing/tests/)
- [The different types of testing in software | Atlassian](https://www.atlassian.com/continuous-delivery/software-testing/types-of-software-testing)
- [Python's doctest: Document and Test Your Code at Once – Real Python](https://realpython.com/python-doctest/)
- [Property-Based Testing With Python](https://hypothesis.works/articles/what-is-property-based-testing/)
- [Hypothesis for Property-Based Testing](https://hypothesis.readthedocs.io/en/latest/)
- [Pytest Documentation](https://docs.pytest.org/en/stable/)
- [Coverage.py Documentation](https://coverage.readthedocs.io/en/latest/)
- [Python unittest Documentation](https://docs.python.org/3/library/unittest.html)
- [pytest-cov: Coverage plugin for pytest](https://pytest-cov.readthedocs.io/en/latest/)
