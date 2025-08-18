# Reproducible dependencies, environments and workflows

:::{objectives}
- Understand that there are tools to help with dependency management
- Understand the difference between a **script** and a **workflow**.
- Understand the pros and cons of "simple" scripts.
:::

:::{instructor-note}
- 20 min teaching/discussion
:::


## It all starts with a good directory structure ...

```
project_name/
├── README.md             # overview of the project
├── data/                 # data files used in the project
│   ├── README.md         # describes where data came from
│   └── sub-folder/       # may contain subdirectories
├── processed_data/       # intermediate files from the analysis
├── manuscript/           # manuscript describing the results
├── results/              # results of the analysis (data, tables, figures)
├── src/                  # contains all code in the project
│   ├── LICENSE           # license for your code
│   ├── requirements.txt  # software requirements and dependencies
│   └── ...
└── doc/                  # documentation for your project
    ├── index.rst
    └── ...
```

## How to avoid: "It works on my machine &#129335;"

There are not many codes that have no dependencies.

Use a **standard way** to list dependencies in your project:
- Python: `requirements.txt` or `environment.yml`
- R: `DESCRIPTION` or `renv.lock`
- Rust: `Cargo.lock`
- Julia: `Project.toml`
- C/C++/Fortran: `CMakeLists.txt` or `Makefile` or `spack.yaml` or the module
  system on clusters or containers
- Other languages: ...

Install dependencies into **isolated environments**:
- For each project, create a new environment, including all its dependencies.
- Collect them **in a file** which documents them at the same time.

### Where to explore more

- [Reproducible research](https://coderefinery.github.io/reproducible-research/)
- [The Turing Way: Guide for Reproducible Research](https://the-turing-way.netlify.app/reproducible-research/reproducible-research.html)
- [Ten simple rules for writing Dockerfiles for reproducible data science](https://doi.org/10.1371/journal.pcbi.1008316)
- [Computing environment reproducibility](https://doi.org/10.5281/zenodo.8089471)

## Dependencies for compiled languages

Programs written in compiled languages such as C, C++, and Fortran
often depend on one or more libraries. For instance, scientific codes for
numerical simulations often make use of libraries for linear algebra,
random number generation, and fast Fourier transforms.
- Some of the common libraries are established for long and have a very stable application programming interface.
- Other libraries are more specific and developed and used only for a single or few applications.
- The common libraries are often preinstalled or available via package managers. For a compute cluster,
installations of libraries are often maintained by staff.

The dependencies of an application code on libraries can be specified
- in a makefile or makefile.include,
- in CMakeLists.txt for use with the CMake build system,
- in easyconfig configuration files for the EasyBuild system or spack.yaml for the Spack build system.

A particular situation is when building and running code on large clusters. With a large pool
of users comes large number of programs and libraries. This needs to be managed to avoid interference.
A common solution is to use so called module systems to selectively activate/deactive the
programs and libraries that are of relevance. Updates of libraries need to be made with caution
as they potentially can break the functionality of large number of programs.

### Where to explore more

- [Build systems course (2024)](https://github.com/PDC-support/build-systems-course)
- [CMake build system](https://cmake.org/)
- [EasyBuild build and installation framework](https://easybuild.io/)
- [Spack package manager](https://spack.io/)


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
         - git+https://github.com/someuser/someproject.git@main
         - git+https://github.com/anotheruser/anotherproject.git@main
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

### Additional optional exercise

::::{exercise} Exercise Additional-repro: You find a useful code online...

Situation: You found a tool on GitHub that you would like to use: <https://github.com/coderefinery/planets>

Have a look at the environment.yml. What problems could appear when installing the packages a year from now?


:::{solution}
Except for Python version, no version numbers are defined, this means a tool like conda will get you the latest available version. This may not be an issue, and in many cases even good to get the latest versions of everything, but it might mean that you have to spend some time debugging in case function names (or their arguments) changed between versions. Dependencies can also have dependencies, which can be incompatible with others (package A needs package X with version 2, while package B needs package X with version 3) -> Tools like conda/pip will find compatible versions for you.
:::
::::

## Automation and reproducible workflows

### What if we need to run many similar calculations?

With our example, it all started relatively simple:
```bash
python generate-data.py --num-planets 100 --output-file initial.csv

python simulate.py --num-steps 50 \
                   --input-file initial.csv \
                   --output-file final.csv \
                   --trajectories-file trajectories.npz

python animate.py --initial-file initial.csv \
                  --trajectories-file trajectories.npz \
                  --output-file animation.mp4
```

But now we want to run this for **different numbers of planets**:
10, 20, 30, 40, ...

One possible solution:
```{code-block} bash
---
emphasize-lines: 3,4,7,12
---
#!/usr/bin/env bash

for num_planets in 10 20 30 40 50; do
    python generate-data.py --num-planets ${num_planets} \
                            --output-file initial.csv

    python simulate.py --num-steps 50 \
                       --input-file initial.csv \
                       --output-file final.csv \
                       --trajectories-file trajectories.npz

    python animate.py --initial-file initial.csv \
                      --trajectories-file trajectories.npz \
                      --output-file animation-${num_planets}.mp4
done
```


## Discussion

:::{discussion} How would you solve this problem?
Can you list some alternatives to the solution presented above
(for-loop inside a shell script)?
:::

::::{discussion} What are the pros and cons of the solution presented above?
- Consider the case where a step can take hours.
- Imagine needing to run hundreds of calculations.
- Consider the case where a step/calculation can fail.
- Consider the case where you might
  find a mistake in one of the Python scripts.

:::{solution}
What is good about the shell script solution:
- It documents what steps are taken.
- Relatively simple.
- Good solution if you only need to run this once or if it takes no time.

Problems:
- Independent calculations are run one after the other.
- If some of them fail, you need to re-run everything or start commenting out
  lines.
- If you find a mistake in one of the scripts, we again need to re-run
  everything or start commenting out lines.
- What if separate steps need different resources (e.g., memory, CPU)
  or environments (e.g., different Python versions)?

In this situation we need a **workflow/pipeline** management tool.
:::
::::


## Where to explore more

- [Snakemake](https://snakemake.github.io/), a framework for reproducible data analysis
- [Nextflow](https://www.nextflow.io/), a framework for reproducible scientific workflows
- There are many more workflow/pipeline
  tools and frameworks. **Do not invent your own!**


:::{keypoints}
- Ideally you can answer the question about dependencies of your code **with one file**.
- Workflow tools can support reproducibility of running multiple scripts, with multiple files and/or input variables.
:::

