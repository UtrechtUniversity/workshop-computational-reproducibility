# Setting up your project

As the saying goes: well begun is half done. As you get started with your project, you can frame it such that you facilitate a reproducible workflow for the rest of the project. In this workshop we will focus on a few things:
* Publication & Licensing
* Project structure
* Version control
* Testing

## Publication & Licensing

Wait, why are we starting here? Isn't 'publication' the thing you do... _at the end?_

We recommend starting with a published (and thus _public_) version of an empty project, and choosing a license from the start. Doing this will encourage you throughout to keep the readability of your work in mind, and minimize the mess you will have to disentangle when you finally decide to make your project publicly available.

Of course, this is not a hard and fast rule. You may deal with sensitive data, and perhaps your code, while developing, contains direct references to this data. You may want to keep your code private because it is your as-yet unpublished, novel and groundbreaking analysis protocol or model, and you would prefer to remain unscooped. Whatever reason you have for wanting to stay private (for now), do consider whether your project may not benefit from being written openly.

Regardless of publication status, we encourage you to choose a license from the start. Even if a project is private, knowing what your license is will prevent you from generating content that does not match the license. For example: you may want to deploy someone else's code that is licensed in a way that is incompatible with the license you prefer. If you have not yet chosen your license, you may inadvertedly create conflicts.

Summarizing: the more you know about your project from the start, and can keep in mind as you proceed, the lower the chance that you will create something that conflicts with these standards that **should eventually all be part of your project**. Why not hit the ground running?

### Where to publish?

There are many websites designed to host code. You should choose a website compatible with the version control software you use, which will ideally not just make your work public, but also:
- allow others to see not just the code, but also individual commits you made and branches you work on;
- publish different releases;
- test your software;
- provide a platform for collaboration.




### More on this
* [_Pick a license, any license._ by Jeff Atwood (Coding Horror)](https://blog.codinghorror.com/pick-a-license-any-license/)
* [Want to choose an license? This tool helps!](https://choosealicense.com)



## Version control with git 
### Overview

1. explain concept of git (local)
  * init
  * add
  * commit
2. explain github (and bitbucket) (internet integration) 
  * push
  * pull
        
### What problem does git solve?

1. management of different versions of one code (switch/merge)
2. writing new features while using the main code
3. collaboration on one code between many authors (scales well)
4. enables easy and efficient backup of different versions of one code

### How does git solve those problems?
#### How does git work

1. starts with an empty directory (git init)
2. every change is tracked, this is called a diff
3. sets of changes can be saved, this is called a commit
4. sets of one to many commits form a branch
5. you can change between different branches

### How does this help?

1. code management: different version are just different sets of commits
2. new features: start with a branch and add new commits. Later add those commits to main branch
3. collaboration: every one writes a small set of commits, and adds them to the main branch
4. the repository has the full history of a project and you can put that online for free (more on that later)

### How do you use git?
#### For a new project (command line)
1. git init
2. git add file1.cpp folder1/file2.txt
3. git commit -m "Write there what the changes are in this commit"

Use 'git status' to see what the status of your (local) git repository is.
### How do you install git

### exercises


## Publishing your project on Github or Gitlab





Templates and ideas for README
- https://gist.github.com/PurpleBooth/109311bb0361f32d87a2
- https://github.com/matiassingers/awesome-readme


# Glossary
| Concept | Definition |
|:-----------:|:-------------|
| License | A (legally binding) document with user guidelines, defining how the product (in this case: software) can be used and/or distributed. |
| Version control | |
| Commit | |
| Branch | |


