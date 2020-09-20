# Git

```
git init
git remote add origin <url>
git add .
git commit -m "message"
git push origin master
```

[Git Cheatsheet](https://gist.github.com/hofmannsven/6814451)

[Git Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows)

## Branch

### create branch

`git checkout -b new-branch-name`

### view all branches

`git branch`

### switch branch

`git switch branch-name`

### push branch

First time:

`git push --set-upstream master <remote-name> new-branch <branch-name>`

### delete branch

`git branch -d node-express-react`

or

_`git branch -D node-express-react`_

### Manage 2 branches at 2 different local places

pull remote to local repo:

`git pull <remote_name> <branch_name>`

local repo will be updated

## [Restore an old version](https://www.git-tower.com/learn/git/faq/restore-repo-to-previous-revision) - [or this](https://stackoverflow.com/questions/4114095/how-do-i-revert-a-git-repository-to-a-previous-commit)

### checkout commit log

`git log`

### quit log

`q`

### restore HEAD to older version

`git reset --hard <commit_id>`

### restore to older version as a branch

`git checkout -b <commit_id>`

## [Workflow Types](https://www.atlassian.com/git/tutorials/comparing-workflows)

- Centralized Workflow

- Feature Branch Workflow

- Forking Workflow

## Errors

- Merge Conflict: when multiple versions of the same file or line of code is committed differently.

  - Find problem

    ```
    git status
    ```

    (Tools: "dedicated merge tool", "Kaleidoscope.app")

  - After conflict is resolved, run

    ```
    git add <filename>
    ```

  - Return to the state before the merge:

    ```
    git merge --abort"
    ```

- Error: Browser still use cache pages after git push (source code update)
  - Dev Console -> Network -> Disable Cache
  - Dev Console -> Application -> Clear Storage
