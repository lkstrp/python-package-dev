# 7. Release

Now that you have a well-tested, formatted and documented project, it is time to think about a release strategy.

## Versioning
Versioning is a way of keeping track of changes in your project. It is important to have a clear versioning strategy to communicate changes to your users. There are several different versioning strategies, but the most common are semantic versioning and calendar versioning.

## Semantic Version
[Semantic Versioning](https://semver.org/):material-file-document: is a versioning strategy that uses a three-part version number: MAJOR.MINOR.PATCH. Projects such as Python itself, pandas, numpy and many others use this versioning scheme.

- MAJOR version is incremented when you make incompatible API changes
- MINOR version is incremented when you add functionality in a backwards-compatible manner
- PATCH version is incremented when you make backwards-compatible bug fixes

By following this versioning strategy, you can easily communicate to your users what has changed in your project. It is also recommended that you actually use the scheme. Many projects have the x.x.x versioning scheme, but don't follow the rules. For example, they simply increment the patch version for every release, even if it is a new feature. This is not good practice and can lead to confusion. A patch should never be a new feature.

The disadvantage of this approach is that you have to distinguish between a feature and a bug fix. This is not always easy, especially in larger projects. What if you want to release a new patch, but the patch sits on top of another feature commit that you don't want to release yet? You could do this by creating a release branch, and cherry-picking the patch on that branch to create another release from there. But this is not always easy, and can lead to confusion. Larger projects use different branches for different releases. For smaller projects, this might be too much extra work.

## Calendar Versioning
If it is difficult to actually follow the rules of semantic versioning, you might want to look at [calendar versioning](https://calver.org/):material-file-document:. The advantage of this approach is that the user also gets a sense of how old and maintained the project is. See this [guide](https://calver.org/#when-to-use-calver):material-file-document: for more information on when to use which scheme. But whichever scheme you choose, it is important to be consistent and follow the rules.

## Resources
- [Versioning - Python Packaging User Guide](https://packaging.python.org/en/latest/discussions/versioning/#semantic-versioning-vs-calendar-versioning)
- [Python Package Release Checklist](https://cookiecutter-hypermodern-python.readthedocs.io/en/latest/guide.html#release-checklist)
- [GitHub Release Documentation](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)
- [PyPI Publishing Documentation](https://packaging.python.org/en/latest/tutorials/packaging-projects/#uploading-the-distribution-archives)
- [Conventional Commits Specification](https://www.conventionalcommits.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Twine Documentation](https://twine.readthedocs.io/en/latest/) (for publishing packages)
- [setuptools_scm](https://github.com/pypa/setuptools_scm/) (for managing versions from git tags)