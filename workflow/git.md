# My git workflow

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
It will show current branch you on forked repository(origin) and the other's repository(upstream)
```
origin	git@version.aalto.fi:yans2/project.git (fetch)
origin	git@version.aalto.fi:yans2/project.git (push)
upstream	https://github.com/timbrist/project.git (fetch)
upstream	https://github.com/timbrist/project.git (push)
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