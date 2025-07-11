# Reproducible dependencies and environments

:::{objectives}
There are not many codes that have no dependencies.
How should we **deal with dependencies**?
:::


## How to avoid: "It works on my machine &#129335;"

Use a **standard way** to list dependencies in your project:
- Python: `requirements.txt` or `environment.yml`
- R: `DESCRIPTION` or `renv.lock`
- Rust: `Cargo.lock`
- Julia: `Project.toml`
- C/C++/Fortran: `CMakeLists.txt` or `Makefile` or `spack.yaml` or the module
  system on clusters or containers
- Other languages: ...

Install dependencies into **isolated environments**:
- For each project, create a new environment.
- Don't install dependencies globally for all projects.
- Install them **from a file** which documents them at the same time.

:::{keypoints}
If somebody asks you what dependencies you have in your project, you should be
able to answer this question **with a file**.
:::


## Demonstration

1. The dependencies in our [example
   project](https://github.com/coderefinery/planets) are listed in a
   [environment.yml](https://github.com/coderefinery/planets/blob/main/environment.yml)
   file.

   :::{discussion}
   - Shouldn't the dependencies in the
     [environment.yml](https://github.com/coderefinery/planets/blob/main/environment.yml)
     file be pinned to specific versions?
   - When is a good time to pin them?
   :::

2. We also have a [container definition
   file](https://github.com/coderefinery/planets/blob/main/container.def):
   - This can be used with [Apptainer](https://apptainer.org/)/
     [SingularityCE](https://sylabs.io/singularity/).
   - A container is like an **operating system inside a file**.
   - If we have the time, we can try Exercise Reproducibility-3 below.
   - This is a simple example. It is possible but trickier to
     do MPI and/or GPU computing using containers.


## Where to explore more

- [Reproducible research](https://coderefinery.github.io/reproducible-research/)
- [The Turing Way: Guide for Reproducible Research](https://the-turing-way.netlify.app/reproducible-research/reproducible-research.html)
- [Ten simple rules for writing Dockerfiles for reproducible data science](https://doi.org/10.1371/journal.pcbi.1008316)
- [Computing environment reproducibility](https://doi.org/10.5281/zenodo.8089471)


## Exercises

:::{exercise} Exercise Reproducibility-1: Time-capsule of dependencies

Imagine the following situation: Five students (A, B, C, D, E) wrote a code
that depends on a couple of libraries.  They uploaded their projects to GitHub.
We now travel **3 years into the future** and find their GitHub repositories and
try to re-run their code before adapting it.

- Which version do you expect to be easiest to re-run? Why?
- What problems do you anticipate in each solution?

`````{tabs}
   ````{group-tab} Conda
     **A**:
     You find a couple of library imports across the code but that's it.

     **B**:
     The README file lists which libraries were used but does not mention
     any versions.

     **C**:
     You find a `environment.yml` file with:
     ```
     name: student-project
     channels:
       - conda-forge
     dependencies:
       - scipy
       - numpy
       - sympy
       - click
       - python
       - pip
       - pip:
         - git+https://github.com/someuser/someproject.git@master
         - git+https://github.com/anotheruser/anotherproject.git@master
     ```

     **D**:
     You find a `environment.yml` file with:
     ```
     name: student-project
     channels:
       - conda-forge
     dependencies:
       - scipy=1.3.1
       - numpy=1.16.4
       - sympy=1.4
       - click=7.0
       - python=3.8
       - pip
       - pip:
         - git+https://github.com/someuser/someproject.git@d7b2c7e
         - git+https://github.com/anotheruser/anotherproject.git@sometag
     ```

     **E**:
     You find a `environment.yml` file with:
     ```
     name: student-project
     channels:
       - conda-forge
     dependencies:
       - scipy=1.3.1
       - numpy=1.16.4
       - sympy=1.4
       - click=7.0
       - python=3.8
       - someproject=1.2.3
       - anotherproject=2.3.4
     ```
   ````

   ````{group-tab} Python virtualenv
     **A**:
     You find a couple of library imports across the code but that's it.

     **B**:
     The README file lists which libraries were used but does not mention
     any versions.

     **C**:
     You find a `requirements.txt` file with:
     ```
     scipy
     numpy
     sympy
     click
     python
     git+https://github.com/someuser/someproject.git@master
     git+https://github.com/anotheruser/anotherproject.git@master
     ```

     **D**:
     You find a `requirements.txt` file with:
     ```
     scipy==1.3.1
     numpy==1.16.4
     sympy==1.4
     click==7.0
     python==3.8
     git+https://github.com/someuser/someproject.git@d7b2c7e
     git+https://github.com/anotheruser/anotherproject.git@sometag
     ```

     **E**:
     You find a `requirements.txt` file with:
     ```
     scipy==1.3.1
     numpy==1.16.4
     sympy==1.4
     click==7.0
     python==3.8
     someproject==1.2.3
     anotherproject==2.3.4
     ```
   ````

   ````{group-tab} R
     **A**:
     You find a couple of `library()` or `require()` calls across the code but that's it.

     **B**:
     The README file lists which libraries were used but does not mention
     any versions.

     **C**:
     You find a [DESCRIPTION file](https://r-pkgs.org/description.html) which contains:
     ```
     Imports:
         dplyr,
         tidyr
     ```
     In addition you find these:
     ```r
     remotes::install_github("someuser/someproject@master")
     remotes::install_github("anotheruser/anotherproject@master")
     ```

     **D**:
     You find a [DESCRIPTION file](https://r-pkgs.org/description.html) which contains:
     ```
     Imports:
         dplyr (== 1.0.0),
         tidyr (== 1.1.0)
     ```
     In addition you find these:
     ```r
     remotes::install_github("someuser/someproject@d7b2c7e")
     remotes::install_github("anotheruser/anotherproject@sometag")
     ```

     **E**:
     You find a [DESCRIPTION file](https://r-pkgs.org/description.html) which contains:
     ```
     Imports:
         dplyr (== 1.0.0),
         tidyr (== 1.1.0),
         someproject (== 1.2.3),
         anotherproject (== 2.3.4)
     ```
   ````
`````

```{solution}
**A**: It will be tedious to collect the dependencies one by one. And after
the tedious process you will still not know which versions they have used.

**B**: If there is no standard file to look for and look at and it might
become very difficult for to create the software environment required to
run the software. But at least we know the list of libraries. But we don't
know the versions.

**C**: Having a standard file listing dependencies is definitely better
than nothing. However, if the versions are not specified, you or someone
else might run into problems with dependencies, deprecated features,
changes in package APIs, etc.

**D** and **E**: In both these cases exact versions of all dependencies are
specified and one can recreate the software environment required for the
project. One problem with the dependencies that come from GitHub is that
they might have disappeared (what if their authors deleted these
repositories?).

**E** is slightly preferable because version numbers are easier to understand than Git
commit hashes or Git tags.
```
:::


