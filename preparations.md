# To our participants: before the workshop
	
We have prepared the workshop as much as we could in a language-independent manner. That was not always possible, and where we are going into detail on tools and tricks we have done so catering to R and Python users. If you use a different language, you are very welcome to join our workshop, and we hope you will be able to help us add to the teaching materials with comparable tools you have found.

## Preparation
In order to take full advantage of the time we have together, would you please prepare the following:
 
### Data and/or code
- **Bring some code!** We will be working on improving your workflow, so it is nice to have material to work with. Consider that we will need something that is small enough that you can edit/play with it during the workshop, but also comprehensive so that someone else may be able to reproduce your project on their computer (i.e.: it is nice if there is an output for someone to see at the end!). *NB: If you will not have code to work on, you can still take advantage of the workshop, but please contact your instructor before the course.*
- **Optional: if your data cannot be public: fake it!** We will be exchanging repositories at the end of the day, and try to reproduce each other's work. This can be problematic if your data cannot be shared. If the structure of the data can be put online, you could simulate the dataset (including covariates, so simple models have similar outcomes) using a package like [fakeR](https://cran.r-project.org/web/packages/fakeR/index.html), which simulates fake data within the same structure. Or check out or [this post on conjurer](https://www.r-bloggers.com/generate-synthetic-data-using-r/).

### Terminal
Some of the work we will do will take place over the command line, for which you will need a terminal.
Mac OS and Linux have easily accessible terminals (the program is usually called 'terminal', so that is easy).
Windows' default terminal (Command Prompt) uses different commands, so we do not recommend it for this workshop. 
Instead, as you install Git (see below), you can get the Git Bash terminal with your installation.

If you have no experience with the terminal at all, we recommend practising with it for some basic folder navigation to start.
[This 8.5 minute video](https://www.youtube.com/watch?v=j6vKLJxAKfw) is a good introduction.

### Installation
- **Git**
  - Check if you have git by typing in a terminal: `git`. If you get a lot of options, you are good to go; you can stop reading. If you get an error message, you will need to install it. Here is how to do that.
  - On a windows system: use [gitforwindows.org](https://gitforwindows.org). Make sure you include **git bash** in your installation!
  - On Mac or Linux: use [git-scm.com](https://git-scm.com/)
  - With Linux you can also use `apt install git` in your terminal, or whatever installer you prefer.
  
- **Pip (using miniconda)**
  - Check if you have pip by typing in a terminal: `pip -h`. If you get a lot of options, you are good to go; you can stop reading. If you get an error message, you will need to install it. Here is how to do that.
  - Download the miniconda installer here: [docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)
  - Run the installer; there is no need to change any detault settings, but just in case:
    - You want to install it 'just for me';
    - You want to check 'Add Anaconda to my PATH environment variable'. _(NB: you will also see a message in red text that selecting 'Add Anaconda to my PATH environment variable' is not recommended; continue with this selection to make using conda easier in your terminal. If you have questions or concerns, please contact your instructor.)_
  - Open a new terminal session and type: `conda install -c anaconda pip`.

### Code platforms
- **Sign up for a [github](https://github.com/join?source=header-home), [gitlab](https://gitlab.com/users/sign_up), or [bitbucket](https://bitbucket.org/account/signup/) account**. We will be doing our demonstrations in github, but most things we will show you have easy parallels on the other networks, so you can likely just follow along.
- **Make sure you have an ssh key pair set up** between your computer and the github/gitlab/bitbucket account. 
There is a [great tutorial on how to do this on github](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh).
There is often no need to use a passphrase for your key (and it will slow you down!), but you can opt to use this for an extra layer of security.
Here is a [guide by github on how to work with passphrases](https://docs.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases).
- Confirm the functionality of your ssh keypair using `ssh git@github.com`. Your terminal should tell you something like:
  ```
  Hi bvreede! You've successfully authenticated, but GitHub does not provide shell access.
  Connection to github.com closed.
  ```
