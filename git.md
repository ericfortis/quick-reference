# git

## Clone without history
```shell
git clone --depth 1 repo
```

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

## Rename all ".js" -> ".mjs" (recursively)
```shell
for i in $(find . -iname "*.js"); do
  git mv "$i" "$(echo $i | rev | cut -d '.' -f 2- | rev).mjs";
done;
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


## Patches
Create a patch of the last commit
```shell

git format-patch -1
```

Apply a patch
```shell
git apply --stat my.patch
git apply --check my.patch
git apply my.patch
```

## Prevent commits to `main`
https://stackoverflow.com/a/40465455

```sh
cat << EOF > .git/hooks/pre-commit
#!/bin/sh

branch="\$(git rev-parse --abbrev-ref HEAD)"
if [ "\$branch" = "main" ]; then
  echo "Unauthorized: You can't commit directly to the main branch"
  exit 1
fi
EOF
chmod +x .git/hooks/pre-commit
```