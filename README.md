# Summary

This project aims to leverage RStudio to comprehensively analyze field size data across Africa, incorporating uncertainty visualization and temporal trends.

## Objectives

Map Uncertainty in Field Sizes: We will create our own uncertainty score and create maps that visually represent this level of uncertainty that is associated with field size measurements at various locations throughout Africa.

Analyze Average Field Size by Region: Calculate and compare the average field sizes across different regions in Africa.

Track Average Field Size Trends: Use data to investigate how average field sizes have changed throughout Africa. We will analyze time series of locations to find the trends.

## Approach and Method

We will possibly use the packages below to gather the data, clean it and prepare it for spatial mapping/calculations. Then each of us will work on one of the three sections of the project.

Sfarrow package will be used to open geoparquet polygons as sf objects

Terra package to read, write, analyze and model our spatial data. As well as for Intersect, buffer, and creating vectors.

Ggplot2 for plotting

Sf for supporting spatial vector data, used to read write and convert projections.

Dplyr for filtering and manipulations

Readr to read csv

We can use tidyr in case we want to do any pivot_widers or csv manipulations

## Data/code

Our code will be split into three sections. We will need to map uncertainty throughout sites, find average field sizes by region, and find trends in the field sizes over several years.

# AFCFields

This is the repository for Clark University's Geospatial Analysis with R course (GEOG 246/346). The data for this project was provided by Professor Estes through a Google Drive.

## Installation (Steps to Copy Our Set Up)

The following set up is based on how we set this project. The following code assumes that you are following the steps to set up for Geog246/346 Geospaar package. 

The course materials being referenced to can be installed as a standard `R` package, using a desktop Rstudio installation (or another IDE), or within a `docker` container. For the standalone case, you can simply install the course package from your Rstudio Desktop (or similar IDE) installation (which assumes you have the `devtools` package installed), per step 7 below. 

