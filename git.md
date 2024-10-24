# Git

## Soft reset to the previous commit

```shell
git reset --soft HEAD~1
```

## Hard reset to the previous commit

```shell
git reset --hard HEAD~1
```

## Reset branch and sync with origin

```shell
git fetch --all
git reset --hard origin/<BRANCH>
```

## Squash the last 2 commits

```shell
git reset --soft HEAD~2
git commit -m '<MESSAGE>'
```

## Cherry pick a commit

```shell
git cherry-pick -x <COMMIT>
```

## Delete branch

```shell
# Delete local branch
git branch --delete <BRANCH>

# Delete remote branch
git push --delete <REMOTE> <BRANCH>
```

## Delete tag

```shell
# Delete local tag
git tag --delete <TAG>

# Delete remote tag
git push --delete <REMOTE> <TAG>
```

## Show content of last stash

```shell
git stash show -p
```

## Don't use the system keychain for this repository

```shell
git config --local credential.helper ""
```

## Reset the author of all the commits

```shell
git rebase --root --exec 'git commit --amend --no-edit --reset-author'
```

## List tags type

```shell
git for-each-ref refs/tags
```

Tag type:
- lightweight: `commit`
- annotated: `tag`

## Force purge of orphan commits

```shell
# Tag dangling commits (including stash) as expired
git reflog expire --expire-unreachable=now --all
# Run garbage collector
git gc --prune=now --aggressive
```

## Update remote HEAD 

```shell
git remote set-head <REMOTE> -a
```
