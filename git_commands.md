# Git – Beginner Notes

## 1. What is Git?

**Git** is a distributed version control system used to track changes in files and source code.

Git helps developers to:

* Track code changes
* Work with multiple developers
* Create different versions of a project
* Revert unwanted changes
* Create and manage branches
* Collaborate using platforms such as GitHub, GitLab and Bitbucket

### Git vs GitHub

| Git                   | GitHub                                      |
| --------------------- | ------------------------------------------- |
| Version control tool  | Cloud-based Git hosting platform            |
| Runs on your computer | Runs mainly on the internet                 |
| Tracks code changes   | Stores and collaborates on Git repositories |
| Command-line tool     | Web interface + Git hosting                 |
| Example: `git commit` | Example: GitHub repository                  |

---

# 2. Install Git

Check whether Git is installed:

```bash
git --version
```

Example:

```text
git version 2.51.0
```

---

# 3. Git Configuration

Before using Git, configure your username and email.

### Set username

```bash
git config --global user.name "Naveen Kumar"
```

### Set email

```bash
git config --global user.email "your-email@example.com"
```

### Check configuration

```bash
git config --list
```

You can check individual values:

```bash
git config --global user.name
git config --global user.email
```

---

# 4. Important Git Concepts

A Git project generally works with these areas:

```text
Working Directory
       |
       | git add
       v
Staging Area
       |
       | git commit
       v
Local Repository
       |
       | git push
       v
Remote Repository
     (GitHub)
```

### Working Directory

The files you are currently working on.

### Staging Area

Files that you have selected to include in the next commit.

### Local Repository

The Git repository stored on your computer.

### Remote Repository

A repository hosted on a remote platform such as GitHub.

---

# 5. Initialize a Git Repository

Create a project directory:

```bash
mkdir my-project
cd my-project
```

Initialize Git:

```bash
git init
```

Output may look like:

```text
Initialized empty Git repository
```

Git creates a hidden `.git` directory.

---

# 6. Check Git Status

The most commonly used Git command:

```bash
git status
```

It shows:

* Modified files
* Untracked files
* Staged files
* Current branch
* Other repository information

Example:

```text
On branch main

Untracked files:
  app.py
```

---

# 7. Create a File

Example:

```bash
echo "Hello Git" > README.md
```

Check the status:

```bash
git status
```

Git will identify `README.md` as an **untracked file**.

---

# 8. Git Add

`git add` moves changes from the working directory to the staging area.

### Add one file

```bash
git add README.md
```

### Add multiple files

```bash
git add README.md app.py
```

### Add all files

```bash
git add .
```

Check:

```bash
git status
```

The file should now appear under:

```text
Changes to be committed
```

---

# 9. Git Commit

A commit saves the staged changes into the local Git repository.

```bash
git commit -m "Add README file"
```

The `-m` option is used to provide a commit message.

Example:

```bash
git commit -m "Initial commit"
```

### Good commit messages

```text
Add login page
Fix database connection
Update README
Add Dockerfile
Configure Jenkins pipeline
```

Avoid unclear messages such as:

```text
changes
update
test
abc
```

---

# 10. Git Log

View commit history:

```bash
git log
```

For a shorter version:

```bash
git log --oneline
```

Example:

```text
a45b21c Add Dockerfile
8d921ab Update README
31f4c20 Initial commit
```

---

# 11. Git Diff

`git diff` shows changes that have not been staged.

```bash
git diff
```

After running:

```bash
git add .
```

use:

```bash
git diff --staged
```

to see staged changes.

---

# 12. Git Branch

A **branch** allows you to work on a separate version of the project.

List branches:

```bash
git branch
```

Example:

```text
* main
```

Create a branch:

```bash
git branch feature-login
```

Switch to a branch:

```bash
git switch feature-login
```

Create and switch to a branch:

```bash
git switch -c feature-login
```

---

# 13. Branch Workflow

Example:

```text
main
 |
 |---- feature-login
 |
 |---- feature-payment
 |
 |---- feature-dashboard
```

Different developers can work on different branches.

Example:

```bash
git switch -c feature-login
```

Make changes:

```bash
git add .
git commit -m "Add login feature"
```

---

# 14. Merge

After completing work on a feature branch, you can merge it into another branch.

Switch to `main`:

