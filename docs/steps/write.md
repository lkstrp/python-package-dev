---
weight: 1
---

# 1. Write

If you are reading this, it can be assumed that you are already familiar with Python. Therefore, this section will not cover the basics, but just point you to some resources.

!!! example

    See demo project:
    
    :material-ray-end: [After](https://github.com/lkstrp/python-package-demo/commit/ff576bf0d8b3ec3cdff19477e2c12c7cc72c834b)

## Version control
If you develop software, you will want to use git. The rest of this guide assumes and requires that you are using git. If not, go and [learn git](https://git-scm.com/docs/gittutorial):material-file-document: now.

If you think you know git, you might want to check out some **advanced git** tips in this [guide](https://www.atlassian.com/git/tutorials/advanced-overview):material-file-document:. The difference between merging and rebasing is a common source of confusion, git hooks are used later, and git worktrees are a common "why didn't I know this before" moment.

## IDE
Most of the tools discussed in the following sections can be used via CLI, but also provide IDE plugins. Popular IDEs are [VSCode](https://code.visualstudio.com/):material-link: and [PyCharm](https://www.jetbrains.com/pycharm/):material-link:. Both support plugins for linters, formatters, type checkers etc. That is quite useful since you get immediate feedback whith inline errors and warnings.

## Coding Assistant
Most of the tools discussed in the following sections can be used via the CLI, but also provide IDE plugins. Popular IDEs are [VSCode](https://code.visualstudio.com/):material-link: and [PyCharm](https://www.jetbrains.com/pycharm/):material-link:. Both support plugins for linters, formatters, type checkers, etc. This is quite useful as you get immediate feedback with inline errors and warnings.

### Privacy compliant ways
Of course, LLMs are a great help, but you may have privacy concerns or simply not be allowed to use them. Using models from OpenAI or Anthropic will send your code to their servers.

Self-hosting has become very easy. There are many [open source models](https://ollama.com/search):material-link: that you can run on your own machine. With [Ollama](https://ollama.com/):material-link: you can run them with a single command, and with [Open WebUI](https://openwebui.com/):material-link: you can use them through a great web interface. [Continue](https://www.continue.dev/):material-link: gives you Copilot-style auto-completion. 

The main obstacle is hardware. You need a decent GPU to run these models. But this is just a hint that these problems are solvable and you should get on the train.

## Resources

**Python**

- [BeginnersGuide - Python Wiki](https://wiki.python.org/moin/BeginnersGuide)
- [Structuring Your Project — The Hitchhiker's Guide to Python](https://docs.python-guide.org/writing/structure/)
- [Python Tutorials – Real Python](https://realpython.com/)
- [Python Tutorial](https://www.w3schools.com/python/)
- [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/):material-link:
- [Python Design Patterns](https://python-patterns.guide/):material-link:

**Git**

- [git - the simple guide - no deep shit!](https://rogerdudler.github.io/git-guide/)
- [Learn Git - Tutorials, Workflows and Commands | Atlassian](https://www.atlassian.com/git)
- [Git Tutorial for Beginners: Learn Git in 1 Hour - YouTube](https://www.youtube.com/watch?v=8JJ101D3knE)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf):material-file-document:

**IDEs & Tools**

- [PyCharm Documentation](https://www.jetbrains.com/help/pycharm/):material-file-document:
- [VS Code Python Tutorial](https://code.visualstudio.com/docs/python/python-tutorial):material-file-document:
- [Jupyter Notebook](https://jupyter.org/):material-link:
