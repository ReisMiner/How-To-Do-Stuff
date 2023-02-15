# How To Undo Git Push.md


## Method to keep local changes

```bash
git push -f origin HEAD^:master
```

## Method to also revert local files

```bash
git reset --hard HEAD~1
git push origin -f
```

## Method to remove commit and keep local changes

```bash
git reset --soft HEAD~1
git push origin -f
```
