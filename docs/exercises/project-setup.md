# Exercises: Project Setup

## Content

[Project setup slides (for reference)](../slides/slides_project-setup.html) 

This section covers the following topics:
* Setting up a folder structure
* using git for version control
* publishing your project on github
* Choosing a license

## Exercises

Time for some hands-on practice!
We have 3 short videos and accompanying exercises for you to go through one by one.
Watch the video first, then apply the exercises to your own project.
Use the [slides](../slides/slides_project-setup.html) linked above to follow links discussed in the video.
Be sure to ask for help when you need it!

### 1. Project setup

#### 1.1 Video
[Video: Project management](https://vimeo.com/462773031)

#### 1.2 Exercise 
_NB: You can check the slides for more detail._
- Use cookiecutter to install a project structure in your system, following the video or [these slides](../slides/slides_project-setup.html#4).
- If you have trouble with cookiecutter: use the instructions [on this slide](../slides/slides_project-setup.html#6) instead.
- Take a look at the folder and files within it, to see where your answers to cookiecutter ended up!
- Place your project files in the right folder.
- Adjust paths in your code, and be sure to use relative paths!
- Does your code run in the new folder structure?


### 2. Version control

#### 2.1 Video
[Video: version control with Git](https://vimeo.com/463264170)

#### 2.2 Exercise
- Follow the steps in the video (or [on these slides](../slides/slides_project-setup.html#17)) to turn your folder into a git repository
- Make a remote version of your project on GitHub!
- Please note: are there (temporary) files you do not wish to track?
  Add them to the `.gitignore` file.
  Consider a `.gitignore` template for your language: examples on [this github repo](https://github.com/github/gitignore).
- Can you use Git and push to Github from your IDE?
- Experiment with editing and committing on github itself.
  You can then 'download' your code to your local repository using `git pull`.
- Optional: What happens if you edit the same file online and locally, and try to push/pull?
  (Hint: this often causes a 'merge conflict', which is no fun to experience.
  Going through it today means we can assist you if necessary!)

 
### 3. Publication & licensing

#### 3.1 Video
[Video: Publication & licensing](https://vimeo.com/463659936)

#### 3.2 Exercise
- Check the license in your project
- Take a look at other license options via [choosealicense.com](https://choosealicense.com/).
- Do you want to change your license? Go into your Github and change the LICENSE.md file. Don't forget to pull your remote changes!

