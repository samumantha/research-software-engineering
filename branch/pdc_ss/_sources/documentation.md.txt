# Documentation

:::{objectives}
- Improve the README of your project or our example project.
:::



:::{instructor-note}
- xx min teaching/discussion
- xx min exercise
:::


## Why? &#128151;&#9993;&#65039; to your future self

- You will probably use your code in the future and may forget details.
- You may want others to use your code or contribute
  (almost impossible without documentation).



## In-code documentation

Docstrings can do a bit more than just comments:
- Tools can generate help text automatically from the docstrings.
- Tools can generate documentation pages automatically from code.

It is common to write docstrings for functions, classes, and modules.

Good docstrings describe:
- What the function does
- What goes in (including the type of the input variables)
- What goes out (including the return type)

**Naming is documentation**:
Giving explicit, descriptive names to your code segments (functions, classes,
variables) already provides very useful and important documentation. In
practice you will find that for simple functions it is unnecessary to add a
docstring when the function name and variable names already give enough
information.

## Sometimes version control is better than a comment

````{admonition} Examples for code comments where Git is a better solution
  **Keeping zombie code** "just in case" (rather use version control):
  ```python
  # do not run this code!
  # if temperature > 0:
  #     print("It is warm")
  ```
  Instead: Remove the code, you can always find it back in a previous version of your code in Git.

  **Emulating version control**:
  ```python
  # John Doe: threshold changed from 0 to 15 on August 5, 2013
  if temperature > 15:
      print("It is warm")
  ```
  Instead: You can get this information from `git log` or `git show` or `git
  annotate` or similar.
````

## Always have at least a README!

An ideal README includes:

- **Purpose**
- Requirements
- Installation instructions
- **Copy-paste-able example to get started**
- Tutorials covering key functionality
- Reference documentation (e.g. API) covering all functionality
- Authors and **recommended citation**
- License
- Contribution guide

