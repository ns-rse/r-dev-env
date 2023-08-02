# R Dev Container

[![Project Status: WIP – Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/r-devel/r-dev-env)

A containerised development environment for editing and compiling the R source code. The environment contains the VSCode IDE and tools needed to compile R.

## Getting started

You can run this environment on GitHub using [codespaces](https://github.com/features/codespaces)

### Codespaces

Click on the 'Open in GitHub Codespaces' button and then click the green 'Create Codespace' button.

> You will see the message "Codespace usage for this repository is paid for by ...", with your username. Don't panic!
> 
> **Note : Github Codespaces offers 120 core hours of free usage per month for every Github user. So the actual number of free hours is 120 divided by the number of cores you are using to run your codespaces.**
>
> **Here for the R-dev-env codespace we have set the codespace usage to 4 cores which leads to 30hrs of free usage per month. And it can also be changed according to your preference.**
> 
> For more details about codespaces billing, see the [Codespaces Billing Docs](https://github.com/features/codespaces). You can calculate your GitHub services usage with the [GitHub Services Pricing Calculator](https://github.com/pricing/calculator) and check your usage allowance under "Codespaces" on https://github.com/settings/billing.
![image](https://github.com/r-devel/r-dev-env/assets/72031540/d42c5d89-7f1d-46fc-8fdd-44e03311c9b2)


The codespace setup screen will then be shown. Starting the container may take a minute or so.

<p align="center">
    <img src="screenshots/setting_up_codespace.png" width="80%">
</p>


You will be taken to a VSCode editor within your browser.

![image](https://github.com/r-devel/r-dev-env/assets/72031540/0597c261-a110-496c-86a4-9fb08f5dc34d)


## Running R

Create a file in VS Code ending with a .R extension. You can create new files by clicking on the new file icon in VS Code.
![image](https://github.com/r-devel/r-dev-env/assets/72031540/0597c261-a110-496c-86a4-9fb08f5dc34d)



Open the file by clicking on the filename. You should see R:(not attached) in the bottom bar.
![image](https://github.com/r-devel/r-dev-env/assets/72031540/e670f985-6e9b-4c34-b7e5-d1ca4e48ffe5)


Click on the R:(not attached) button to launch R in the terminal. You can then send code from the .R file to the R terminal by pressing cmd/ctrl + enter.
![image](https://github.com/r-devel/r-dev-env/assets/72031540/27fbceda-3f7a-4baa-ac07-95e772a392eb)


## R Contribution Workflow

### Build Setup (Without Recommended Packages)
1. Environment Variables
    - We have environment variables for setting the paths for building R and storing the source code.
    - The path ENV variables for R Build and R Source code are BUILDDIR and TOP_SRCDIR respectively.
    - The environment variables are set in the codespace image and are available when the codespace starts.
      
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/7e208955-5cc1-4761-95f5-20ba85575dd3)
2. svn checkout
   - The svn checkout cmd lets us create working of a repository with specific tag/branch.
   - Example:
      ```bash
       svn checkout https://svn.r-project.org/R/trunk/ "$TOP_SRCDIR"
      ```
   - Output : We get file structure something like this after checking out R source code from R svn repository.
     ![image](https://github.com/r-devel/r-dev-env/assets/72031540/4d5ec49c-4abe-4d2b-bdae-31f96404d6d4)

3. cd to BUILDDIR
   - We need to change our directory to R build directory(BUILDDIR) to build and configure our R source code.
   - First we will create a directory using env var BUILDDIR.
     ```bash
     mkdir -p $BUILDDIR
     ```
   - Then we can change directory from root to $BUILDDIR one.
     ```bash
      cd $BUILDDIR
     ```
4. configure source code
   - After we change directory to BUILDDIR we can configure and build R.
   - CMD
     ```bash
     "$TOP_SRCDIR/configure" --enable-R-shlib --without-recommended-packages
     make
     sudo make install
     ```
   - The configure cmd prepares for building R, creating files and folders inside the BUILDDIR directory.
   - Output : We get file structure something like this after using configure command.
     
     ![image](https://github.com/r-devel/r-dev-env/assets/72031540/0d4878fa-c1a8-462b-8365-76cc5dadf734)

5. After having built the current development version of R, we can now make changes in source code and make our contributions.

### Build Setup (With Recommended Packages)
This build setup differs from the above because the recommended packages for R are included.
1. svn checkout using cmd
   ```bash
       svn checkout https://svn.r-project.org/R/trunk/ "$TOP_SRCDIR"
   ```
2. Then we will create a directory using BUILDDIR env var.
   ```bash
   mkdir -p $BUILLDIR
   ```
3. Then we will install recommended packages using cmd
   ```bash
    "$TOP_SRCDIR/tools/rsync-recommended"
   ```
   ![Screenshot from 2023-07-28 15-18-22](https://github.com/r-devel/r-dev-env/assets/72031540/532a3f40-ab17-43b4-b729-ab57b2e3ffe9)
4. We can now change directory to $BUILDDIR using cmd
   ```bash
   cd $BUILLDIR
   ```
5. configure source code
   - After we change directory to BUILDDIR we can configure and build R.
   - CMD
     ```bash
     "$TOP_SRCDIR/configure" --enable-R-shlib
     make
     sudo make install
     ```
   - The configure cmd prepares for building R, creating files and folders inside the BUILDDIR directory.
   - Output : We get file structure something like this after using configure command.
     
     ![image](https://github.com/r-devel/r-dev-env/assets/72031540/0d4878fa-c1a8-462b-8365-76cc5dadf734)


### Contribution Workflow

1. Example Contribution Workflow using DevContainer:
   -  To start working in R we will click on `R:(not attach)` option which is in the bottom right of our R-dev codespace. It will open R terminal for us.
     
       ![image](https://github.com/r-devel/r-dev-env/assets/72031540/4ad3ed18-108a-4f29-ab6c-7f32d81721a7)
       ![image](https://github.com/r-devel/r-dev-env/assets/72031540/b3bdd3da-903d-4330-81c3-e41147d5dcd4)

   -  We can now run R commands. We will use the `utils::askYesNo()` function as an example
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/00ffb5cf-250b-49d9-ab37-4028ad708164)

      ```R
      > askYesNo("Is this a good example?")
      Is this a good example? (Yes/no/cancel) Yes
      [1] TRUE
      ```
 2. Edit the source code of `utils::askYesNo()` to change the default options. The source code can be found in `$TOP_SRCDIR/src/library/utils/R/askYesNo.R`.
    
     **File Opening tip in VSCode** : We are editing the `askYesNo.R` file which is located at `$TOP_SRCDIR/src/library/utils/R/askYesNo.R`.  To open this file on the command line, without searching through directories, you can run the below.
   
     ```bash
    code $TOP_SRCDIR/src/library/utils/R/askYesNo.R
     ```
    
    Before edit:
    ![image](https://github.com/r-devel/r-dev-env/assets/72031540/6e7f368a-7a71-457c-a08e-de0d1b3c476f)

    ```R
    prompts = getOption("askYesNo", gettext(c("Yes", "No", "Cancel"))),
    ```
    With edit (for example - change to whatever you like!):
    ![image](https://github.com/r-devel/r-dev-env/assets/72031540/b7476540-1030-4f88-ae3c-1c2f9dd90deb)

     ```R
      prompts = getOption("askYesNo", gettext(c("Oh yeah!", "Don't think so", "Cancel"))),
     ```
 4. Re-build the utils package (we only need to re-build the part we have modified). We can rebuild the package by following simple steps
    - First we need to be inside $BUILDDIR, for that we can change directory to `cd $BUILDDIR`.
    - After that we can run cmd `make` and `sudo make install` in a series.
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/e32f8b8f-c573-41e6-b4cc-31fb3494891a)

      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/709dd607-5d22-4b17-90ad-8f642ecad6b6)


    - This will re-build any parts of R that have changed, in this case only re-building the utils package, then re-install R. If we open a new R terminal we will see our changes getting reflected.
 5. Check the edit has worked as expected by re-running the example code:
    ![image](https://github.com/r-devel/r-dev-env/assets/72031540/97fcfee8-dae5-402c-8bf4-0df62a63c3b0)

    ```R
    > askYesNo("Is this a good example?")
    Is this a good example? (Oh yeah!/don't think so/cancel) Oh yeah!
    [1] TRUE
    ```

## Commiting Changes
For commiting changes using svn we need to first change directory to $TOP_SRCDIR from $BUILDDIR
1. `cd $TOP_SRCDIR` - Change Directory to $TOP_SRCDIR
2. `svn status` - We could see the changes made in file askYesNo.R
3. `svn update` - this command commits the changes made inside the file.
4. `svn diff > path.diff` - This create a path file which will have changes made inside the files.

### SVN help
While working with the R Contributors Workflow on Codespace, we might find some svn commands handy.
1. `svn status` - The svn status command is used to show the status of files and directories in your working copy. It displays information about the differences between your local copy and the repository, indicating which files are modified, added, deleted, or conflicted. 
2. `svn cleanup --remove-unversioned` - The svn cleanup --remove-unversioned command is a specific variation of the svn cleanup command with an additional option. This command is used to perform a cleanup operation on the working copy while also removing any unversioned files and directories.
3. svn update - The svn update command is used to bring your working copy up to date with the latest changes from the repository.

Example :  To build the R from source code we checkout we need to be inside the $BUILLDIR directory. But sometimes mistakely we might build the R inside the $TOP_SRCDIR
To roll back the changes made inside the $TOP_SRCDIR, we can use `svn cleanup --remove-unversioned`.

## Stopping and Restarting Codespaces
#### How to Stop Codespaces?
- To stop codespaces we just need to navigate to the Codespaces option in the bottom left of the Codespace panel.
   ![image](https://github.com/r-devel/r-dev-env/assets/72031540/6154aff9-2b46-44ab-aba7-4c454ef9d52d)
- After clicking on codespaces option we will get a drop down above something like this👇
  ![image](https://github.com/r-devel/r-dev-env/assets/72031540/9bac270a-63c9-44ed-aa43-ce9f37a754ca)
- Click on "Stop Current Codespace". It will stop the codespaces you are currently using or running.
- You will be redirected to a Restart Codespaces page. The page shows a link to restart the codespace you just stopped.
  ![image](https://github.com/r-devel/r-dev-env/assets/72031540/e87082b3-fcd4-4943-9301-1a219eb58bf8)

#### How to Restart Codespaces again?
  > The code changes and operations we have performed inside the codespace will still be inside the stopped codespace. Also, the codespace may have an inactivity time limit and close after 30 minutes. If your codespace is stopped then you can restart it as shown below.
- Lets see how we can restart the codespaces again.
- To restart codespaces again we can go to this link https://github.com/codespaces
- Here we can see list our codespaces we have created
  ![image](https://github.com/r-devel/r-dev-env/assets/72031540/23ae4b6e-70fb-4fb2-98ec-d89833804742)
- Currently, here we have one codespaces that we just stopped few seconds ago.
- To restart it, we can just click on the codespaces we wanted to use and it will start the codespaces again for us.
- You can also see an active label added to the codespaces we just started
  ![image](https://github.com/r-devel/r-dev-env/assets/72031540/ea0589f4-301c-4c65-bd72-0207c3c5e897)

#### Using Locally
 - We can also use this codespace locally. For that we need to have some prerequisites installed.
 - - Prerequisites : 
   1. Docker Engine or Docker Desktop. You can find the docker desktop install instructions [here](https://www.docker.com/products/docker-desktop/).
   2. VSCode Editor. You can install VSCode from [here](https://code.visualstudio.com/download).
 - Steps to run R Development Container locally
   1. Head to the GitHub page for the R de Github project link for R Development Container (https://github.com/r-devel/r-dev-env)
   2. Clone the repository locally or download the zip file
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/a978e4e6-f6a6-428e-8dd2-e18b3570906f)
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/d89a6e0a-4ef4-444b-9ef8-94f3a97bf194)
   3. Change Directory to r-dev-env and list all the branches using `git branch --all`
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/021dd155-912c-4052-9a39-1347df4f7559)
   4. We need to checkout to `devel` branch which has latest and updated config for the R-dev-Env project.
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/c9dfb222-1d98-42e6-a2d3-7d5a06a77e30)
   5. Now we can open this branch inside VSCode editor using cmd `code .`. This will redirect us to VSCode editor.
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/497f5e19-ef0d-417f-9f1f-f72b374e903a)
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/5fbca33d-093c-459b-825d-aafe2c4a3fc3)
   6. After this step please be sure that your docker engine is started. If you have installed Docker Desktop just open the Docker Desktop app the engine starts automatically and if you are using just docker engine make sure to start it with `systemctl start docker` cmd)
   7. We can see pop-up at the bottom right of the VSCode editor which says reopen in Dev Container. Click on 'Reopen in DevContainer' button.
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/5c29b955-972f-4a7c-bad8-2d8050b13b9d)
   8. After clicking on that button we will see our container is getting ready. It will take some time. So till that time you can have coffee :)
      ![image](https://github.com/r-devel/r-dev-env/assets/72031540/044d1e27-22a6-45df-82ec-8fb65abd75e8)
   9. We can also test the dev container is working or not by just printing the env variables we have mentioned inside container. And there we go!!! We have setup our R-dev-env Locally.
       ![image](https://github.com/r-devel/r-dev-env/assets/72031540/026668de-a9bb-49bc-a515-c16a218b685d)



      


   
## Useful Links

* [R in Visual Studio code](https://code.visualstudio.com/docs/languages/r)

* [VSCode R Wiki](https://github.com/REditorSupport/vscode-R/wiki)

* [Getting started with Dev Containers](https://code.visualstudio.com/docs/devcontainers/tutorial)

* [Install Docker Desktop](https://www.docker.com/products/docker-desktop/)

* [Installing Docker on Linux](https://docs.docker.com/desktop/install/linux-install/)

* [Dev Container Documentation](https://code.visualstudio.com/docs/devcontainers/containers)
