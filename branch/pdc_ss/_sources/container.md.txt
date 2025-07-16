# Container exercises

:::{exercise} Exercise Container-1: Time-travel with containers

Imagine the following situation: A researcher has written and published their research code which
requires a number of libraries and system dependencies. They ran their code
on a Linux computer (Ubuntu). One very nice thing they did was to publish
also a container image with all dependencies included, as well as the
definition file (below) to create the container image.

Now we travel 3 years into the future and want to reuse their work and adapt
it for our data. The container registry where they uploaded the container
image however no longer exists. But luckily (!) we still have the definition
file (below). From this we should be able to create a new container image.

- Can you anticipate problems using the definition file here 3 years after its
  creation? Which possible problems can you point out?
- Discuss possible take-aways for creating more reusable containers.

``````{tabs}
  `````{tab} Python project using virtual environment
    ```{literalinclude} containers/bad-example-python.def
    :language: docker
    :linenos:
    ```

    ````{solution}
    - Line 2: "ubuntu:latest" will mean something different 3 years in future.
    - Lines 11-12: The compiler gcc and the library libgomp1 will have evolved.
    - Line 30: The container uses requirements.txt to build the virtual environment but we don't see
      here what libraries the code depends on.
    - Line 33: Data is copied in from the hard disk of the person who created it. Hopefully we can find the data somewhere.
    - Line 35: The library fancylib has been built outside the container and copied in but we don't see here how it was done.
    - Python version will be different then and hopefully the code still runs then.
    - Singularity/Apptainer will have also evolved by then. Hopefully this definition file then still works.
    - No help text.
    - No contact address to ask more questions about this file.
    - (Can you find more? Please contribute more points.)

    ```{literalinclude} containers/bad-example-python.def
    :language: docker
    :linenos:
    :emphasize-lines: 2, 11-12, 30, 33, 35
    ```
    ````
  `````

  `````{tab} C++ project
    This definition files has potential problems 3 years later. Further down on
    this page we show a better and real version.

    ```{literalinclude} containers/bad-example-cxx.def
    :language: docker
    :linenos:
    ```

    ````{solution}
    - Line 2: "ubuntu:latest" will mean something different 3 years in future.
    - Lines 9: The libraries will have evolved.
    - Line 11: We clone a Git repository recursively and that repository might evolve until we build the container image the next time.
      here what libraries the code depends on.
    - Line 18: The library fancylib has been built outside the container and copied in but we don't see here how it was done.
    - Singularity/Apptainer will have also evolved by then. Hopefully this definition file then still works.
    - No help text.
    - No contact address to ask more questions about this file.
    - (Can you find more? Please contribute more points.)

    ```{literalinclude} containers/bad-example-cxx.def
    :language: docker
    :linenos:
    :emphasize-lines: 2, 9, 11, 18
    ```
    ````
  `````
``````
:::


::::::{exercise} Exercise Container-2: Build a container and run it on a cluster

Here we will try to build a container from
[the definition file](https://github.com/coderefinery/planets/blob/main/container.def)
of our example project.

Requirements:
1. Linux (it is possible to build them on a macOS or Windows computer but it is
   more complicated).
2. An installation of [Apptainer](https://apptainer.org/) (e.g. following the
   [quick installation](https://apptainer.org/docs/user/latest/quick_start.html#quick-installation)).
   Alternatively, [SingularityCE](https://sylabs.io/singularity/) should also
   work.

Now you can build the container image from the container definition file.
Depending on the configuration you might need to run the command with `sudo`
or with `--fakeroot`.

Hopefully one of these four will work:
```console
$ sudo apptainer build container.sif container.def
$ apptainer build --fakeroot container.sif container.def

$ sudo singularity build container.sif container.def
$ singularity build --fakeroot container.sif container.def
```

Once you have the `container.sif`, copy it to a cluster and try to run it
there.

Here are two job script examples:

:::::{tabs}
   ::::{group-tab} Dardel (Sweden)
     ```bash
     #!/usr/bin/env bash

     # the SBATCH directives and the module load below are only relevant for the
     # Dardel cluster and the PDC Summer School; adapt them for your cluster

     #SBATCH --account=edu24.summer
     #SBATCH --job-name='container'
     #SBATCH --time=0-00:05:00

     #SBATCH --partition=shared

     #SBATCH --nodes=1
     #SBATCH --tasks-per-node=1
     #SBATCH --cpus-per-task=16


     module load PDC singularity


     # catch common shell script errors
     set -euf -o pipefail


     echo
     echo "what is the operating system on the host?"
     cat /etc/os-release


     echo
     echo "what is the operating system in the container?"
     singularity exec container.sif cat /etc/os-release


     # 1000 planets, 20 steps
     time ./container.sif 1000 20 ${SLURM_CPUS_PER_TASK} results
     ```
   ::::

   ::::{group-tab} Saga (Norway)
     ```bash
     #!/usr/bin/env bash

     #SBATCH --account=nn9997k
     #SBATCH --job-name='container'
     #SBATCH --time=0-00:05:00

     #SBATCH --mem-per-cpu=1G

     #SBATCH --nodes=1
     #SBATCH --tasks-per-node=1
     #SBATCH --cpus-per-task=16


     # catch common shell script errors
     set -euf -o pipefail


     echo
     echo "what is the operating system on the host?"
     cat /etc/os-release


     echo
     echo "what is the operating system in the container?"
     singularity exec container.sif cat /etc/os-release


     # 1000 planets, 20 steps
     time ./container.sif 1000 20 ${SLURM_CPUS_PER_TASK} results
     ```
   ::::
::::::


:::{exercise} Exercise Container-3: Building a container on GitHub and running it on a cluster

You can build a container on GitHub (using GitHub Actions) or GitLab (using
GitLab CI) and host the image it on GitHub/GitLab.  This has the following
advantages:
- You don't need to host it yourself.
- But the image stays close to its sources and is not on a different service.
- Anybody can inspect the recipe and how it was built.
- Every time you make a change to the recipe, it builds a new image.

If you want to try this out:
- Take [this repository](https://github.com/bast/apptainer-conda)
  as starting point and inspiration.
- Don't focus too much on what this container does, but rather [how it is built](https://github.com/bast/apptainer-conda/tree/main/.github/workflows).
- To build a new version, one needs to send a pull request which updates `VERSION`
  and modifies the definition file (in this case `conda.def`).
:::


:::{exercise} Exercise Reproducibility-5: Building a container on a cluster

This may not be easy and you will probably need help from a TA or the
instructor but is a great exercise and we can try to do this together.

A good starting point is the [Apptainer User
Guide](https://apptainer.org/docs/user/latest/), particularly the documentation
about [definition
files](https://apptainer.org/docs/user/latest/definition_files.html).

A good test is to build the container on one computer and try to run it on
another one.  A big benefit of this exercise is that it will clarify to you
which dependencies your code really has because you have to document them -
there are no shortcuts.
:::