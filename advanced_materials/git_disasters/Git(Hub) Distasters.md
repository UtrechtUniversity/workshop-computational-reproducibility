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

::: {.callout-note}

## What is a force-push?

Normally `git push` only adds new commits on top of what is already on GitHub. If your local branch and the remote branch have diverged (for example, after `git reset --hard HEAD~1` removed a commit that is already on GitHub), Git refuses the push, because it would throw away commits on the remote.

A **force-push** (`git push --force`) tells GitHub: "Make this branch look exactly like mine, even if that deletes commits." The commit disappears from the remote branch's history.

This is dangerous on shared branches:

- Collaborators who already pulled the commit still have it locally. Their history no longer matches `origin/main`, and the next pull can produce confusing conflicts, or they may accidentally push the "deleted" commit back.
- Work that was based on the deleted commit becomes orphaned.
- The removed commit is hard to recover for anyone who does not have a copy.

Force-pushing is fine on a private feature branch that only you use. If you must force-push, prefer `git push --force-with-lease`, which refuses to overwrite commits you have not seen yet.

:::

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
- Removing a secret from history with `git-filter-repo`
- Force-pushing rewritten branches safely and what collaborators must do afterwards
- GitHub features: secret scanning and push protection
- Prevention: `.gitignore`, environment variables, pre-commit hooks

**Prerequisite:** your fork of the recipe book. Use only the clearly fake value below; never put real credentials in a workshop repository.

### Scenario

Suppose you have a script that uploads recipes from your recipe-book repo to a fictional *FoodBlog* website. This script requires a `foodblog.env` config file containing settings for your blog, including a username and API key needed to upload to the blog. Suppose the script is run on GitHub using GitHub Actions when you push changes -- there, it uses [GitHub Secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets), so your API key does not need to be in the config file. But you sometimes also want to run the upload script locally -- for this, you sometimes add the API key to your `foodblog.env`.

It is easy to see how in this scenario you might end up accidentally committing and pushing the secret API key to GitHub. This kind of thing, unfortunately, happens often!

In this exercise, you will learn how to address this scenario---and how to prevent it!

#### Create the disaster

Work in your clone of your fork. The repository has no `.gitignore`, which is part of how this happens. To make the config file tracked before you add the key, create a harmless starter version on `main`:

```
git switch main
git pull
cat > foodblog.env << 'EOF'
FOODBLOG_TITLE=my-recipe-book
EOF
git add foodblog.env
git commit -m "Add FoodBlog config with title"
```

1. Create a feature branch:

```
git switch main
git pull
git switch -c add-pesto-blog
```

2. Create a new recipe:

```
cat > sides/pesto.md << 'EOF'
# Pesto

- Basil
- Pine nuts
- Parmesan
- Olive oil
- Garlic
EOF
```

3. After creating this recipe, you want to manually run the upload script, so you add your API key to the config file:

```
echo "FOODBLOG_API_KEY=not-a-real-key" >> foodblog.env
# imagine you now run the fictional upload script: $ upload_foodblog.py
```

4. You now want to commit the new recipe. You stage that file, then use `git commit -am`:

```
git add sides/pesto.md
git commit -am "Add pesto recipe"
```

::: {.callout-note}

In `git commit -am "message"`, `-a` stages modifications and deletions to tracked files (similar to `git add -u`), and `-m` supplies the commit message. It does **not** add untracked files. Here, the recipe is staged explicitly, but `-a` also stages the unintended API-key change in the already tracked config file. A safer option is to stage only the intended file with `git add <file>` and commit without `-a`. For finer control over parts of a file, use `git add -p` to interactively choose changes to stage, then review with `git diff --cached` before committing.

:::

5. Now you realize you also need salt:

```
echo "- Salt" >> sides/pesto.md
git commit -am "Add salt to pesto"
```

6. Push the branch to your fork and open a pull request:

```
git push -u origin add-pesto-blog
```

Look at your branch on GitHub and open `foodblog.env`: the fake key is visible in the file and in the commit history.

::: {.callout-note}

GitHub *push protection* may block a push when a value matches a known secret pattern. This exercise uses a clearly fake value and does not guarantee that push protection will trigger.

:::

7. To simulate a second branch that independently adds the same key to the tracked config, start from `main` and change the file there too:

```
git switch main
git pull
git switch -c upload-retry
echo "FOODBLOG_API_KEY=not-a-real-key" >> foodblog.env
git commit -am "Configure upload retry"
git push -u origin upload-retry
git switch add-pesto-blog
```

