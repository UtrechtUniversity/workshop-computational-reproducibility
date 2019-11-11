# !!This workshop is under construction!!

# Best Practices in Writing Reproducible Code

Ensuring your research is reproducible can be a difficult task. Scripting your analysis is a start, but this in and of itself is no guarantee that you, or someone else, can faithfully repeat your work at a later stage. In this workshop, we will help you not only to make your work reproducible, but also to increase the efficiency of your workflow. We do this by teaching you a few good programming habits: how to set up a good project structure, how to comment well, and how to document your code so that it can be used by others. We will furthermore introduce you to Git and GitHub, which are essential tools in managing and publishing code. Reproducibility requires extra effort, but we will focus on teaching you skills that will save you much more time in the long run than they cost to implement.

In this hands-on workshop, you will learn to become a better programmer. We will take you through a project  from research question to published code in a single (admittedly intense, but fun!) day. At the end of this day you will know:

- How to set up and use an efficient project structure, and make it easily replicable;
- How to document your code well;
- How to use Git for version control, and GitHub to publish your code;
- How to license and release your code to ensure maximum reusability.

The workshop is language-independent in principle. We do require participants to have at least a basic understanding of a programming language like Python or R. If you are unsure if the language you use will fit our course, please get in touch before registration.

## Program

The course contains three main elements:
1. **Project setup**: [choose a license, set up a repository](project-setup/README.md), [define folders](project-setup/project_structure.md), [choosing an IDE](project-setup/ide.md).
2. **Code quality & documentation**: [code readability, code review and refactoring, defensive programming, unit tests](code-documentation/code_quality.md), [comments, documentation, creating documentation with doxygen](code-documentation/documentation.md).
3. **Publication & reproduction**: [obtaining a doi, ensuring accessibility](reproducibility/access.md), [reproducibility requirements](reproducibility/reproducibility.md).


## Schedule

| Time  | Activity |
|-------:|----------|
|9:00 | Welcome & introduction| 
| 9:30 | Project setup |
| _10:30_ | _coffee break_|
| 10:45 | Version control with git |
|_12:30_ | _lunch (not provided)_ |
| 13:30 | Code quality & documentation |
|_15:15_| _coffee break_ |
| 15:30 | Reproducibility| 
| 16:30 | Reproduction|
| 17:00 | Concluding remarks|

## Preparations
- Install docker
- If you do not have a github/gitlab account yet: make one
- bring data and a question.



## Brainstorm

The results of a brainstorm on 6 May 2019 at the Utrecht University Library is [on these slides](https://docs.google.com/presentation/d/1MIPsWt08Kixe1TZfPeM8LvJv7p2es7lZ4Ui88FYbl5Y/edit?usp=sharing).


# Introduction

This is an introductory workshop. There are many more tools out there that you can use to make your work reproducible, and it is worth it to invest more time than today into obtaining habits and tools for reproducible research. Many tools are language-specific, so we will not address them for that reason, but we have linked to them as much as possible. Furthermore, this workshop is a work in progress, so we encourage you to submit tools you have discovered, for us to update this repository.




# Tools for reproducible research


## Tools in/with R

- **Rmarkdown**
- **Binder**, opens your github repo in a live Rstudio cloud server. See [this blogpost](https://kbroman.org/blog/2019/02/18/omg_binder/) for more ooohs and aaahs.


## General tools
- **Docker**



## Material

An overview of the workshop material is listed here.

**Workshop reference material**

- [slide template option](https://www.overleaf.com/latex/templates/fibeamer-for-utrecht-university/zhzstbsdzfkb)
- [slides on overleaf](https://www.overleaf.com/project/5cd02c9080045054bb2c6894) (private access?)

**Exercises**

**External resources and developer inspiration**

- https://guide.esciencecenter.nl/
- https://software-carpentry.org (who develop all their material on [github](https://github.com/swcarpentry). Specifically check out repos on [project management](https://github.com/swcarpentry/managing-research-software-projects), [make](https://github.com/swcarpentry/make-novice), [git](https://github.com/swcarpentry/git-novice), and [this repo that collects all their lessons](https://github.com/swcarpentry/swcarpentry).
- https://nicercode.github.io/
- [The role of data stewardship in software sustainability and reproducibility](https://zenodo.org/record/1419085#.XEneGrpFxaQ)
- [A book-in-progress on reproducible science by the Turing institute](https://github.com/alan-turing-institute/the-turing-way/)
- [GIT pro book](https://www.git-scm.com/book/en/v2)
- [Packaging data analytical work reproducibly using R (and friends)](https://peerj.com/preprints/3192.pdf)
- [Teaching Computational Reproducibility for Neuroimaging](https://www.frontiersin.org/articles/10.3389/fnins.2018.00727/full)
- [Defensive Programming in Python](https://github.com/UU-IMAU/Python-for-lunch-Notebooks/blob/master/PFL_10_defensive_programming/Defensive_programming.ipynb)

## Acknowledgements

This course was developed at Utrecht University, and received contributions from:
- [Armel Lefebvre](https://github.com/armell)
- [Barbara Vreede](https://github.com/bvreede)
- [Bianca Kramer](https://github.com/bmkramer)
- [Cedric Thieulot](https://github.com/cedrict)
- [Erik van Sebille](https://github.com/erikvansebille)
- [Jeroen Bosman](https://github.com/JeroenBosman)
- [Jeroen Ooms](https://github.com/jeroen)
- [Jonathan de Bruin](https://github.com/J535D165)
- [Lukas van de Wiel](https://github.com/hooiberg)
- [Mateusz Kuzak](https://twitter.com/matkuzak)
- [Menno Fraters](https://github.com/MFraters)
- [Philippe Delandmeter](https://github.com/delandmeterp)
- [Renato Alves](https://github.com/unode)
