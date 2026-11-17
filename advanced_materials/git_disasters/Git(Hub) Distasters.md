# Git(Hub) Distasters

We are walking you through some common disasters that can happen with Git and on GitHub and we show you how to solve them.

Please created a fork of <https://github.com/dianaiusan/recipe-book-2024> and clone it also locally to your computer.

## Help, I accidentally committed my changes to the wrong branch!

### Local Branch

Check with git status that you are on the `main` branch. It is good practice to work on feature branches and not directly on `main`.

Create a feature branch `new_recipe` locally on your computer.

```
git branch new_recipe
```

Git unfortunately does not switch automatically to this new branch. But you forgot that and now you added a new recipe and committed it to `main`:

```
% cat recipe-book-2024/soups/snert.md
# Snert

- Peas
- Carrots
- Onions
- Hema rookworst
```

```
git add soups/snert.md
git commit -m "new soup"
```

Oh noooooooooo!!!!!! 

Don't panic. What we need to do is to transfer those commits to our newly created branch.

**Solution 1**
First, look at what happened:

```
git status
git log --oneline --all --graph
* cb82d8a (HEAD -> main) new soup
| * 6b3acd5 (origin/carrot_cake) carrot cake recipe
|/
*   f154464 (origin/main, origin/HEAD) Merge pull request #1 from chStaiger/apple_pie
|\
| * 4b729e3 Adding Apple Pie
|/
```

We see, in contrast to other changes on the main branch, the "new soup" went directly to main. The "carrot cake" has not been merged yet.
The part of the graph "Apple pie" shows how a neatly merged feature branch looks like.

1. `new_recipe` needs to catch up with main

```
git switch new_recipe
git merge main
```

This will also work if you completely forgot to create a new feature branch. Simply create the branch from main and then continue with step 2.

2. Clean up main

```
git switch main
git reset --hard HEAD~1
```

Now the log shows us

```
git log --oneline --all --graph
* cb82d8a (new_recipe) new soup
| * 6b3acd5 (origin/carrot_cake) carrot cake recipe
|/
*   f154464 (HEAD -> main, origin/main, origin/HEAD) Merge pull request #1 from chStaiger/apple_pie
|\
| * 4b729e3 Adding Apple Pie
|/
```

The soup has moved to `new_recipe` and the main branch HEAD is again where it should be, at the last pull request.

### Remote Branch

Now we extend the disaster above. We did not only update `main` locally by accident but also pushed the new recipe to main to the remote repository.

The log again shows us what happened locally:

```
git log --oneline --all --graph
* 8e5dcdc (HEAD -> main, origin/main, origin/HEAD) new soup
| * 6b3acd5 (origin/carrot_cake) carrot cake recipe
|/
*   f154464 Merge pull request #1 from chStaiger/apple_pie
|\
| * 4b729e3 Adding Apple Pie
|/
```

Some AI might tell you now that you need to `reset` main. 
But this is not the save solution!

Other people may already have pulled the current version of `main` (at hash 8e5dcdc). If you reset main locally, it differs from origin/main, and you would have to force-push (`git push --force`) to make GitHub match. That rewrites shared history and causes problems for everyone who already has the commit. 

So we undo the commit with a new commit (git revert) instead of deleting it.

As you might have the old feature branch `new_recipe` with the commits. Let's use a new feature branch for this exercise.

Create

```
git branch new_recipe_2
```

and take a look at the history:

```
git log --oneline --all --graph

* 8e5dcdc (origin/main, origin/HEAD, main) new soup
| * cb82d8a (HEAD -> new_recipe) new soup
|/
| * 6b3acd5 (origin/carrot_cake) carrot cake recipe
|/
*   f154464 (new_recipe_2) Merge pull request #1 from chStaiger/apple_pie
|\
| * 4b729e3 Adding Apple Pie
|/
```

The last changes on `main` have the hash `8e5dcdc` which is indicated by our commit message "new soup". We also see our "new soup" with a different hash in the feature branch.

**Step 1**

Now we want to transfer the hash 8e5dcdc to our feature branch `new_recipe_2`.