```bash
git switch main
```

Merge the feature:

```bash
git merge feature-login
```

Example:

```text
feature-login
      |
      | merge
      v
     main
```

---

# 15. Clone a Repository

To download an existing Git repository:

```bash
git clone <repository-url>
```

Example:

```bash
git clone https://github.com/example/project.git
```

Then:

```bash
cd project
```

Check:

```bash
git status
```

---

# 16. Git Remote

A remote is a connection to a remote Git repository.

Check remote repositories:

```bash
git remote -v
```

Example:

```text
origin  https://github.com/user/project.git (fetch)
origin  https://github.com/user/project.git (push)
```

`origin` is the default name commonly used for the remote repository.

---

# 17. Git Push

`git push` uploads local commits to a remote repository.

```bash
git push
```

For the first push of a new branch:

```bash
git push -u origin main
```

For a feature branch:

```bash
git push -u origin feature-login
```

After setting the upstream branch, you can normally use:

```bash
git push
```

---

# 18. Git Pull

`git pull` downloads changes from the remote repository and integrates them into your current branch.

```bash
git pull
```

Example:

```text
GitHub
   |
   | git pull
   v
Local Repository
```

---

# 19. Git Fetch

`git fetch` downloads information about changes from the remote repository without automatically merging them.

```bash
git fetch
```

Difference:

```text
git fetch
    ↓
Download remote changes
    ↓
Does NOT automatically merge


git pull
    ↓
Fetch remote changes
    ↓
Integrate changes
```

---

# 20. Git Restore

Discard changes made to a file:

```bash
git restore filename
```

Example:

```bash
git restore app.py
```

This removes unstaged changes from the file.

### Unstage a file

If you accidentally run:

```bash
git add app.py
```

you can remove it from staging:

```bash
git restore --staged app.py
```

---

# 21. Git Stash

Sometimes you have unfinished work but need to switch branches.

Use:

```bash
git stash
```

Your changes are temporarily stored.

Check stashes:

```bash
git stash list
```

Restore the latest stash:

```bash
git stash pop
```

Example:

```text
Working changes
      |
      | git stash
      v
   Stash
      |
      | git stash pop
      v
Working changes restored
```

---

# 22. Git Reset

`git reset` can be used to move changes or commits back.

### Unstage a file

```bash
git reset HEAD filename
```

Modern alternative:

```bash
git restore --staged filename
```

### Reset the latest commit

```bash
git reset --soft HEAD~1
```

This removes the commit but keeps the changes staged.

> Be careful with `git reset --hard` because it can permanently remove local changes.

---

# 23. Git Tag

Tags are commonly used to mark important versions.

Create a tag:

```bash
git tag v1.0
```

List tags:

```bash
git tag
```

Push a tag:

```bash
git push origin v1.0
```

Example:

```text
v1.0
v1.1
v2.0
```

---

# 24. .gitignore

`.gitignore` tells Git which files should not be tracked.

Create:

```text
.gitignore
```

Example:

```text
*.log
.env
node_modules/
__pycache__/
*.tmp
```

For example, if you have:

```text
.env
```

inside `.gitignore`, Git will ignore that file.

This is useful for:

* Passwords
* API keys
* Environment files
* Log files
* Temporary files
* Dependency directories

> Never commit passwords, private keys, API tokens or other secrets to Git repositories.

---

# 25. GitHub Workflow

A common Git + GitHub workflow is:

```text
Developer
    |
    | Edit files
    v
Working Directory
    |
    | git add
    v
Staging Area
    |
    | git commit
    v
Local Repository
    |
    | git push
    v
GitHub Repository
```

---

# 26. Basic Git Workflow

The most important workflow to remember:

```bash
git status

git add .

git commit -m "Update project"

git push
```

Before starting work:

```bash
git pull
```

So a typical workflow is:

```bash
git pull

# Make changes

git status
git add .
git commit -m "Add new feature"
git push
```

---

# 27. Complete Example

Let's create a simple project.

### Step 1 – Create directory

```bash
mkdir git-demo
cd git-demo
```

### Step 2 – Initialize Git

```bash
git init
```

### Step 3 – Create a file

```bash
echo "My Git Project" > README.md
```

### Step 4 – Check status

```bash
git status
```

### Step 5 – Stage the file

