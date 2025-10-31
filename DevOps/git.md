# GIT : 

Git is tool used for VSC, SCM 
- VCS - Version Control System.
- SCM - Source Control Management
- DVCS - Distributed Version Control System

### Basic commands
```
git status


git add <path/filename> ..
git commit -m 'message'
git push

git stash
git stash pop

git diff <path/filename>

git checkout -- .
# revert last commit before push 
git reset HEAD~1
```

### Setup
-
```
git config --global user.name "Preetam Patil"
git config --global user.email ""
```
- For new project
```
git init
```

### Create branch locally and push to remote repository
```
git checkout -b <New Branch Name>
git push -u <remote-branch: origin/master> <local-branch>
# if want to set upstream branch manual, It is not required if created branch correctly
git branch --set-upstream-to=origin/<branch> <local-branch>
```

### Add, Commit, push 
```
git add <path/filename> ..
git commit -m 'message'
git push
```

### Amend added changes to last pushed commit 
```
git add <path/filename>
# --no-edit : dont change last commit message , -m : update last commit message
git commit --amend --no-edit
git push
```

## Rebase current branch with origin branch and push to remote repository
```
git pull
git rebase <origin/master>
```
> Resolve conflict if any and add to staging 
```
git rebase --continue
git push --force-with-lease
```

## Merge current branch with origin branch and push to remote repository - Not recommended
```
git pull
git merge <origin/master>
```
> Resolve conflict if any and add to staging 
```
git merge continue
git push --force-with-lease
```

## Combine commits 
-
```
git rebase -i HEAD~<Count-of-commits-to-be-combined>
```
- It will populate editor, Replace 'pick' with 's' for commit we want to squash. one commit should have 'pick' which will be final/combined commit
- Another editor will be populated where we need to remove all commits which are squashed 
-
```
git push --force-with-lease
```

## Go to earlier commits 
- search for head {eq:head01} where you want to move 
```
	git reflog
```
- 
```
git reset --hard HEAD@{101}
git push --force-with-lease
```


### Delete branch
- Local 
```
git branch -d <branch>
```
- Remote
```
git push <remote> --delete <branch>
```

### Logs
```
git log 
# Options
--oneline
--since , --until, --grep= 

```

### Diff
```
git diff
git diff commitId1..commitId2 
# highlight changes by word rather than by line
git diff --color-words
git show --color-words 94ff81a
```
