# Version control, testing, and license

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

## Licenses

Choosing the right license for your project is important. The license defines what other are allowed to do with your code. There a hunderds of different software licenses. Some of them are very permissive while other .... 

## Publishing your project on Github or Gitlab

Nowadays, there a websites to host your code. These websites run a version of git that makes it possible for other to easily see your code, the commits you made and branches you work on. These websites offer more, they make it possible to make issues, make releases, test your software and let others collaborate. 



Templates and ideas for README
- https://gist.github.com/PurpleBooth/109311bb0361f32d87a2
- https://github.com/matiassingers/awesome-readme
