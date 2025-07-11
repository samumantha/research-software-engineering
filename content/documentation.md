# Documentation

:::{objectives}
- Improve the README of your project or our example project.
:::

:::{keypoints}
- Often a README is enough.
- Documentation which is only in the source code is not enough.
- Documentation needs to be kept
  **in the same Git repository** as the code.
  It evolves [with the code](https://github.com/coderefinery/planets/network).
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