```bash
git add README.md
```

### Step 6 – Commit

```bash
git commit -m "Initial commit"
```

### Step 7 – Check history

```bash
git log --oneline
```

---

# 28. Connecting Local Repository to GitHub

Create a repository on GitHub.

Then add the remote:

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

Check:

```bash
git remote -v
```

Rename the branch to `main` if required:

```bash
git branch -M main
```

Push:

```bash
git push -u origin main
```

---

# 29. Common Git Commands Cheat Sheet

| Command         | Purpose                        |
| --------------- | ------------------------------ |
| `git --version` | Check Git version              |
| `git config`    | Configure Git                  |
| `git init`      | Create a Git repository        |
| `git clone`     | Download a repository          |
| `git status`    | Check repository status        |
| `git add`       | Stage changes                  |
| `git commit`    | Save changes                   |
| `git log`       | View commit history            |
| `git diff`      | View changes                   |
| `git branch`    | Manage branches                |
| `git switch`    | Switch branches                |
| `git merge`     | Merge branches                 |
| `git remote`    | Manage remote repositories     |
| `git fetch`     | Download remote changes        |
| `git pull`      | Download and integrate changes |
| `git push`      | Upload commits                 |
| `git restore`   | Restore/discard changes        |
| `git stash`     | Temporarily store changes      |
| `git tag`       | Create version tags            |
| `git reset`     | Reset changes/commits          |

---

# 30. Git Commands You Should Learn First

For beginners, focus on these commands first:

```bash
git --version
git config
git init
git clone
git status
git add
git commit
git log
git branch
git switch
git merge
git remote
git pull
git push
git fetch
git restore
git stash
```

### Most important 5 commands

```bash
git status
git add .
git commit -m "message"
git pull
git push
```

Remember the basic flow:

```text
                git add
Working -----------------> Staging
                            |
                            | git commit
                            v
                       Local Repository
                            |
                            | git push
                            v
                       GitHub
```

---

# 31. Practice Task

Create a Git repository for a simple project.

### Requirements

1. Create a directory named `student-project`.
2. Initialize it as a Git repository.
3. Create a `README.md` file.
4. Add the file to staging.
5. Commit the file.
6. Create a branch called `development`.
7. Switch to the `development` branch.
8. Add another file called `app.txt`.
9. Commit the new file.
10. Push the project to GitHub.
11. Check the commit history using `git log --oneline`.

### Expected basic workflow

```bash
mkdir student-project
cd student-project

git init

echo "Student Project" > README.md

git status
git add README.md
git commit -m "Initial commit"

git switch -c development

echo "My application" > app.txt

git add .
git commit -m "Add application file"

git log --oneline
```

---

# Quick Revision

```text
Git
 |
 +-- Working Directory
 |       |
 |       +-- git add
 |
 +-- Staging Area
 |       |
 |       +-- git commit
 |
 +-- Local Repository
         |
         +-- git push
                 |
                 v
             GitHub
```

**Remember:**

`git add` → Prepare changes

`git commit` → Save changes locally

`git push` → Send changes to GitHub

`git pull` → Get changes from GitHub

`git status` → Check what's happening


# Setup
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Start / download a repository
git init                         # Initialize a Git repo
git clone <repo-url>             # Clone an existing repo

# Check what's happening
git status                       # Show changed/staged files
git log                          # Show commit history
git log --oneline                # Compact history

# Stage & commit
git add file.txt                 # Stage one file
git add .                        # Stage all changes
git commit -m "Your message"     # Commit staged changes

# Branches
git branch                       # List branches
git branch feature               # Create branch
git switch feature               # Switch branch
git switch -c feature            # Create + switch
git merge feature                # Merge feature into current branch

# Remote repositories
git remote -v                    # Show remotes
git fetch                        # Download remote changes
git pull                         # Fetch + integrate changes
git push                         # Upload your commits
git push -u origin main          # First push + set upstream

# See changes
git diff                         # Unstaged changes
git diff --staged                # Staged changes
git show                         # Show latest commit

# Undo common mistakes
git restore file.txt             # Discard unstaged changes
git restore --staged file.txt    # Unstage a file
git commit --amend               # Modify latest commit

# Temporarily save work
git stash                        # Stash uncommitted changes
git stash pop                    # Restore latest stash