# Git(Hub) Disasters

This tutorial walks you through common Git and GitHub mishaps and shows you how to recover from them.

## Table of contents

- [Setup](#setup)
- [Exercise 1: Help, I accidentally committed my changes to the wrong branch!](#exercise-1-wrong-branch) (🟢 Beginner → 🟡 Intermediate)
  - [Local branch](#local-branch)
  - [Remote branch](#remote-branch)
- [Exercise 2: Merge commit versus squash merge](#exercise-2-merge-versus-squash) (🟡 Intermediate)
  - [Undo the "Squash and merge"](#undo-the-squash-and-merge)
  - [Merge commit](#merge-commit)
- [Exercise 3: Help, I accidentally committed secrets to GitHub!](#exercise-3-secrets) (🟡 Intermediate → 🔴 Advanced)
- [Exercise 4: Help, I need to rebase but there are so many merge conflicts!](#exercise-4-rebase-conflicts) (🔴 Advanced)

| Exercise | Level |
|----------|-------|
| 1. Committed to the wrong branch | 🟢 Beginner (local) / 🟡 Intermediate (remote) |
| 2. Merge commit versus squash merge | 🟡 Intermediate |
| 3. Committed secrets to GitHub | 🟡 Intermediate / 🔴 Advanced |
| 4. Rebase with many merge conflicts | 🔴 Advanced |

## Setup

Fork <https://github.com/dianaiusan/recipe-book-2024> to your GitHub account, then clone your fork to your computer. Run the commands below from the root of your local clone.

## Exercise 1: Help, I accidentally committed my changes to the wrong branch! {#exercise-1-wrong-branch}

**Level:** 🟢 Beginner (local branch) → 🟡 Intermediate (remote branch)

**You will learn:**

- How to check which branch you are on (`git status`, `git branch`, `git switch`)
- How to read history with `git log --oneline --all --graph`
- How to move a commit to another branch (`git merge`, `git reset --hard`, `git cherry-pick`)
- What a fast-forward merge is
- Why you should not rewrite shared history, and how to undo a pushed commit with `git revert`

### Local branch

Check with `git status` that you are on the `main` branch and that your working tree is clean. It is good practice to work on feature branches, not directly on `main`.

Create a feature branch named `new_recipe`:

```
git branch new_recipe
```

This creates the branch but does not switch to it. You forget to switch branches, add a new recipe, and commit it to `main` instead:

::: {.callout-tip}

Use `git status` to check your current branch before editing or committing. Creating a branch with `git branch` does not check it out.

:::

```
% cat soups/snert.md
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

Oh noooooooo!!!!!

Don't panic. We need to move the commit to our newly created feature branch.

**Solution 1**
First, inspect the current history:

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

The `new soup` commit went directly onto `main`, while `carrot cake` is still on its feature branch. The `Apple Pie` commits show a feature branch that was merged into `main`.

1. Bring `new_recipe` up to date with `main`:

```
git switch new_recipe
git merge main
```

Because `new_recipe` was created at the previous `main` commit, this merge brings the new commit onto `new_recipe` (normally as a fast-forward). If you forgot to create a feature branch before committing, create one now from `main`, then continue with step 2.

::: {.callout-note}

'fast-forward' means that Git does not create a merge commit, but instead just adds the new commit on top of the branch. This is only possible if the branch containing your new commit and the branch you want to merge into do not have divergent histories. Otherwise, git will have to create a merge commit.

:::

2. Move `main` back to its previous commit:

::: {.callout-warning}

`git reset --hard` discards uncommitted changes. Before running it, verify that you are on `main`, that the commit is safely reachable from `new_recipe`, and that your working tree is clean.

:::

```
git switch main
git reset --hard HEAD~1
```

Check the result:

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

The `new soup` commit is now on `new_recipe`, and `main` points to its previous commit.

### Remote branch

Suppose the disaster is worse: we did not only update `main` locally by accident but also pushed the new recipe to main to the remote repository. In this part of the exercise, we will simulate this scenario.

Let's recreate the same mistake on `main` and push it, so the commit is on the shared remote branch too. This simulates the accidental push:

```
git switch main
git cherry-pick new_recipe
git push origin main
```

The log now shows the pushed commit locally:

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

You might think you should reset `main`, but that is not the safe solution for a commit that has already been pushed.

Other people may already have pulled the current version of `main` (shown here with the example hash `8e5dcdc`). Resetting `main` locally would make it differ from `origin/main`; making GitHub match would then require a force-push. That rewrites shared history and causes problems for people who already have the commit.

Instead, undo the change with a new commit using `git revert`; do not delete the already-published commit.

The earlier `new_recipe` branch already contains the recipe, but we will use a new feature branch for this exercise. Fetch the remote branch, then create `new_recipe_2` from the commit immediately before the accidental push:

```
git fetch origin
git switch -c new_recipe_2 origin/main~1
```

Now inspect the history:

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

In this example, `8e5dcdc` is the accidental commit on `main`. The same recipe is on `new_recipe` under a different hash.

::: {.callout-note}

Hashes and pull-request numbers in this tutorial are examples. Use the values from your own history.

:::

**Step 1**

Now apply the pushed commit to `new_recipe_2`:

```
git switch new_recipe_2
git cherry-pick 8e5dcdc
```

`git cherry-pick` reapplies the changes from one commit as a new commit on the current branch. Check with `git log --oneline` and `ls soups/` that the recipe is now on `new_recipe_2`.

**Step 2**

Now undo the commit on `main`. Make sure the commit hash matches the accidental commit in your own log:

```
git switch main
git revert 8e5dcdc --no-edit
git push
```

**Step 3**

Push the feature branch to your fork:

```
git switch new_recipe_2
git push -u origin new_recipe_2
```

Then open a pull request from `new_recipe_2` into `main`.

Verify your history:

```
git log --oneline --all --graph
```

## Exercise 2: Merge commit versus squash merge {#exercise-2-merge-versus-squash}

**Level:** 🟡 Intermediate

**You will learn:**

- The difference between *Create a merge commit* and *Squash and merge* on GitHub
- How each option shapes the history of `main`
- Why Git does not recognize a squash-merged branch as merged (`git branch --merged`)
- How to undo a merge on a shared branch with `git revert`

**Prerequisite:** the open pull request from Exercise 1.

What is the difference between merging a pull request with a merge commit and using squash merge?

Both options bring the changes from a pull request into `main`. The difference is how those changes appear in `main`'s history.

With **Create a merge commit**, the feature branch's commits remain in `main`'s history, and Git adds a merge commit that joins the two lines of history. You can see where the feature branch started and when it was merged. Git recognizes the branch as merged because its commits are ancestors of `main`.

With **Squash and merge**, GitHub combines the pull request's changes into one new commit on `main`. This keeps `main`'s history short and linear, but the feature branch's individual commits are not part of `main`'s history (they remain on the feature branch unless it is deleted). Because the squash commit does not record merge ancestry, Git does not report the feature branch as merged, even though its changes are in `main`.

### Try it with your existing pull request

Use the open pull request from the previous exercise. Make sure `new_recipe_2` is checked out, then add two commits so the difference becomes visible:

```
git switch new_recipe_2
echo "- Leek" >> soups/snert.md
git commit -am "Add leek"
echo "- Celery" >> soups/snert.md
git commit -am "Add celery"
git push
```

On GitHub, merge the pull request with **Squash and merge** and use the title `Merge with Squash`. Then inspect the result locally:

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

- `8e5dcdc new soup` is the commit that was accidentally pushed to `main` in the previous exercise.
- `e74f032 Revert "new soup"` is the commit that undid that change.
- `f362a0a Merge with Squash (#2)` is the squash commit created by GitHub. The `(#2)` is an example pull-request number, added automatically. This one commit contains the combined changes from all three pull-request commits: the new soup, the leek, and the celery.


### Undo the "Squash and merge"

To restore `main` to its state before the squash merge, undo the squash commit. Make sure you have not deleted your feature branch.

The squash commit (shown here with the example hash `f362a0a`) is already pushed to `origin/main`, so undo it with a new commit instead of rewriting shared history. Use the actual hash from your log.

```
git switch main
git pull
git revert f362a0a --no-edit
git push
```

### Merge commit

Open a new pull request from `new_recipe_2` into `main`. Because squash merge did not add the feature-branch commits as ancestors of `main`, the branch can be proposed again after the squash commit has been reverted.

On GitHub, merge the pull request with **Create a merge commit** and use the title `Merge with Commit`. Then inspect the result locally:

```
git switch main
git pull
git log --oneline --graph --all
```

The merge commit joins the feature branch to `main`, so the individual feature-branch commits are visible in the history:

```
*   50b73bf (HEAD -> main, origin/main, origin/HEAD) Merge pull request #3 from chStaiger/new_recipe_2
|\
| * 5ad67bb (origin/new_recipe_2, new_recipe_2) Add celery
| * 9ad569f Add leek
| * 4fd4c73 new soup
* | eb7c1d2 Revert "Merge with Squash (#2)"
* | f362a0a Merge with Squash (#2)
* | e74f032 Revert "new soup"
* | 8e5dcdc new soup
|/
```

**Quiz**
- Which option gives a shorter history of main?
- Which option lets you still see every individual commit?
- Why does Git not recognize a squashed branch as merged?
- When would you choose which option?

## Exercise 3: Help, I accidentally committed secrets to GitHub! {#exercise-3-secrets}

**Level:** 🟡 Intermediate → 🔴 Advanced

**You will learn:**

- Why deleting a secret in a new commit does not remove it from history
- First response: revoke and rotate the exposed credential
- Removing a secret from history (`git commit --amend`, `git rebase -i`, `git filter-repo` or BFG Repo-Cleaner)
- Force-pushing safely (`git push --force-with-lease`) and what collaborators must do afterwards
- GitHub features: secret scanning and push protection
- Prevention: `.gitignore`, environment variables, pre-commit hooks

**Prerequisite:** your fork of the recipe book. Use a fake secret only (for example `API_KEY=not-a-real-key`); never use real credentials in this exercise.

### Scenario

TODO: Describe the scenario (e.g. adding a `config.env` file with an API key and pushing it to a branch on your fork).

### Step 1: Revoke the secret

TODO: Explain why this comes first, and how to rotate a credential.

### Step 2: Remove the secret from the latest commit (not yet pushed)

TODO: `git commit --amend` / `git reset`.

### Step 3: Remove the secret from history (already pushed)

TODO: `git filter-repo` or BFG, then force-push.

### Step 4: Prevent it from happening again

TODO: `.gitignore`, secret scanning, push protection, pre-commit hooks.

**Quiz**

- TODO

## Exercise 4: Help, I need to rebase but there are so many merge conflicts! {#exercise-4-rebase-conflicts}

**Level:** 🔴 Advanced

**You will learn:**

- What `git rebase` does and how it differs from `git merge`
- Why rebasing a long-lived branch can cause the same conflict many times
- Resolving conflicts (`git status`, `git diff`, `git add`, `git rebase --continue`)
- Escaping a rebase (`git rebase --abort`, `git reflog`, `git reset`)
- Reducing the pain: squashing first (`git rebase -i`), merging instead, `git rerere`
- Why you should not rebase shared branches

**Prerequisite:** your fork of the recipe book.

### Scenario

TODO: Describe the scenario (e.g. a feature branch that edits the same recipe files as several commits on `main`).

### Step 1: Start the rebase and meet the conflicts

TODO: `git rebase main`, reading conflict markers.

### Step 2: Resolve conflicts and continue

TODO: edit, `git add`, `git rebase --continue`.

### Step 3: Abort and recover

TODO: `git rebase --abort`, `git reflog`.

### Step 4: A smarter strategy

TODO: Squash commits first, or use `git merge`, or enable `git rerere`.

**Quiz**

- TODO
