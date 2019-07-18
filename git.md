# Git

## Fixup and autosquash

In git some commits can be marked as fixup of others, that can be very helpful
for drafting commits without going through a complex `git rebase` afterwise.

This feature can be used suchwise:
```bash
# Create a commit
git add -p file1 file2
git commit -m "Create new feature"  # Commit id: b3dd96c

# Oops we forgot to add a file
git add file2
git commit --fixup b3dd96c
```

The commit log looks like that:
```
$ git log --online
af423b9 fixup! Create new feature
b3dd96c Create new feature
324f2cb Previous commit
```

The commit can now be squashed using `--autosquash`:
```bash
git rebase --interactive --autosquash 324f2cb
```

One caveat of `fixup` is that it needs a commit id, which is not confortable
for daily use, the following git alias allow to fixup tags dans branch names
like `git fixup HEAD`:

```
[alias]
fixup = !sh -c 'SHA=$(git rev-parse $1) \
	&& git commit --fixup $SHA \
	&& GIT_SEQUENCE_EDITOR=true git rebase -i --autosquash $SHA~' -
```

Sources:

* https://fle.github.io/git-tip-keep-your-branch-clean-with-fixup-and-autosquash.html
* https://twitter.com/tiste/status/1107587553383907328
