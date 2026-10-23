# Git and GitHub --- Beginner to Practical Guide

A beginner-friendly reference for learning Git and GitHub, understanding
common commands, and fixing everyday Git problems.

## Table of Contents

1.  [What are Git and GitHub?](#what-are-git-and-github)
2.  [Important Git concepts](#important-git-concepts)
3.  [Install and configure Git](#install-and-configure-git)
4.  [Create a repository](#create-a-repository)
5.  [The everyday Git workflow](#the-everyday-git-workflow)
6.  [Working with GitHub](#working-with-github)
7.  [Branches and merging](#branches-and-merging)
8.  [Pulling and pushing changes](#pulling-and-pushing-changes)
9.  [Viewing history and comparing
    changes](#viewing-history-and-comparing-changes)
10. [Undoing changes safely](#undoing-changes-safely)
11. [Merge conflicts](#merge-conflicts)
12. [Common errors and solutions](#common-errors-and-solutions)
13. [Using `.gitignore`](#using-gitignore)
14. [Good commit-message practices](#good-commit-message-practices)
15. [A complete example workflow](#a-complete-example-workflow)
16. [Quick command reference](#quick-command-reference)
17. [Practice exercises](#practice-exercises)

------------------------------------------------------------------------

## What are Git and GitHub?

### Git

Git is a **version control system**. It records changes to files so you
can review previous versions, work on separate branches, and collaborate
with other developers.

### GitHub

GitHub is an online service for hosting Git repositories. It makes it
easier to share code, collaborate, review changes, and keep a remote
copy of a project.

**Simple difference:** Git is the version-control tool on your computer;
GitHub is a website/service where Git repositories can be hosted.

## Important Git concepts

-   **Repository (repo):** A project tracked by Git.
-   **Working directory:** The files you are currently editing.
-   **Staging area:** The place where you select changes for the next
    commit.
-   **Commit:** A saved snapshot of staged changes.
-   **Branch:** A separate line of development.
-   **Remote:** A repository hosted somewhere else, often on GitHub.
-   **`origin`:** The usual name given to the main remote repository.
-   **`main`:** A common name for the primary branch.
-   **`HEAD`:** A reference to the commit/branch you currently have
    checked out.
-   **Merge:** Combines changes from one branch into another.
-   **Clone:** Copies a remote repository to your computer.
-   **Push:** Sends local commits to a remote repository.
-   **Pull:** Fetches remote changes and integrates them into your
    current branch.
-   **`.gitignore`:** Lists files Git should normally leave untracked.

### The three main local stages

1.  **Working directory:** Edit a file.
2.  **Staging area:** Choose changes with `git add`.
3.  **Repository history:** Save staged changes with `git commit`.

Typical flow:

``` text
Edit files → git add → git commit → git push → GitHub
```

## Install and configure Git

Check whether Git is installed:

``` bash
git --version
```

Set the name and email that will appear in your commits. Replace the
examples with your own details:

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Check your settings:

``` bash
git config --global --list
```

`--global` applies the setting to your user account on that computer. A
repository-specific setting can be configured without `--global`.

## Create a repository

### Start tracking an existing project

Open a terminal in your project directory:

``` bash
cd path/to/your-project
git init
git status
```

-   `cd` changes the current directory.
-   `git init` creates a Git repository in the current directory.
-   `git status` shows the current branch and file states.

If Git says it reinitialized an existing repository, a `.git` repository
already exists there. That message does not normally mean the command
failed.

### Stage and commit files

``` bash
git add README.md
git status
git commit -m "Add project README"
```

-   `git add README.md` stages only that file.
-   `git add .` stages changes under the current directory; review
    `git status` first.
-   `git commit -m "..."` records the staged changes with a message.

A commit saves only what is staged. Editing a file after staging it may
require running `git add` again.

## The everyday Git workflow

For changes in an existing project:

``` bash
git status
git add .
git status
git commit -m "Describe the change"
git push
```

Explanation:

1.  Check which files changed.
2.  Stage the intended files.
3.  Check the staged changes again.
4.  Create a commit.
5.  Push the commit to the remote repository.

If this is the first push for a new branch, you may need
`git push -u origin main` instead of `git push`.

Useful status meanings:

-   **Untracked:** Git sees a new file but is not tracking it yet.
-   **Modified:** A tracked file has changed.
-   **Staged:** A change is selected for the next commit.
-   **Committed:** The change is saved in local history.
-   **Ahead/behind:** Your local branch has commits not on the remote,
    or the remote has commits you do not have locally.

## Working with GitHub

### Connect an existing local repository to GitHub

First create an empty repository on GitHub and copy its repository URL.
Then run the following in your project directory, replacing the URL:

``` bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git branch -M main
git push -u origin main
```

Before the first push, ensure you have made at least one commit.

-   `git remote add origin URL` connects the local repository to the
    remote URL.
-   `git branch -M main` renames the current branch to `main`.
-   `git push -u origin main` pushes `main` and sets the upstream so
    later `git push` and `git pull` can usually omit the remote and
    branch names.

Check configured remotes:

``` bash
git remote -v
```

If `origin` already exists, `git remote add origin ...` will report that
the remote already exists. Inspect it first:

``` bash
git remote -v
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

Use `set-url` only if you intend to change the existing remote.

### Clone a GitHub repository

``` bash
git clone https://github.com/USERNAME/REPOSITORY.git
cd REPOSITORY
```

Cloning downloads the repository and its history and normally configures
the remote as `origin`.

## Branches and merging

Branches let you work on a feature without immediately changing the main
branch.

List branches:

``` bash
git branch
git branch -a
```

Create and switch to a new branch:

``` bash
git switch -c add-documentation
```

Switch to an existing branch:

``` bash
git switch main
```

Older equivalent commands include `git checkout -b add-documentation` to
create and switch to a branch, and `git checkout main` to switch
branches.

Merge a feature branch into `main`:

``` bash
git switch main
git pull
git merge add-documentation
git push
```

Review the result and resolve any conflicts before pushing.

Delete a branch after it has been merged:

``` bash
git branch -d add-documentation
```

Delete a remote branch only when you intend to remove it from the shared
repository:

``` bash
git push origin --delete add-documentation
```

The `-D` option force-deletes a local branch even if Git believes it has
unmerged changes. Avoid it unless you understand the consequences.

## Pulling and pushing changes

### Push

``` bash
git push
```

Uploads local commits to the configured remote branch.

### Pull

``` bash
git pull
```

Downloads remote changes and integrates them into the current branch. A
pull may merge or rebase depending on your Git configuration and
options.

### If the remote has commits you do not have

First check your branch and working tree:

``` bash
git status
git branch --show-current
```

If you have uncommitted work, commit it or otherwise safely preserve it
before integrating remote changes. One common workflow is:

``` bash
git pull --rebase origin main
git push
```

Rebase replays your local commits on top of the updated remote branch.
If a conflict occurs, resolve it as described below. Do not use this
command blindly if your team has a different workflow.

## Viewing history and comparing changes

``` bash
git log
git log --oneline
git log --oneline --all
git status
git diff
git diff --staged
git show
```

-   `git log` shows commit history.
-   `git log --oneline` displays a compact history.
-   `git log --oneline --all` shows compact history across visible
    branches.
-   `git diff` shows unstaged changes.
-   `git diff --staged` shows changes staged for the next commit.
-   `git show` displays details of a commit, by default the latest one.

## Undoing changes safely

Git has several ways to undo work. Choose carefully because some
commands discard changes.

### Unstage a file but keep its edits

``` bash
git restore --staged README.md
```

### Discard unstaged edits to a file

``` bash
git restore README.md
```

This discards the file's unstaged changes. Make sure you no longer need
them first.

### Undo a published commit safely

``` bash
git revert COMMIT_ID
```

`git revert` creates a new commit that reverses the selected commit's
changes. It is often appropriate for commits already shared with others.

### Move a branch back to an earlier commit

``` bash
git reset --soft COMMIT_ID
git reset --mixed COMMIT_ID
git reset --hard COMMIT_ID
```

-   `--soft` moves the branch while keeping changes staged.
-   `--mixed` moves the branch and leaves changes unstaged.
-   `--hard` also resets tracked working files and can permanently
    discard local changes.

Use `reset` with care, especially on shared branches. Prefer `revert`
for undoing commits that others may already have pulled. Never use
destructive commands without checking your status and understanding what
will be lost.

## Merge conflicts

A merge conflict happens when Git cannot automatically combine
overlapping changes, for example when two branches edit the same lines.

A typical conflict workflow:

1.  Run the merge or pull command.
2.  Run `git status` to find conflicted files.
3.  Open each conflicted file and decide what the final content should
    be.
4.  Remove the conflict markers and save the file.
5.  Stage the resolved file.
6.  Complete the merge or rebase as Git instructs.

Conflict markers look like this:

``` text
<<<<<<< HEAD
Your current branch's content
=======
Incoming branch's content
>>>>>>> other-branch
```

These markers are temporary. Edit the file to the desired final content
and remove all markers before staging.

For a merge:

``` bash
git add README.md
git commit -m "Resolve README merge conflict"
```

For a rebase, after resolving and staging the file, use:

``` bash
git rebase --continue
```

Use the command Git requests for the operation in progress. To cancel an
in-progress merge, `git merge --abort` may help; to cancel a rebase, use
`git rebase --abort`. Check `git status` before taking action.

### When local and remote histories are unrelated

This can happen when you create a local repository with a commit and
separately create a GitHub repository with its own README commit. Git
may refuse to combine the two histories automatically.

If you intend to combine both histories, preserve your work and use:

``` bash
git pull origin main --allow-unrelated-histories
```

Git may create a merge conflict. Resolve the files, stage them, complete
the merge, and then push. This option is not a general fix for every
push error.

## Common errors and solutions

### 1. `src refspec main does not match any`

**Common reason:** The local `main` branch does not exist yet, or there
is no commit to push.

Check:

``` bash
git status
git branch
git log --oneline
```

If the files are ready but there is no commit:

``` bash
git add .
git commit -m "Initial commit"
git branch -M main
git push -u origin main
```

### 2. `remote origin already exists`

Check the remote:

``` bash
git remote -v
```

If the URL is incorrect and you want to replace it:

``` bash
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

### 3. `! [rejected] ... (non-fast-forward)`

**Meaning:** The remote has commits that your local branch does not
contain.

A common solution, when appropriate for your workflow, is:

``` bash
git pull --rebase origin main
git push
```

Resolve conflicts if prompted. Avoid `git push --force` as a quick fix:
it can overwrite remote history and affect other contributors.

### 4. `fatal: refusing to merge unrelated histories`

The local and remote repositories have separate starting histories. If
you intentionally want to combine them, see [When local and remote
histories are
unrelated](#when-local-and-remote-histories-are-unrelated).

### 5. `nothing to commit, working tree clean`

Git sees no new changes to commit. Check whether you saved your file and
whether you are in the correct directory:

``` bash
pwd
git status
```

On Windows PowerShell, use `Get-Location` if you prefer the PowerShell
command.

### 6. LF/CRLF warning on Windows

LF and CRLF are different line-ending styles. A warning about conversion
is often informational and does not by itself mean the commit or push
failed. Check the final result with:

``` bash
git status
```

### 7. A file is missing from GitHub

Check whether it is staged, committed, ignored, or excluded by
`.gitignore`:

``` bash
git status
git check-ignore -v path/to/file
```

Do not add secrets or large generated files just to make the status
clean.

## Using `.gitignore`

A `.gitignore` file tells Git which untracked files and directories to
ignore. Example for a Python project:

``` gitignore
# Python cache files
__pycache__/
*.py[cod]

# Virtual environments
.venv/
venv/

# IDE and operating-system files
.vscode/
.idea/
.DS_Store
Thumbs.db

# Local database files (ignore only if they should not be versioned)
*.db
*.sqlite3

# Local environment variables and secrets
.env
```

Adjust this example for your project. A database or data file may be an
important part of a project, so do not ignore it automatically if it
should be shared.

Important: `.gitignore` does not stop tracking a file that is already
tracked. If a tracked file should no longer be tracked but should remain
on your computer, you may use:

``` bash
git rm --cached path/to/file
git commit -m "Stop tracking generated file"
```

Review the path before running `git rm`.

## Good commit-message practices

Write short messages that explain what the commit changes.

Good examples:

``` text
Add Git command reference
Fix README setup instructions
Add SQL query for total runs by team
Resolve README merge conflict
Update Python project documentation
```

Avoid messages that say only `changes`, `update`, or `final` when a more
specific description is possible.

## A complete example workflow

This example assumes you have created a GitHub repository and have a
local project ready.

``` bash
# 1. Open your project directory
cd path/to/your-project

# 2. Initialize Git (only if this is not already a repository)
git init

# 3. Check files
git status

# 4. Stage the README
git add README.md

# 5. Create the first commit
git commit -m "Add project README"

# 6. Rename the current branch
git branch -M main

# 7. Add the GitHub remote (only if origin is not already configured)
git remote add origin https://github.com/USERNAME/REPOSITORY.git

# 8. Push and set upstream
git push -u origin main
```

If you already have a remote, do not add it again. Verify it with
`git remote -v`. If the GitHub repository already contains commits, you
may need to pull and integrate them before pushing.

For future changes:

``` bash
git status
git add README.md
git commit -m "Update project README"
git push
```

## Quick command reference

  -----------------------------------------------------------------------
  Command                             Purpose
  ----------------------------------- -----------------------------------
  `git --version`                     Check Git installation

  `git config --global --list`        View global configuration

  `git init`                          Initialize a repository

  `git clone URL`                     Copy a remote repository

  `git status`                        Check working-tree and staging
                                      status

  `git add FILE`                      Stage a file

  `git add .`                         Stage changes under the current
                                      directory

  `git commit -m "MESSAGE"`           Save staged changes in a commit

  `git log --oneline`                 View compact commit history

  `git diff`                          View unstaged changes

  `git diff --staged`                 View staged changes

  `git branch`                        List local branches

  `git switch -c NAME`                Create and switch to a branch

  `git switch main`                   Switch to `main`

  `git merge NAME`                    Merge a branch into the current
                                      branch

  `git remote -v`                     View remote URLs

  `git remote add origin URL`         Add a remote named `origin`

  `git pull`                          Fetch and integrate remote changes

  `git push`                          Upload local commits

  `git push -u origin main`           First push and set upstream

  `git restore --staged FILE`         Unstage a file

  `git revert COMMIT_ID`              Create a commit that reverses
                                      another commit

  `git stash`                         Temporarily save uncommitted
                                      changes

  `git stash pop`                     Reapply the latest stashed changes

  `git fetch`                         Download remote updates without
                                      integrating them

  `git branch -d NAME`                Delete a merged local branch
  -----------------------------------------------------------------------

## Practice exercises

Try these in a test repository so you can experiment safely.

1.  Create a folder and initialize Git inside it.
2.  Create a `README.md` file and add a few lines.
3.  Run `git status` before and after `git add README.md`.
4.  Commit the file with a clear message.
5.  Make another edit, inspect it with `git diff`, and commit it.
6.  Create a branch called `practice-branch`, edit a file, and commit.
7.  Switch to `main` and merge `practice-branch`.
8.  Create a GitHub repository, connect it as `origin`, and push.
9.  Change the README on GitHub, then practice pulling the change
    locally.
10. Practice resolving a merge conflict in a test repository.

## Final reminders

-   Check `git status` often.
-   Commit small, meaningful changes.
-   Read Git's error message before choosing a fix.
-   Pull and integrate remote changes when needed before pushing.
-   Do not force-push to solve ordinary push rejections.
-   Never commit passwords, API keys, tokens, or private credentials.
-   Use a test repository when learning destructive commands such as
    `reset --hard`.
-   A README should explain what a project does, how to set it up, how
    to run it, and how to use it.
