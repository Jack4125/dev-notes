---
title: "Git"
tags: "branching, workflow"
---

# Git

```
git init
git remote add origin <url>
git add .
git commit -m "message"
git push origin master
```

[Git Cheatsheet](https://gist.github.com/hofmannsven/6814451)

[Complete Git Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows)

## Branch

### create branch

`git checkout -b new-branch-name`

### view all branches

`git branch`

### switch branch

`git switch branch-name`

### push branch

`git push --set-upstream remote-name branch-name`

### delete branch

`git branch -d node-express-react`

or

_`git branch -D node-express-react`_

### Manage 2 branches at 2 different local places

pull remote to local repo:

`git pull <remote_name> <branch_name>`

local repo will be updated

once work is done, push

## [Restore an old version](https://www.git-tower.com/learn/git/faq/restore-repo-to-previous-revision) - [or this](https://stackoverflow.com/questions/4114095/how-do-i-revert-a-git-repository-to-a-previous-commit)

### checkout commit log

git log

### quit log

q

### restore HEAD to older version

git reset --hard 0ad5a7a6

### restore to older version as a branch

git checkout -b 0ad5a7a6

## Workflow Types

### Centralized Workflow

1.  Clone or update from remote master the source code into local machine.
    - If new work, use clone:
      ```
      git clone <url> <file-name>
      ```
      (cloning automatically adds 'origin'?)
    - If already exist, update:
      ```
      git update
      ```
2.  Make changes at local copy
    - usually means working on code AND automated tests ( [see testing](https://martinfowler.com/bliki/SelfTestingCode.html) )
    - add and commit together:
      ```
      git commit -am "<msg>"
      ```
3.  Once _locally_ production ready, pull from remote master

    ```
    git pull --rebase origin master
    ```

    update current HEAD branch with latest changes from remote.

    _git pull = git fetch & git merge_
    [See this article](http://www.ranjeetvimal.com/difference-git-pull-git-fetch-git-clone/) and [this](https://stackoverflow.com/questions/21756614/difference-between-git-merge-origin-master-and-git-pull). Since git-pull merges automatically, it is recommended to use it on "clean working copy" - as in newly cloned, not yet changed or edited. (Weird, since the point of cloning is to make changes...?)

    - If after update, test still shows no error, then commit.
    - If after update, error shows up, then repeat from step 2.
    - Once there is no more error, push up to repository.

4.  (Optional?) Test or build again on integration machine after upload, incase problems occur during file upload or other weird issues. (Automated software: "Cruise")

### Feature Branch Workflow

- uses "pull requests"

### Forking Workflow

- Mainly in public open source
- 3 main repositories: central repository, individual coder's own server-repository, and own local-repository.
- Developers push to their own server-side repositories, and only the project maintainer can push to the official repository. This allows the maintainer to accept commits from any developer without giving them write access to the official codebase.

## Errors

- Merge Conflict: Merge conflict occurs when multiple versions of the same file or line of code is committed differently. This will either show up during merging, or after merge with

  ```
  git status
  ```

  when trying to fix the conflict, talking to the other team member about why they wrote their code that way may be necessary.

  (Tools: "dedicated merge tool", "Kaleidoscope.app")

  After conflict is resolved, run

  ```
  git add <filename>
  ```

  again, and when there is no more errors, commit again.

  To return to the state before the merge:

  ```
  git merge --abort"
  ```

- Browser still use cache pages after git push (source code update):
  - Dev Console -> Network -> Disable Cache
  - Dev Console -> Application -> Clear Storage
