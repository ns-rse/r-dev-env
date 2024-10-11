This tutorial takes you through the various steps of making contributions to the
R code base

## 1. Attach to the R workspace

To start working in R we need to first attach to a workspace session of R. To do
this click on `R:(not attach)` at the bottom right of the VSCode window. This
will open an R terminal for us.

Figure: VSCode before attaching to an R Workspace session

![VSCode before attaching to an R Workspace session](../assets/rdev11.png)

![VSCode after attaching to an R Workspace session](../assets/rdev12.png)

We can now run R commands. We will use the `utils::askYesNo()` function as an
example

```R title="Running the existing askYesNo() function."
> askYesNo("Is this a good example?")
Is this a good example? (Yes/no/cancel) Yes
[1] TRUE
```

## 2. Editing Source Code

Edit the source code of `utils::askYesNo()` to change the default options. The
source code can be found in `$TOP_SRCDIR/src/library/utils/R/askYesNo.R` and
we want to edit line 20 where the `prompts` option is set.

You can redirect to that file using...

```bash
code $TOP_SRCDIR/src/library/utils/R/askYesNo.R
```

**Before edit:**

![askYesNo.R before editing](../assets/rdev20.png)

```R title="askYesNo.R before editing" linenums="20"
> prompts = getOption("askYesNo", gettext(c("Yes", "No", "Cancel"))),
```

**With edit (for example - change to whatever you like!):**

![askYesNo.R after editing](../assets/rdev21.png)

```R title="askYesNo.R after editing" linenums="20"
> prompts = getOption("askYesNo", gettext(c("Oh yeah!", "Don't think so", "Cancel"))),
```

## 3. Rebuild R

We are now ready to re-build R with our changes. Since we have only modified the
utils package, rebuilding R will only re-build the utils package.

Quit R with `q()` or by closing the R terminal and in the bash terminal change
directory th the build directory. This is stored in the environment variable
`$BUILDDIR` so we can...

```bash
cd $BUILDDIR
```

Now run the `make` command to rebuild R with the changes you made in step 2.
This will be much faster than the full build because only the changes we made to
the utils package are rebuild.

```bash
make
```

![Output rebuilding R using make](../assets/rdev22.png)

It's a good idea to check the changes you have made have not broken anything.
R comes with a test suite and you can run that on your local changes by
running `make check` to run R's test suite against your local changes. You may
skip this step while you are iterating on a bug fix or other development, until
you are ready to [create a patch](./patch_update.md) but remember to run the
tests before creating a patch.

To use the re-built R, simply open a new R terminal.

## 4. Cross check and Re-running Code

You can check the edit has worked as expected by in the re-built R by re-running
the example code:

![Example showing the modified output of askYesNo()](../assets/rdev23.png)

```R title="Example showing the modified output of askYesNo()"
    > askYesNo("Is this a good example?")
    Is this a good example? (Oh yeah!/don't think so/cancel) Oh yeah!
    [1] TRUE
```
