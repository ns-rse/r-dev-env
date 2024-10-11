The R Core Team commit changes to the development version of R sometimes
multiple times a day. It's a good idea to update your local copy of the source
code from time to time, especially before creating a patch. To do so, follow
these steps:

## 1. Close R terminal

If you have an R terminal open, quit R (`q()`) or close the terminal.

## 2. Review local changes

Use the Subversion diff command to review changes you have made to source code

```bash title="Reviewing local changes"
cd $TOP_SRCDIR
svn diff
```

## 3. Revert changes (optional)

If you no longer want to keep your local changes, you can revert them.

This can be done on a specific file

```bash title="Reverting changes to a specific file"
svn revert src/library/utils/R/askYesNo.R
```

...a whole directory...

```bash title="Reverting all changes in a specific directory"
svn revert src/lib/utils
```

...or all local changes

```bash title="Reverting all local changes"
svn revert -R .
```

## 4. Rebuild and check with any local changes

If you have no local changes remaining, skip to the next step.

Otherwise, go to the build directory to build and check R with your local
changes.

```bash title="Checking local changes pass tests"
cd $BUILDDIR
make
make check
```

If the check fails with an error, you have broken something with your local
changes. Fix this before proceeding. Otherwise go back to the source directory
to continue

```bash title="Changing directory"
cd $TOP_SRCDIR
```

## 5. Update using svn

Use the Subversion command `update` to update your local copy with the latest
changes by the R Core Team.

```bash title="Updating"
svn update
```

## 6.  Rebuild and check with the updates

To rebuild R with the latest changes from the R Core Team and any local changes
you have kept, go to the build directory to build and check R

```bash title="Running checks against the latest core changes"
cd $BUILDDIR
make
make check
```

If the check fails, this will be due to recent changes made by the R Core
Team. See [SVN Help](./svn_help.md) for how to revert to a version that passes
check.