The dockerized approach, which will be followed for the full class, provides a consistent environment, making it less susceptible to the idiosyncrasies of different operating systems. The same container environment will be used for developing `R` packages for class assignments and projects. More detail on working with `docker` can be found in the materials for [Advanced Geospatial Analysis with Python](https://hamedalemo.github.io/advanced-geo-python/lectures/docker.html), taught by Professor Alemohammad. For now we will just use it for installing course materials. Please follow these steps to get started. 

### 1. Get a GitHub account

If you don't already have one, please go to [github.com](https://github.com/) and sign up for a free account. 

If you are enrolled in this course, also get a [personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) for GitHub, which is necessary for undertaking assignments (which will be on submitted in a private repo established on your own GitHub account). Use the classic token rather than the newer fine-grained tokens. 

- Go into your GitHub account, and click settings, and then (on the left)  developer settings 
- Select personal access tokens
- Generate a new token (classic), name it something meaningful, and check the "repo" box
- Copy the token and paste it somewhere safe (e.g. a secure password manager) 

### 2. If you are a Windows users, install a Linux terminal emulator

- If not, skip to step 3

- If so, either install [`WSL`](https://learn.microsoft.com/en-us/windows/wsl/install), or [Git Bash](https://gitforwindows.org/). You can get away with Windows command prompt or Power Shell, but a *nix emulator is preferred.

- Either way, please **DO NOT** use the Windows command prompt or Power Shell for subsequent work (you are of course free to do so, but you will have to troubleshoot any problems that arise on your own). 

### 3. Install `docker`

Download and install the version of docker for your operating system from [here](https://www.docker.com/products/docker-desktop/), and create an account. Note: You can sign up with your Github credentials

### 4. Set up a project directory on your computer 

If you are taking this class, this will be the directory you use to install the class materials and your own assignment repositories/packages. Assuming you have a directory called something like `c:\My Documents\projects`, make a sub-folder called `geog246346`. Using your *nix terminal, navigate to it. 

```bash
cd c/My\ Documents/projects/geog246346
```

### 5. Clone the `geospaar` repository

  ```bash
  git clone https://github.com/agroimpacts/geospaar.git
  ```

### 6. Build or pull the `docker` image

Using docker, you can either build or pull the docker image you need by running the following in your terminal, assuming you have either a Mac with an Intel chip, or a Windows-based machine. If not, go to the section on Mac with M1/M2 chips. 

#### Intel-based Mac and Windows

You can either build a local image or pull a pre-built image:

- build (assuming you are in the project directory you made in step 4):

  ```bash
  cd geospaar
  LATEST=VERSION # replace VERSION with the latest version number, here 4.3.2
  docker build . -t agroimpacts/geospaar:$LATEST
  ```
  
- pull (this gets you the latest version already committed to docker hub):

  ```bash
  LATEST=VERSION # replace VERSION with the version number, currently 4.3.2
  docker pull agroimpacts/geospaar:$LATEST
  ```

#### Mac Silicon
For Macs with a Silicon chip (M1/M2/M3), it appears that your OS has to be 14.2 or higher to run the container, otherwise there are clashes with the Linux architecture that is used to build the image (which is linux/amd64, whereas M1/M2 wants to use linux/arm64). We have started an experimental build script for M1/M2 chips running OS <14.2, but this doesn't work yet, so for now we advise you to upgrade your OS, and then run the build command as:

  ```bash
  docker build --platform=linux/amd64 . -t agroimpacts/geospaar:$LATEST
  ```

If that still doesn't work, please install `R` and Rstudio desktop

#### 7. Run the container
After building, run the image using the following script that comes with the `geopaar` repo:

  ```bash
  PORT=8787 # this is the port to run on--you might want to change it
  MY_DIR=c/My\ Documents/projects/geog246346 # change to your directory!!!
  ./run-container.sh -v $LATEST -p $PORT $MY_DIR
  ```

Note: Make sure that MY_DIR is the path of the directory that your cloned `geospaar` is in. MY_DIR should not include "geospaar" at the end of the path, because then `docker` will try and mount the `geospaar` folder, which will cause problems (the script will fail). We want to mount the directory `geospaar` is in so that we can create other projects in the same directory as `geospaar` while we are working in the `docker` container. 
  
You can also (preferably) run it from your project directory containing `geospaar`, as follows:

  ```bash
  cd $MY_DIR # or, if you are in geospaar, you can one level up with cd ..
  ./geospaar/run-container.sh -v $LATEST -p $PORT `pwd`
  ```

Either approach to launching will give you a URL (https://localhost:8787) that you can copy and paste into your browser, which will then give you a fully functioning Rstudio-server instance after you log in. 

When you are finished with Rstudio server, you should stop the container:

  ```bash
  docker stop geospaar_rstudio
  ```
  
You can restart the container again with the same `./geospaar/run-container.sh ...` command you used previously.  

### 8. Additional GitHub configuration steps

Before installing the course package, there are a few more GitHub configuration steps you have to set up to set up your GitHub on your container-based Rstudio server (or your local) Rstudio desktop. These entail setting up ssh keys and adding them to your GitHub account. 

The instructions for setting those up are found [here](https://agroimpacts.github.io/geospaar/unit1-module1.html#using-git-and-github) in Unit 1, Module, specifically 4.1 on `git` configuration and 4.3 on syncing your first repository. 

Once you have completed those steps and confirmed you can access the remote repo of `geospaar`, you can install the package. 

For people launching the container from Git Bash, there are some additional steps that need to be followed:

1. First, make sure your project folder is fresh and empty, except for a new clone of `geospaar` 
2. If it isn't, the easiest and least risky is to make a new folder (e.g. geog246346b). Move into that folder, and run git clone again on the `geospaar` repo 
3. Make sure your an container is not running (`docker stop geospaar_rstudio`)
4. Run `docker image ls` and copy the image id for any existing `agroimpacts/geospaar:$LATEST` images. Then using that copied id, run `docker rmi <imageid>` (replace `<imageid>` with the id you just copied. That will remove the current image. 
5. Enter the geospaar folder (`cd geospaar`) to get into your newly cloned `geospaar`, and then rerun the docker build commands.  Then launch the container again
6. You are now in a fresh Rstudio server environment.  You should see that the "geospaar" and the "r_ver4.3.2_packages" folders are there. The Rstudio server interface should not be pointing at a new project 
7. Now, follow the steps mentioned at the top of this section: 
  - Configure your GitHub username and email in the terminal of Rstudio server
  - Then create an ssh key and add it to GitHub
8. Now, open the geospaar project using the new project dialog in Rstudio server. The opened project should show a `git` tab in the lower left pane of the IDE 
9. In the terminal in Rstudio server run the following commands:

  ```bash
  git config --global safe.directory '*'
  git config core.fileMode false
  ```
10. Next, run `git status`. It should show a bunch of files have been modified. If it does, run:
  `git stash`, followed again by `git status`. It should show no changes to the repo.  Try `git pull`, which should that you are be up to date. This should be working now


### 9. Project Data Input

After receiving access to the data. Download/Unzip into a folder called "afcdata".
In your project directory create the following file path with the data:
```
external/afcdata/
```
within afcdata there should be two folders, label and image, along with two csv files
and a parquet file. It is important to have the files;
'label_catalog_allclasses.csv', 'label_catalog_filtered.csv', and
'mapped_fields_final.parquet' all be under the afcdata folder. DO NOT create another
subfolder for them (this is okay to do, and preferred, for the images and labels).

### 10. Clone Our Repo

```bash
 https://github.com/HLed12/afcfields.git
  ```

 ### Special Thanks

We would like acknowledge the people who made this project possible. @ldemaz for providing the data, tools, and knowledge to make this project and @vanchy-li for her commitment to our success and the long hours of fixing our projects major issues. Thank you both! 

On the web:
Also thanks to @LLeiSong, the materials are also available through the [course website](https://agroimpacts.github.io/geospaar/).