Now the key appears in separate commits on both `add-pesto-blog` and `upload-retry`. The branches may each have more than one commit containing or descending from the exposed key.

8. Oh noooooooo!!! Your colleague points out that you leaked a key. You quickly delete the file from `add-pesto-blog` and push again:

```
git rm foodblog.env
git commit -m "Remove config file"
git push
```

Is the secret gone from the repository? Check all local branches and the other remote branch:

```
git log --all --oneline -- foodblog.env
git show origin/upload-retry:foodblog.env
```

The key is still in the history, and the `upload-retry` branch still contains it in its latest version. Deleting the file on one branch only adds a new commit; it does not erase the earlier commits or the copy on the other branch.

### Step 1: Revoke the secret

If this had been a real credential, **immediately revoke or disable it at the service that issued it**. Then create a replacement and store it in an approved secrets manager or environment variable, not in the repository. Revocation is the urgent step because it stops the leaked credential from being used; **history cleanup cannot make an exposed credential secret again or guarantee that every copy is erased**.

Purging it from repository history is probably still worthwhile: for instance, there might be a delay in revoking the credential. And as long as it's there, you may keep receiving alerts from GitHub about secret exposure. Or imagine that instead of leaking a credential, you have accidentally pushed some non-revokable personal information. For all these reasons, knowing how to remove something from the git history is invaluable!

### Step 2: Remove the secret from history

Because the key was committed on multiple branches, we need to remove the config file from every affected branch history rather than fixing only the current branch. This is because every commit records a snapshot of the files at that point. A later commit that deletes `foodblog.env` changes the current snapshot, but the earlier snapshots—and the secret in them—remain reachable in history, and therefore on GitHub on the web.

We will use the `git-filter-repo` tool to rewrite that history, purging the secret from it. It removes the named path from each commit it processes, then updates the affected local branch refs to point to rewritten commits. Doing this manually would mean finding every commit on every affected branch that contains the file, editing or recreating those commits, then recreating all their descendant commits because their parent commit IDs changed. The work grows quickly with the number of commits and branches, and it is easy to miss a copy. That is why a history-rewriting tool is useful here.

Use a fresh clone for the cleanup. `git-filter-repo` checks that it is operating on a fresh clone and may refuse to rewrite an existing working repository. A fresh clone also gives you a clean working tree and the latest pushed branches:

```
git clone https://github.com/YOUR-USERNAME/recipe-book-2024.git recipe-book-cleanup
cd recipe-book-cleanup
git switch --track origin/add-pesto-blog
git switch --track origin/upload-retry
git switch add-pesto-blog
```

Install [`git-filter-repo`](https://github.com/newren/git-filter-repo) if needed. Run this from the root of `recipe-book-cleanup`:

```
git filter-repo --path foodblog.env --invert-paths
```

The command may take a little while, but it does the same path removal consistently across the local branches in one operation.

Verify that the path no longer appears in the rewritten local branches:

```
git log --branches --oneline -- foodblog.env
git ls-tree -r --name-only add-pesto-blog -- foodblog.env
git ls-tree -r --name-only upload-retry -- foodblog.env
```

All three commands should print nothing: there should be no commit in the local branches that touched the path, and neither branch tip should contain it.

`git-filter-repo` may remove the `origin` remote as a safety measure. Check it with `git remote -v`; if it is missing, add your fork again (replace the username with yours):

```
git remote add origin https://github.com/YOUR-USERNAME/recipe-book-2024.git
```

Then update only the affected branch tips with the cleaned histories:

```
git push --force origin add-pesto-blog upload-retry
```

This (`--force`) intentionally rewrites the published branches. See [What is a force-push?](#what-is-a-force-push) for why force-pushes can be risky and why `--force-with-lease` is generally preferred. Before proceeding, confirm that `origin` is your fork, verify that nobody has pushed new commits since you last fetched, and coordinate with collaborators. In this cleanup, `git-filter-repo` may remove the remote-tracking refs that `--force-with-lease` relies on. Tell collaborators not to push old clones; they should re-clone the cleaned repository or carefully reset their local branches. A clone or fork made before cleanup can still contain the old commits.

*For real credentials, contact GitHub Support if sensitive data remains accessible through cached views or pull-request references.*

### Step 3: Prevent it from happening again

Add files that may contain local credentials to `.gitignore` **before** creating them:

```
echo "foodblog.env" >> .gitignore
```

Keep credentials outside the repository, such as in environment variables or an approved secrets manager. Enable GitHub secret scanning and push protection where available, and consider a pre-commit secret scanner for local checks. These are additional safeguards, not a substitute for rotating a credential that was exposed.

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