# How to work with this repository


## Step 1: Load the repository and the dependencies

Clone the repository:
```bash
git clone git@github.com:UtrechtUniversity/workshop-computational-reproducibility.git
```

Open the Rproject (`workshop.Rproj`) in Rstudio.

(The following code will be in R, so should be run in Rstudio.)

Install renv:
```r
install.packages("renv")
```

Load the dependencies from the lockfile:
```r
renv::restore()
```

## Step 2: Edit the slides and the bookdown

After editing slides, render them locally by pressing the knit button.

Alternatively, you can run:
```r
rmarkdown::render('filename.Rmd')
```

After editing any part of the book, render it locally with:
```r
bookdown::render_book(input="index.Rmd", output_format = "gitbook")
```


## Step 3: Sharing the materials

When updating an existing instance of the workshop, updating the master branch of this repository will update the Github Pages version of the repository. Your changes will be visible on the Github Pages website a few minutes after the branch is updated.

When starting a new repository, make sure your repository has Github Pages enabled. You will want to build the site from the root of the master repository, to make sure both the bookdown and the slides can be rendered properly.