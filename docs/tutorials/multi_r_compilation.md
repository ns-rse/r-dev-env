The [Building R](./building_r.md) and [R Contribution Workflow](./building_r.md)
tutorials use the simplest set up, working on a single copy of the R sources.

It can be helpful to work with multiple versions of R:

- to compare your modifications with an unmodified copy, e.g. to compare speed
    or memory usage, or
- to work on independent changes, so that each patch only contains the changes
    for one bug fix or new feature.

You can build multiple R versions in the same Codespaces environment.

## 1.Choose a name

First choose a name for the R version. This will be used to identify the
`r-devel` and the environment variables `$BUILDDIR` and `$TOP_SRCDIR` are set
to match.

For illustration, we will use `r-devel-raw`, which you might use to name a
version of R that you never modify.

## 2. Set environment variables

You can set the `$BUILDDIR` and `$TOP_SRCDIR` environment variables to match
your chosen name using the `set_build_r` function:

Open a terminal in the codespace and run the `set_build_r` function with your
chosen name as the argument. The new values of the environment variables will be
printed as confirmation, for example...

```bash title="Updating $BUILDDIR and $TOP_SRCDIR"
set_build_r r-devel-raw
BUILDDIR is now set to /workspaces/r-dev-env/build/r-devel-raw
TOP_SRCDIR is now set to /workspaces/r-dev-env/svn/r-devel-raw
```

## 3. Synchronise builds

If you have an unmodified build of R-devel using the default name of
directories with `rsync`:

```bash title="Synchronising builds."
rsync -a "$(dirname "$BUILDDIR")/r-devel/"* $BUILDDIR
rsync -a "$(dirname "$TOP_SRCDIR")/r-devel/"* $TOP_SRCDIR
```

Otherwise you can follow the steps in the [Building R](./building_r.md)
tutorial to checkout the R sources and build R using the new source and
build directories.

## 4. Check your build is available

Once you have a build of R under the new build directory, you will see your
chosen name in the choices when running the `which_r` script to select the
version of R to run in new terminals, e.g.

```bash title="Possible versions of R that are available shown by which_r"
which_r
Which version of R should be used in new R terminals?
1. R 4.4.0 (release version built into this container)
Additional R builds available:
2. r-devel
3. r-devel-raw
    Enter the number corresponding to the selected version:
```

<!-- markdownlint-disable MD046 -->
!!! Note
    `$BUILDDIR` and `$TOP_SRCDIR` will be set to the defaults using the label
    `r-devel` whenever a new bash terminal is opened, e.g. when the codespace is
    restarted.

    Whenever following instructions that use these variables to refer to the build
    and source directory, be sure they are pointing to the desired version!

    You can check the values any time with

    ```bash
    echo $BUILDDIR
    echo $TOP_SRCDIR
    ```

    and switch with `set_build_r <name>`.
<!-- markdownlint-enable MD046 -->
