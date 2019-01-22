# Project structure

## Links and inspiration dump

- [Noble et al 2009, Plos Comp Biol](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424)
- [NiceR code](https://nicercode.github.io/blog/2013-04-05-projects/)
- [Carl Boettiger's workflow](https://www.carlboettiger.info/2012/05/06/research-workflow.html)



## Setting up a project structure
We start a project with **data**. The data that forms the basis of your work, will get its own, read-only `data` folder. It is important that the content of this folder does not change; your code will not edit it, nor will you. It will only be used as source material for the project during its analysis. The project will generate **output**, which 

`{finish this paragraph}`


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


`{make folder tree to visualize the above}`



### Absolute and relative paths



## Automating the project structure

What **could** we want to automate?

- installing the required packages and dependencies
- generating the project structure
- analyse
- tests to determine whether the project can be run
- downloading data, filling up the data folder
- rendering documentation



## Exercises 

### SLide exercise

- Examples (which folder goes this imaginary file?), central discussion, ambiguous


### Individual 

- Create a folder structure like the one before for your own project (including subdirectories)
- Place a data file in the data folder, write a script that reads the datafile, generates an output file, and places it in the appropriate folder.



### Group 

Create a folder structure like the one discussed before. Get consensus about the structure. 

