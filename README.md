# Parallel Panel Burner Setup

Efforts modelling the gas burner setup of a [parallel panel test](https://github.com/MaCFP/macfp-db/tree/master/Fire_Growth/NIST_Parallel_Panel), without sample. Goal is to get the burner itself simulated well to reduce the number of unknowns when attempting fire spread simulation.

This project uses experimental data from the [MaCFP group](https://github.com/MaCFP). Simulations are conducted using the [Fire Dynamics Simulator (FDS)](https://github.com/firemodels/fds). The FDS simulations are assessed using the [`fdsreader` Python package](https://firedynamics.github.io/LectureFireSimulation/content/tools/03_analysis/02_fdsreader.html#boundary-data).

The experimental data can simply be cloned into the `GeneralInformation` directory, see respective README. Jupyther notebooks are used to assess the simulation results, stored in the `RunReports` directory. The simulations are located in sub-directories in `BurnerSims`. The labels of the sub-directories are used as simulation setup labels. In the `.gitignore` file, definitions are set to automatically exclude all FDS output from commits. Thus, it should be safe to run simulations in these sub-directories. Only the FDS input is stored.

For those with access: the simulation results are available at `/beegfs/cobra/MaCFP/MaCFP_Burner/BurnerSims`.

## Contributions

Please make sure to remove the output of Jupyter notebooks before commit. You can set up [filters in git to automate this process](https://timstaley.co.uk/posts/making-git-and-jupyter-notebooks-play-nice/).

At fist navigate to the `.git` directory in your cloned repo. Open the `config` file and add the following lines:

```
[filter "nbstrip_full"]
clean = "jq --indent 1 \
    '(.cells[] | select(has(\"outputs\")) | .outputs) = []  \
    | (.cells[] | select(has(\"execution_count\")) | .execution_count) = null  \
    | .metadata = {\"language_info\": {\"name\": \"python\", \"pygments_lexer\": \"ipython3\"}} \
    | .cells[].metadata = {} \
    '"
```

This filter uses [`jq` to perform the cleanup](https://stedolan.github.io/jq/download/). For example, on Windows you could download the `jq` binary (1.5 and up), put is somewhere and change the second line in the above command to reflect its location, like: `clean = "c:/jq-win64.exe --indent 1 \`; or set up an alias.

The filter needs also to be set up in the `.gitattributes` file, by adding `*.ipynb filter=nbstrip_full`. Here this is already done, when you clone the repo, the `.gitattributes` file should be already there.
