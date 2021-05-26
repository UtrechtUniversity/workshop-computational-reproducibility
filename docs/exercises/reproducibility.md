# Exercises: Reproducibility

## Content

[Slides on Reproducibility (for reference)](../slides/slides_reproducibility.html) 

The following topics are covered:
* obtaining a doi and ensuring accessibility
* dealing with requirements and dependencies

## Exercises

Last but not least, let's make our code accessible for others! 
Here are your last videos and accompanying exercises. 
Watch the video, then apply the exercises to your own project. 
Use the slides linked above for reference, and be sure to ask for help when you need it!

### 1. Dependencies

#### 1.1 Video

[Video: Dependencies](https://vimeo.com/464028630)

#### 1.2 Exercise
Address dependencies and language versions in your README.
- What version of your coding language is required?
- Which packages does a user need to install before running your project?
  What versions?
- Can you provide their installation instructions?


### 2. Binder

#### 2.1 Video

[Video: Binder](https://vimeo.com/464010497)

#### 2.2 Exercise (optional)
**For R**

- Generate a file called `runtime.txt`, either in the root of your project, or in a (hidden) folder called `.binder/`.
- Write in the file: `r-2020-10-02` -- or specify the R version with `r-3.6-2020-10-02`.
- Write a file called `install.R`, in `.binder/` or in root, and use it to write install code for your packages, e.g.:
  ```
  install.packages("ggplot2")
  install.packages("dplyr")
  ```
- Binderise your project by following the instructions via [mybinder.org](https://mybinder.org/)

**For Python**

Binder automatically loads Python 3.6.

- Add dependencies to your binder in the requirements.txt file like this:
  ```
  numpy==1.14.5
  pandas==1.1.2
  ```
- Binderise your project by following the instructions via [mybinder.org](https://mybinder.org/)


### 3. Archiving

#### 3.1 Video

[Video: Archiving](https://vimeo.com/463947879)

#### 3.2 Exercise (optional)

- Follow the workflow [outlined in this guide](https://guides.github.com/activities/citable-code/) to archive your code to Zenodo.
  Use the [Sandbox version of Zenodo](http://sandbox.zenodo.org/) to make sure your repository is not actually archived permanently!
