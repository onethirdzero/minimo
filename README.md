A fork of minimo used to apply custom patches on top of the upstream theme.

## Installation

This repo gets installed as a submodule.

```
git submodule add https://github.com/onethirdzero/minimo themes/minimo
git submodule init
git submodule update
```

## How to add new custom patches

All custom changes are committed to the `custom` branch.

```
$ git checkout custom

# Make changes and commit.
```

These changes are then cherry picked onto the `master` branch. This is so that the custom changes can be easily rebased each time we need to pull updates from upstream.

```
$ git checkout master
$ git cherry-pick -x custom
```

Push your changes to `origin`.

```
$ git push
```

## How to pull updates from this fork to the main repo

The submodule in the main repo will see this repo as its `origin`. We'll need to remove the patches from its `master` so that it can cleanly pull from its `origin`.

```
$ cd <path to submodule in main repo>
$ git reset --hard <parent of earliest custom patch>
$ git pull --rebase
```

The pulled changes on `master` will already contain the custom patches on its tip.

## How to pull updates from upstream

Make sure that the most recent `master` commits match the ones on `custom`. Then, reset `master` to parent of the earliest custom patch.

```
$ git checkout master
$ git reset --hard <parent of earliest custom patch>
```

Pull from upstream.

```
$ git pull upstream master --rebase
```

Rebase `custom` onto the tip of the updated `master`.

```
$ git checkout custom
$ git rebase master
```

Lastly, re-cherry-pick the patches from `custom` onto master.

```
$ git checkout master
$ git cherry-pick -x custom
```

Push your changes to `origin`.

```
$ git push -f origin master
```
