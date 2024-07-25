# My git workflow


## 

**Step 1:** Initiate the project on your local-machine from remote-repository
```bash
# Download your repository
git clone https://github.com/timbrist/gittest.git
cd gittest
```

**Step 2:** Branch out to new feature of the project

```bash
# Create a new branch.
git checkout -b feature-1

# Check all branches, 
git branch -a

# Do new feature in the project.
```

**Step 3:** Save the work you have done on local-machine

```bash
# show the changes
git status

# Stage the changes on a specific file 
git add <filename>

# show the difference of staged files
git diff --staged

# commit the staged files
git commit -m "Your commit message here"
```

**Step 4:** Save the work you have done on remote-repository
```bash
# submit the what you have done onto the remote repository(origin)
git push origin feature-1
```

**Step 5:** IMPORTANT: Merge your feature branch into main branch.
Only after you save your work into main branch, your work is done.

Go to the remote-repository, and click "Compare & pull request".


## Fork
![fork](https://timbrist.github.io/workflow/fork.png)

Fork other's repository before cloning, because the code will not always run on your computer or other's. 
```bash
# 1. get the repository from your account that fork from other's 
git clone https://github.com/<your-username>/<forked-repo>.git
cd <forked-repo>

# 2. Create a new branch.
git checkout -b <branch-name>
```



## to get the information

### to track the repository

```bash
git remote -v
```
It will show repository you forked(origin) and the other's repository(upstream)
```
origin	git@version.aalto.fi:yans2/project.git (fetch)
origin	git@version.aalto.fi:yans2/project.git (push)
upstream	https://github.com/timbrist/project.git (fetch)
upstream	https://github.com/timbrist/project.git (push)
```

### to track the local branches 

```bash
git branch -r
```
It will show current branch(origin/main) you are using(origin/HEAD) 
and the branch of other's repository(upstream/main)
```bash
origin/HEAD -> origin/main
origin/main     # this is the branch you are using, HEAD is just a pointer(like c/c++).
upstream/main
```

**case 1**: 
Project "A" on GitLab, which import from Project "B" on GitHub,
How to make a pull request when there is a update on Project "B". 


```bash
# 1. Clone your GitLab repository:
git clone <your-gitlab-repository-url>
cd <your-repo-name>

# 2. Add the GitHub repository as a remote:
git remote add upstream <github-repo-url>

# Fetch updates from the upstream repository:
git fetch upstream

# Merge updates into your local branch:
git checkout main  # or whatever branch you are tracking
git merge upstream/main  # or the appropriate branch

# ! Resolve Conflicts, 
# I like to do it on vscode. 

# Push Changes
git push origin main  # or the appropriate branch

```