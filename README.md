# linux_eoan_rsetup
How to get R up and running with key packages in Linux 19.10

Setting up a new Linux based R and RStudio Set Up
This is notes to self for more quickly getting R up and running again.

Step 1: 

sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu eoan-cran35/'
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo apt update
sudo apt install r-base-dev

Step 2:

go to RStudio and download file, then use file manager to run the file. It should just work.

Step 3: Install minimum necessary packages

In the TidyVerse the key packages I currently find most necessary:
# Required Packages
sessionInfo()
# Install Packages

#tidyverse_pkgs <- c("tidyverse", "readxl", "haven", "jsonlite", "xml2", "httr", "rvest", "lubridate", "hms", "magrittr", "glue", "broom")
install.packages(tidyverse_pkgs)
library(tidyverse_pkgs)

NOTE:  It is very important to put quotes around the package names or else you will create a bunch of empty package folders with nothing in them and then get a lot of annoying errors.  You would then need to go through and use terminal to manually add all the missing blank package folders (like "haven" and "readr") -- Essentially without quotes it will try to do what you want and try to install the tidyverse but not succeed completely.

Step 4: 

If any of those fail, and you see weird things missing that give you error/fails, then open a terminal and start working down this list:

Open Terminal (ctrl-alt-t) and install:

sudo apt-get install libprotbuf-dev
sudo apt-get install libjq-dev
sudo apt-get install libudunits-dev
sudo apt-get install libV8-dev
sudo apt-get install libgdal-dev

Step 5:

Set up SSH Keys and Git Connections

1. open terminal and enter
sudo apt update
sudo apt install git

2. check version
git --version

3. SSH Keys

See if you have keys:

A. ls -al ~/.ssh/

B. then go to RStudio Global Options << tools << git << create ssh keys

C. Click “Create” and RStudio will generate an SSH key pair, stored in the files ~/.ssh/id_rsa and ~/.ssh/id_rsa.pub.

D. In terminal type: 

To add ssh-ask pass type:

sudo apt-get update
sudo apt-get install ssh-askpass

E. then check if running:

eval "$(ssh-agent -s)"
to make sure the ssh agent is running.

F. then in terminal type:

ssh-add ~/.ssh/id_rsa

This should store your credentials

Step 6. Add Keys to GitHub

1. go to GitHub
2. click on small profile pic on RIGHT. and got to settings. Security << add SSH << paste from the clipboard (since you used RStudio to view public key and copied it to clip board right???)

Step 7. Git Repo

1. create a new repo
2. copy the SSH Key from the Clone box
3. open RStudio and create new project with version control - git
4. paste the ssh url key

This should get you up and running!