See also the
[JOSS review checklist](https://joss.readthedocs.io/en/latest/review_checklist.html).


## Demonstration

- If we have time, we will improve the README.md of our example repository:
  <https://github.com/coderefinery/planets>


## What if you need more than a README?

- Write documentation in
  [Markdown (.md)](https://en.wikipedia.org/wiki/Markdown)
  or
  [reStructuredText (.rst)](https://en.wikipedia.org/wiki/ReStructuredText)
  or
  [R Markdown (.Rmd)](https://rmarkdown.rstudio.com/)

- In the **same repository** as the code -> version control and **reproducibility**

- Use one of many tools to build HTML out of md/rst/Rmd:
  [Sphinx](https://sphinx-doc.org),
  [MkDocs](https://www.mkdocs.org/),
  [Zola](https://www.getzola.org/), [Jekyll](https://jekyllrb.com/),
  [Hugo](https://gohugo.io/), RStudio, [knitr](https://yihui.org/knitr/),
  [bookdown](https://bookdown.org/),
  [blogdown](https://bookdown.org/yihui/blogdown/), ...

- Deploy the generated HTML to [GitHub Pages](https://pages.github.com/) or
  [GitLab Pages](https://docs.gitlab.com/ee/user/project/pages/)


## Where to explore more

- [Documentation lesson material](https://coderefinery.github.io/documentation/)
- [Talk material "Documenting code" by S. Wittke](https://github.com/samumantha/documentation_example)
- Inside Sphinx we can add tables, images, equations, code snippets, ... ([more information](https://coderefinery.github.io/documentation/sphinx/#exercise-adding-more-sphinx-content).
- [CodeRefinery mini-workshop](https://coderefinery.github.io/mini-workshop/2/documentation/)


## Exercises

````{exercise} Documentation-1: Comments
  Let's take a look at two example comments (comments in Python start with `#`):

  **Comment A**
  ```python
  # now we check if temperature is below -50
  if temperature < -50:
      print("ERROR: temperature is too low")
  ```

  **Comment B**
  ```python
  # we regard temperatures below -50 degrees as measurement errors
  if temperature < -50:
      print("ERROR: temperature is too low")
  ```
  Which of these comments is more useful? Can you explain why?

  ```{solution} Solution
  - Comment A describes **what** happens in this piece of code. This can be
    useful for somebody who has never seen Python or a program, but for somebody
    who has, it can feel like a redundant commentary.

  - Comment B is probably more useful as it describes **why** this piece of code
    is there, i.e. its **purpose**.
  ```
````

`````{exercise} Documentation-2a: Sphinx

Our goal in this episode is to build HTML pages locally on our computers.

````{prereq} Before we start, let us verify whether we have the software we need

  You may need to activate your CodeRefinery conda environment we set up
  in the installation instructions.  This was covered as part of the
  installation instructions, but the most usual command to do this is:
  ```console
  $ conda activate coderefinery
  ```

  Check whether Python is available
  (you should see a version; precise version is not so important):
  ```console
  $ python --version

  Python 3.11.5
  ```

  Check whether Sphinx is available
  (you should see a version; precise version is not so important):
  ```console
  $ sphinx-build --version

  sphinx-build 5.3.0
  ```

  Check whether the quickstart tool is available
  (you should see a version; precise version is not so important):
  ```console
  $ sphinx-quickstart --version

  sphinx-quickstart 5.3.0
  ```

  Check whether MyST parser is available
  (you should see no output):
  ```console
  $ python -c "import myst_parser"
  ```

If the above commands produce an error
(**command not found** or **module not found** or **ModuleNotFoundError**),
please follow our
[installation instructions](https://coderefinery.github.io/installation/conda-environment/).
But please don't give up if you don't have these - the episodes after this one will work even without these
tools.
````

Create a directory for the example documentation, step into it, and inside
generate the basic documentation template:

```console
$ mkdir doc-example
$ cd doc-example
$ sphinx-quickstart
```

The quickstart utility will ask you some questions. For this exercise, you can go
with the default answers except to specify a project name, author name, and project release:

```
> Separate source and build directories (y/n) [n]: <hit enter>
> Project name: <your project name>
> Author name(s): <your name>
> Project release []: 0.1
> Project language [en]: <hit enter>
```

A couple of files and directories are created:

| File/directory | Contents |
| -------------- | -------- |
| conf.py        | Documentation configuration file |
| index.rst      | Main file in Sphinx |
| _build/        | Directory where docs are built (you can decide the name) |
| _templates/    | Your own HTML templates |
| _static/       | Static files (images, styles, etc.) copied to output directory on build |
| Makefile       | Makefile to build documentation using make |
| make.bat       | Makefile to build documentation using make (Windows) |

`Makefile` and `make.bat` (for Windows) are build scripts that wrap the sphinx commands, but
we will be doing it explicitly.

Let's have a look at the `index.rst` file, which is the main file of your documentation:

```rst
.. myproject documentation master file, created by
   sphinx-quickstart on Sat Sep 23 17:35:26 2023.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to myproject's documentation!
=====================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
```

- We will not use the `Indices and tables` section now, so remove it and everything below.
- The top four lines, starting with `..`, are a comment.
- The next lines are the table of contents. We can add content below:

```rst
.. toctree::
   :maxdepth: 2
   :caption: Contents:

   some-feature.md
```
Note that `some-feature.md` needs to be indented to align with `:caption:`.

We now need to tell Sphinx to use markdown files. To do this, we open
`conf.py` and replace the line:
```python
extensions = []
```

with this line so that Sphinx can parse Markdown files:
```python
extensions = ['myst_parser']
```

Let's create the file `some-feature.md` (in Markdown format) which we have just listed in
`index.rst` (which uses reStructured Text format).

```md
# Some feature

## Subsection

Exciting documentation in here.
Let's make a list (empty surrounding lines required):

- item 1

  - nested item 1
  - nested item 2

- item 2
- item 3
```

We now build the site:

```console
$ ls -1

_static
_templates
conf.py
index.rst
make.bat
Makefile
some-feature.md

$ sphinx-build . _build

... lots of output ...
build succeeded.

The HTML pages are in _build.

$ ls -1 _build

_sources
_static
genindex.html
index.html
objects.inv
search.html
searchindex.js
some-feature.html
```

Now open the file `_build/index.html` in your browser.
- Linux users, type:
  ```console
  $ xdg-open _build/index.html
  ```
- macOS users, type:
  ```console
  $ open _build/index.html
  ```
- Windows users, type:
  ```console
  $ start _build/index.html
  ```
- If the above does not work:
  Enter `file:///home/user/doc-example/_build/index.html` in your browser (adapting the path to your case).

Hopefully you can now see a website. If so, then you are able to build Sphinx pages locally.
This is useful to check how things look before pushing changes to GitHub or elsewhere.

Note that you can change the styling by editing `conf.py` and changing the value `html_theme`
(for instance you can set it to `sphinx_rtd_theme` (if you have that Python package installed)
to have the Read the Docs look).

````` 

`````{exercise} Documentation-2b: Add more content to your example Sphinx documentation

1. Add a entry below `some-feature.md` labeled `another-feature.md` (or a better name) to the `index.rst` file.
2. Create a file `another-feature.md` in the same directory as the `index.rst` file.
3. Add some content to `another-feature.md`, rebuild with `sphinx-build . _build`, and refresh the browser to look at the results.
4. Use the [MyST Typography](https://myst-parser.readthedocs.io/en/latest/syntax/typography.html) page as help.

Experiment with the following Markdown syntax:

- \*Emphasized text\* and \*\*bold text\*\*

- Headings:
```md
# Level 1

## Level 2

### Level 3

#### Level 4
```

- An image: `![alt text](image.png)`

- `[A link](https://www.example.org)`

- Numbered lists (numbers adjusted automatically):
```md
1. item 1
2. item 2
3. item 3
1. item 4
1. item 5
```

- Simple tables:
```md
| No.  |  Prime |
| ---- | ------ |
| 1    |  No    |
| 2    |  Yes   |
| 3    |  Yes   |
| 4    |  No    |
```

- Code blocks:
````markdown
The following is a Python code block:
```python
  def hello():
      print("Hello world")
```

And this is a C code block:
```c
#include <stdio.h>
int main()
{
    printf("Hello, World!");
    return 0;
}
```
````

- You could include an external file (here we assume a file called "example.py"
  exists; at the same time we highlight lines 2 and 3):
````markdown
```{literalinclude} example.py
:language: python
:emphasize-lines: 2-3
```
````

- We can also use Jupyter notebooks (*.ipynb) with Sphinx. It requires the
  [myst-nb](https://myst-nb.readthedocs.io/) extension to be installed.
`````


:::{keypoints}
- Often a README is enough.
- Documentation which is only in the source code is not enough.
- Documentation needs to be kept
  **in the same Git repository** as the code.
  It evolves [with the code](https://github.com/coderefinery/planets/network).
- Sphinx and Markdown is a powerful duo for writing documentation.
:::
