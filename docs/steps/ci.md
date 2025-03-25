# 9. CI/CD

Many of the tools and processes discussed here can be used in a CI/CD pipeline. This is a great way to automate your workflow and ensure that your code is always up-to-date, compliant and tested. Without any manual work.

!!! example

    See demo project:
    
    :material-ray-start: [Before](https://github.com/lkstrp/python-package-demo/commit/abc44f95c0d5573661e6e90884ea7a4823eb2448) | :material-ray-end: [After](https://github.com/lkstrp/python-package-demo/commit/0d5d70aadde2783643ca37f700fd4b8f877016b4) | :octicons-diff-16: [Diff](https://github.com/lkstrp/python-package-demo/compare/abc44f95c0d5573661e6e90884ea7a4823eb2448...0d5d70aadde2783643ca37f700fd4b8f877016b4)


## CI/CD platforms

There are many CI/CD tools out there. Git hosting platforms have their own CI/CD tools built in. But there are also external tools that can be used.

- [GitHub Actions](https://github.com/features/actions)
- [GitLab CI/CD](https://docs.gitlab.com/ee/ci/)
- [Travis CI](https://travis-ci.org/)

You can basically do anything with these services. But in addition there are some external web hooks that can also be triggered by git events, without having to set them up through the platform. Some of these have already been mentioned in the previous sections:

- [pre-commit.ci](https://pre-commit.ci/)
- [ReadTheDocs](https://readthedocs.org/)

## CI/CD jobs
This section will discuss how to set up each job on GitHub Actions, as this is the largest platform for open source projects. But it would work similarly for any other platform. Have a look at the [quick start](https://docs.github.com/en/actions/writing-workflows/quickstart):material-file-document: of GitHub Actions. Some tools are also external webhooks and don't rely on a specific platform. But they could also be set up as a separate jobs.

!!! note

        GitHub Actions refer to the entire CI/CD platform. A GitHub Action is a single action that someone (sometimes GitHub itself) has developed for you to use. Similar to a software package, but for actions.

### Publish package
Publishing a package most likely means publishing to PyPI. As discussed in the [publish section](publish.md), you can rely on the PyPI trigger to publish to conda forge. You'll also want to publish a small release note to your git hosting platform.

#### Trigger
All publishing steps are usually triggered by a git tag. If you set up [`setuptools_scm`](https://setuptools-scm.readthedocs.io/en/latest/) you don't even have to do anything in your project. Just create a tag and push it to your git hosting platform. This will trigger the publish job.

```bash
# Push your commit (if needed)
git push upstream main
# Add a tag to your commit
git tag v0.1.0
# Push the tag to your git hosting platform
git push upstream v0.1.0
```

and in your GitHub action setup:

```yaml
name: Release

on:
  push:
    tags:
    - v*.*.*
# ...
```


#### GitHub release
To create a GitHub release, you could use a GitHub action like [softprops/action-gh-release](https://github.com/softprops/action-gh-release).

```yaml
# ...
  release:
    name: Create GitHub release
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - uses: softprops/action-gh-release@v2
      with:
        body: |
          Some custom text: Thanks to all contributors or link to your release notes.
        append_body: true
        generate_release_notes: true
```

See the [documentation]([softprops/action-gh-release](https://github.com/softprops/action-gh-release) of the workflow for all configurations.

#### PyPI
PyPI provides [documentation](https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/) on how to use GitHub actions to publish to PyPI. There is also an [action](https://github.com/marketplace/actions/pypi-publish) to make it even easier. You should use authentication via a [Trusted Publisher](https://docs.pypi.org/trusted-publishers/).


### Tests
To ensure that all tests pass, you can run them on every commit and pull request in your CI/CD pipeline. A pull request would not be merged if the tests failed. You can also run these tests on a schedule, e.g. every morning, to ensure that any dependencies do not break your code. It's also useful to run your tests on multiple platforms and Python versions if you want to support more than one.

#### Trigger
The configuration below will be triggered for every commit to the main branch and every pull request. It will also run on a schedule every day at 5am UTC. If a new commit is pushed before the tests are finished, you can cancel the tests in progress to save time and resources.

```yaml
name: Test

on:
  push:
    branches:
    - master
  pull_request:
    branches: ['*']
  schedule:
  - cron: "0 5 * * *"

# Cancel any in-progress runs when a new run is triggered
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

#### Test matrix
To run multiple tests on different platforms and Python versions, you can use a test matrix. See the [documentation](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) for more details. You don't have to support all Python versions, but declare which versions are supported in your `pyproject.toml`. Python versions get 2 years of support and 5 years of security updates. You should not use an older version. Check the [endoflife.date](https://endoflife.date/python) of Python for more details.

```yaml
# ...
jobs:
  test:
    # Test package build in matrix of OS and Python versions
    name: Test package
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        python-version:
        - 3.9
        - 3.10
        - 3.11
        - 3.12
        - 3.13
        os:
        - ubuntu-latest
        - macos-latest
        - windows-latest
```


#### Test steps
The only thing you need to set up now is the same process you would run locally. Check out the code, set up the environment, install your package and run the tests. The process might look like this:

```yaml
# ...
  test:
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}
    - name: Install package and dependencies
      run: |
        pip install -e .
    - name: Run tests
      run: |
        pip install pytest
        pytest .
```


### Linting and Formatting
There are many different solutions. But if you have a pre-commit set up, as discussed in the [formatting section](format.md), the easiest way is to simply use [pre-commot.ci](https://pre-commit.ci/) as an external hook to do the pre-commit. Check the documentation for details. Especially for open source projects, this is very easy. Then run the exact same rules locally and in the CI/CD pipeline. 

Otherwise, if your project is private or you cannot use pre-commit.ci for some other reason, you would need to create a custom GitHub action that checks out your code and runs the pre-commit. If the pre-commit fails, the job will fail. At best, it would also commit all changes back to the branch (this is what pre-commit.ci does). The first part is fairly easy to set up, the second part can be a bit more complicated because of authentication issues from within your job. But it can be done.



### Dependency updates
Dependency updates are a great way to keep your code up to date. 

#### Dependabot
[Dependabot](https://github.com/dependabot/dependabot-core) is a GitHub tool that automatically creates pull requests to update your GitHub Actions dependencies. It is very easy to [setup](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference).

#### pre-commit.ci
[pre-commit.ci](https://pre-commit.ci/) can be used not only to perform the pre-commit on your project, but also to create pull requests to update your pre-commit dependencies. It can be set up in the same way as the pre-commit hook.

#### Renovate
[Renovate](https://renovatebot.com/) is another more feature-rich tool for updating your dependencies. It is not necessary for a classic Python package, but it is worth mentioning. You can either host it yourself or rely on their cloud service.


### Code analysis
Code analysis tools can check your code for security vulnerabilities and other problems.

#### CodeQL
[CodeQL (https://codeql.github.com/) is GitHub's code analysis tool. If you already host your project on GitHub, you can add it to your project with a single click. It will then run on every commit and pull request. It is also free for public repositories. See the [documentation](https://docs.github.com/en/code-security/code-scanning/enabling-code-scanning/configuring-default-setup-for-code-scanning) to learn how to set up CodeQL for your project.

# Resources
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitLab CI/CD Documentation](https://docs.gitlab.com/ee/ci/)
- [CircleCI Documentation](https://circleci.com/docs/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [Codecov Documentation](https://docs.codecov.com/) (for code coverage reporting)
- [SonarQube Documentation](https://docs.sonarqube.org/) (for code quality analysis)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
- [Dependabot Documentation](https://docs.github.com/en/code-security/dependabot)