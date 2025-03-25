# 5. Type

Python is a dynamically typed language. This means that you don't have to specify the type of a variable when you declare it. While this is one of the advantages of Python - you can write code faster, easier and more flexibly - it can also lead to problems. Because you don't know what type a variable is at runtime, you can run into problems if you try to use a variable in a way that is incompatible with its type. This can lead to bugs that are difficult to find and fix.

[PEP 484 – Type Hints](https://peps.python.org/pep-0484/):material-file-document: introduced type hints to Python, which have been accepted with Python 3.5.

Normal Python code:
``` python
def greeting(name):
    return 'Hello ' + name
```

Staticly typed Python code:
```python
def greeting(name: str) -> str:
    return 'Hello ' + name
```

Python's type hints have no direct effect on the runtime performance of your code. They are designed to be completely optional and ignored during execution. So even if they are typed, you could pass a different type to the function. However, similar to [formatters and linters](format.md), there are static type checkers that will check this for you. It is recommended to use type hints, as they help you to write **correct and self-documenting** code. An existing code base can also be typed step by step.

!!! example

    See demo project:
    
    :material-ray-start: [Before](https://github.com/lkstrp/python-package-demo/commit/51278f4ee1c455b203778e19ed6c26ad9eadfea4) | :material-ray-end: [After](https://github.com/lkstrp/python-package-demo/commit/51b766c7d78c44085950ef487d2f59937492e44c) | :octicons-diff-16: [Diff](https://github.com/lkstrp/python-package-demo/compare/51278f4ee1c455b203778e19ed6c26ad9eadfea4...51b766c7d78c44085950ef487d2f59937492e44c)

## Tools

### Static type checkers

There are different type checkers, but they will all align to PEP 484. However, they are all different and vary in speed. If you don't know where to start, have a look at mypy. If you have a really large code base, you might want to check one of the others.

#### mypy

[mypy](https://mypy-lang.org/):material-link: is the most popular static type checker for Python. It was the first major type checker for Python and is widely used. Also, the default checks are not too strict, so you can start using it in an existing code base without too much effort.

A [simple example](https://mypy.readthedocs.io/en/stable/getting_started.html):material-file-document: of what mypy will check:
```python
# my_file.py
def greeting(name: str) -> str:
    return 'Hello ' + name

greeting(3)
greeting(b'Alice')
greeting("World!")

def bad_greeting(name: str) -> str:
    return 'Hello ' * name
```
To run mypy, you can use the following command:
```bash
$ mypy my_file.py
my_file.py:5: error: Argument 1 to "greeting" has incompatible type "int"; expected "str"  [arg-type]
my_file.py:6: error: Argument 1 to "greeting" has incompatible type "bytes"; expected "str"  [arg-type]
my_file.py:10: error: Unsupported operand types for * ("str" and "str")  [operator]
Found 3 errors in 1 file (checked 1 source file)
```

Check the [mypy documentation](https://mypy.readthedocs.io/en/stable/):material-file-document: for more informations.

#### pyright

[pyright](https://github.com/microsoft/pyright):material-link: is a static type checker for Python created by Microsoft. It is written in TypeScript and runs on Node.js. It is designed to be fast and efficient and might run faster on large code bases than mypy. And since it is developed by Microsoft, it has good support for Visual Studio Code.

Check the [pyright documentation](https://microsoft.github.io/pyright/):material-file-document: for more informations.

#### pytype
[pytype](https://github.com/google/pytype):material-link: is a static type checker for Python created by Google. 

Check the [pytype documentation](https://google.github.io/pytype/):material-file-document: for more informations.

#### pyre
[pyre](https://pyre-check.org/):material-link: is a static type checker for Python created by Facebook.

Check the [pyre documentation](https://pyre-check.org/docs/getting-started/):material-file-document: for more informations

### Runtime type checkers
There are also runtime type checkers that check the types of variables at runtime. These libraries can also do normal data validation. This is a more advanced topic and does not just sit on top of PEP 484. But for the sake of completeness, there are a few libraries that might be worth looking at.

#### pydantic
[pydantic](https://pydantic-docs.helpmanual.io/):material-link: is a data validation and settings management library for Python. It uses Python type hints to validate the data at runtime. It is widely used in the FastAPI framework.

Check the [pydantic documentation](https://pydantic-docs.helpmanual.io/):material-file-document: s.

#### attrs
[attrs](https://www.attrs.org/en/stable/):material-link: is a library for creating classes without writing boilerplate code. It is similar to dataclasses, but has more features and is more flexible. It also uses Python type hints to validate the data at runtime.

Check the [attrs documentation](https://www.attrs.org/en/stable/):material-file-document: for more informations.

## Resources
- [PEP 484 – Type Hints](https://peps.python.org/pep-0484/)
- [typing — Support for type hints — Python documentation](https://docs.python.org/3/library/typing.html)
- [Python Type Checking (Guide) – Real Python](https://realpython.com/python-type-checking/)
- [Python Type Checking | TestDriven.io](https://testdriven.io/blog/python-type-checking/)
- [mypy documentation](https://mypy.readthedocs.io/en/stable/)
- [Pydantic: Data Validation and Settings Management](https://docs.pydantic.dev/)