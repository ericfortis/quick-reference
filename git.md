# git

## Create Feature Branch
```shell
git checkout main
git checkout -b myfeature # Creates a branch
```

## Update Feature Branch
```shell
git checkout myfeature
git pull
git rebase main
```

## Merge Feature Branch
```shell
git checkout main
git merge foo
```


##  File history including renames
```shell script
git log -p --follow src/fileName.js
```

## Search for a deleted file
```shell script
git log --all --full-history -- 'src/**/fileName.*'
```

## Bisect
```shell script
git bisect start HEAD HEAD~10 --   # Check the last 10 commits
git bisect bad   # or good
git bisect reset 
```

## Clean
Clean directories deleted in another clone. This happens when
pulling, because that keeps any deleted directory (but empty).
```shell script
git clean -fd
```

## Squashing already pushed commits
Squashes the last 10 (e.g. working in `my-branch`)
```shell script
git rebase -i HEAD~10
git push origin +my-branch
```
then on the other clients
```shell script
git pull -r
```
https://stackoverflow.com/questions/5667884


## Remove branches of a non-existing remote
https://stackoverflow.com/a/19298897


## Repo Size
```shell
git gc
git count-objects -vH
```

## Show file content of another branch
Without having to checkout the other branch
```shell
git show <branch>:<file>
```

## Recover a deleted branch that's not in reflog
```shell
git fsck --full --no-reflogs --unreachable --lost-found | grep commit | cut -d\  -f3 | xargs -n 1 git log -n 1 --pretty=oneline > .git/lost-found.txt
```
https://stackoverflow.com/a/22303923