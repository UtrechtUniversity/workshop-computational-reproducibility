## Exercises

The main aim today is to prepare your project in such a way that someone else can (try to) run your scripts and reproduce your results. For inspiration checkout [this R project](https://github.com/MindTheGap-ERC/StratPal_ms_supp) or [this Python project](https://github.com/UtrechtUniversity/gis-python-power).

1. Write a README

    - Make sure to include the following information:

    - What does your project do?

    - How does the user access your project? (E.g. download, or clone with git clone…)

    - What should the user install 

    - How does the user run the main script(s)

1. Dependencies

    - What version of the programming language is used?
    - What versions of the packages are used?
    - Either write the packages and version numbers in your README
    - Or create a file that can be used by an environment manager (`renv`, `uv`, `conda`) and provide instructions to replicate the environment


1. Optional: Improve code readability 
    - Run a linter through your code and identify style issues:

        * **R:** [`lintr`](https://lintr.r-lib.org/)
        * **Python:** [`ruff check`](https://docs.astral.sh/ruff/)
    - Edit your code to improve the style compatibility, based on the feedback from your linter.

    - Run an autoformatter through your code to automatically fix issues instead of simply flagging them:

        * **R:** [`styler`](https://styler.r-lib.org/)
        * **Python:** [`ruff format`](https://docs.astral.sh/ruff/)

1. Optional: Create a function
    - Identify pieces of code that are candidate to convert into a function
    - Create a function (ask help if needed)

1. Optional: Comments and Docstrings
    - Add a docstring to a function, preferably the last function you worked on (so it’s fresh in your memory). Keep in mind: what does my user need to know when they are working with this function?

    - Grab a limited chunk of code to work on, and look at the existing comments. Can you replace a ‘how’ comment with a ‘why’ comment? Think: what is the purpose of this code? Rather than: this is how this code works.

    - Are there elements in your chunk that are currently without comments that would benefit from clarification? Try to comment on the thought behind the code rather than simply translating its process in English.

    - Can you delete superfluous comments or zombie code? If the code is clear enough without the comment, it’s better to remove it.