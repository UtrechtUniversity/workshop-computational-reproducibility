# Project structure


## Setting up a project structure
We start a project with **data**. The data that forms the basis of your work, will get its own, read-only `data` folder. It is important that the content of this folder does not change; your code will not edit it, nor will you. It will only be used as source material for the project during its analysis. The project will generate **output**, which 


### `~` (root)
This folder:

- contains all the following subfolders.
- contains analysis scripts.
- contains installation instructions and/or makefile script.
- contains the project's main README.

### `data`
This folder:

- contains (raw) research data, in read-only files.
- is filled at the start of the project, and may not be edited by you or your code during the project.
- may have subdirectories indicating e.g. different kinds of data (e.g. `images`).

Alternative folder name: `input/`

### `output`
This folder:

- contains the output files produced by your analysis.
- can be deleted, and reproduced entirely by the analysis.
- may have subdirectories; e.g. `figs` or `models`.

Alternative folder name: `results/`


### `scripts`
This folder:

- contains code.
- is never executed by itself: the content is limited to e.g. function definitions.
- may have subdirectories; e.g. `R` or `Python`.

Alternative folder names: `src/`, `R/`


### `doc`
This folder:

- contains the code documentation, perhaps even the paper.
- is edited during the project by the user.

Alternative folder name: `paper/`
