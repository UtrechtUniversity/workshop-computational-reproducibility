## To our participants: before the workshop

We have prepared the workshop as much as we could in a language-independent manner. That was not always possible, and where we are going into detail on tools and tricks we have done so catering to R and Python users. If you use a different language, we hope you will be able to help us add to the teaching materials with comparable tools you have found.

### Preparations
In order to take full advantage of the time we have together, would you please prepare the following:
 
#### Data and/or code
- **Bring some code!** We will be working on improving your workflow, so it is nice to have material to work with. Consider that we will need something that is small enough that you can edit/play with it during the workshop, but also comprehensive so that someone else may be able to reproduce your project on their computer (i.e.: there is something, like data, that goes in, and there is something, like results or figures, that comes out).
- **Optional: if you do not have code, bring data!** You will have some time to write code during the course, so don’t worry if you don’t have a project ready to go. We will also bring example data to work with BUT if you have your own data that you can bring that would be even better, as you’ll be more motivated to work with it!
- **Optional: if your data cannot be public: fake it!** We will be exchanging repositories at the end of the day, and try to reproduce each other's work. This can be problematic if your data cannot be shared. If the structure of the data can be put online, you could simulate the dataset (including covariates, so simple models have similar outcomes) using a package like [fakeR](https://cran.r-project.org/web/packages/fakeR/index.html). It simulates fake data within the same structure.

#### Installing
- **Pip (using miniconda)**
  - Check if you have pip by typing in a terminal: `pip -h`. If you get a lot of options, you are good to go; you can stop reading. If you get an error message, you will need to install it. Here is how to do that.
  - Download the miniconda installer here: [docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)
  - Run the installer; there is no need to change any detault settings, but just in case:
    - You want to install it 'just for me';
    - You want to 'add Anaconda to my PATH environment variable' and 'Register Anaconda as my default Python 3.7' (NB: you will also see a message in red text that selecting _Add Anaconda to my PATH environment variable_ is not recommended; continue with this selection to make using conda easier in your terminal. If you have questions or concerns, please contact your instructor.
  - Open a new terminal session and type: `conda install -c anaconda pip`
- **Git**
  - Check if you have git by tying in a terminal: `git -h`. If you get a lot of options, you are good to go; you can stop reading. If you get an error message, you will need to install it. Here is how to do that.
  - On a windows system: use [gitforwindows.org](https://gitforwindows.org). Make sure you include **git bash** in your installation!
  - On Mac or Linux: use [git-scm.com](https://git-scm.com/)
  - With Linux you can also use `apt-get install git` in your terminal, or whatever installer you prefer.
- **Rstudio** 
  - If you are working with R, make sure you have Rstudio!
  - If not, you can install it from [rstudio.com](https://rstudio.com/products/rstudio/download/)

#### Github
- **Sign up for a github account**, or gitlab or bitbucket, if you prefer. We will be doing our demonstrations in github, but most things we will show you have easy parallels on the other networks, so you can likely just follow along.
- **Make sure you have an ssh key pair set up** between your computer and the github (or gitlab, or bitbucket) account (there is a [great tutorial on how to do this on github](https://help.github.com/en/enterprise/2.17/user/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).


### Accessibility
The instructors have checked that the room and nearby bathroom facilities are wheelchair accessible. Furthermore, and we have done our best to accommodate color blindness with our slides and other material. Please let your instructors know if there is anything else in particular we can do to make sure you can take full advantage of our course.