```
git switch new_recipe_2
git cherry-pick 8e5dcdc
```

`git cherry-pick` takes the changes of one commit and re-applies them as a new commit on the current branch. Check with `git log --oneline` and `ls soups/` that the new recipe is now on `new_recipe_2`.

**Step 2**

Now we remove the commit from `main`.

```
git switch main
git revert 8e5dcdc --no-edit
git push
```

**Step 3**

Push your new feature to the repository in its own branch:

```
git switch new_recipe_2
git push -u origin new_recipe_2
```

And create a pull request.

Verify your history:

```
git log --oneline --all --graph
```

## Git commit merge versus squash and merge

What is the difference between `commit merge` or a pull request and `squash and merge`.

Both options bring the changes of a pull request into `main`. The difference is what ends up in the history of `main`.

With `Create a merge commit`, all commits of the feature branch are kept in the history of main, and Git adds one extra commit (the merge commit) that joins the two lines of history. You can see exactly where the feature branch started and when it was merged. Git also recognises the branch as merged.

With `Squash and merge`, GitHub combines all commits of the pull request into one new commit on main. The history stays short and linear, but the individual commit messages are lost, and Git no longer sees any connection between main and the feature branch. Even though the content is in main, the branch is not listed as merged.

### Try it with your existing pull request

Use the open pull request from the previous exercise. Add one or two extra commits to the branch first, so the difference becomes visible:

```
git switch new_recipe_2
echo "- Leek" >> soups/snert.md
git commit -am "Add leek"
echo "- Celery" >> soups/snert.md
git commit -am "Add celery"
git push
```

On GitHub, merge the pull request with "Squash and merge" and give it a clear title "Merge with Squash". Then look at the result locally:

```
git switch main
git pull
git log --oneline --graph --all
git branch -a --merged main
```

```
* f362a0a (HEAD -> main, origin/main, origin/HEAD) Merge with Squash (#2)
* e74f032 Revert "new soup"
* 8e5dcdc new soup
| * 5ad67bb (origin/new_recipe_2, new_recipe_2) Add celery
| * 9ad569f Add leek
| * 4fd4c73 new soup
|/
| * cb82d8a (new_recipe) new soup
|/
| * 6b3acd5 (origin/carrot_cake) carrot cake recipe
|/
*   f154464 Merge pull request #1 from chStaiger/apple_pie
|\
| * 4b729e3 Adding Apple Pie
|/
```

- `8e5dcdc new soup` is the commit that was accidentally pushed to main in the previous exercise.
- `e74f032 Revert "new soup"` is the revert that removed it again.
- `f362a0a Merge with Squash (#2)` is the squash commit created by GitHub. The (#2) is the pull request number, added automatically. This single commit contains the combined changes of all three commits from the pull request: the new soup, the leek, and the celery.


### Undo the "Squash and merge"

To get back to the original state of the Pull Request we will now undo the changes on `main`. Make sure you did not delete your feature branch.

The squash commit `f362a0a` is already pushed to `origin/main`, so the same rule applies as in the “already pushed” exercise: undo it with a new commit instead of rewriting history.

```
git switch main
git pull
git revert f362a0a --no-edit
git push
```

### Merge commit

Create a new Pull request into main with your feature branch.

On GitHub, merge the pull request with "Create a merge commit" and give it a clear title "Merge with Commit". Then look at the result locally:

```
git switch main
git pull
git log --oneline --graph --all
```

We now see all of our single commits showing up in the `main` branch history:

```
*   50b73bf (HEAD -> main, origin/main, origin/HEAD) Merge pull request #3 from chStaiger/new_recipe_2
|\
| * 5ad67bb (origin/new_recipe_2, new_recipe_2) Add celery
| * 9ad569f Add leek
| * 4fd4c73 new soup
* | eb7c1d2 Revert "new soup (#2)"
* | f362a0a new soup (#2)
* | e74f032 Revert "new soup"
* | 8e5dcdc new soup
|/
```

**Quiz**
- Which option gives a shorter history of main?
- Which option lets you still see every individual commit?
- Why does Git not recognize a squashed branch as merged?
- When would you choose which option?
