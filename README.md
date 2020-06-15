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

NOTE: run the following to install a messed up package: 

in RStudio

install.packages("Rcpp", dependencies=TRUE, INSTALL_opts = c('--no-lock'))

answer found at:
https://askubuntu.com/questions/1163130/permission-denied-while-installing-r-package?noredirect=1&lq=1

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


A couple of links where I found info:

https://happygitwithr.com/ssh-keys.html
https://stackoverflow.com/questions/55929757/installing-r-3-6-on-ubuntu-disco-19-04

Step Annoying and AVOID:

1. if you use rgeos and use the google mapping functions you will need PROTOBUF and PROTOBUF-COMPILER.  To get this in linux and ubunutu 19.10 you must do the following:

1. go to terminal:
A. sudo apt-get upgrade
B. sudo snap install protobuf --classic (only after you have run sudo apt-get upgrade)
C. sudo apt-get install protobuf-compiler (note that you had already installed protobuf in earlier step


II. Printer Annoyances

Ok, why this is....who knows!  But....

find driver for printer.  Download and install (either terminal or manager) then go to << add printer << add it << then go to additional printer settings and select drivers from list and add.  Then!!! go to settings (still under the additional settings dialog) and change the server to one with an ip address.  as in this link: https://askubuntu.com/questions/1192711/cant-install-epson-et-3750-printer-on-ubuntu-19-10.  This final step where the device url is et to the ip with the PASSTHRU seems to connect everything.  Ugh. Annoying. 1 hr of my life goonnnnne!

When using the printer via a USB cable: re-install the drivers and printer and make sure CUPS is up to date. Add printer via the localhost 631 and then choose the updated driver for the printer.  

When printing select the printer with the garbage address for an ip -- the one with the numbers and stuff in the address bar seems to be the one that works.  Otherwise you get the "rendering completed" and nothing happens loop -- like the sent to printer nothing actually prints from the network thing.  IT seems to be a bug as it shows up 5 printers but only one (and the one you wouldn't think) appears to be the right one.  


III.  Extra Fonts

#extra fonts


#https://github.com/yixuan/showtext
library(showtext)
font_add_google("Schoolbell", "bell")

library(showtext)
#font_add_google("Schoolbell", "bell")
#font_add_google("Dancing Script", "Indie Flower")
#font_add_google("Shadows Into Light", "Permanent Marker")
#font_add_google("Permanent Marker", "Caveat")
#font_add_google("Ubuntu Condensed")
#font_add_google("Gloria Hallelujah", "Luckiest Guy")
#font_add_google("Bangers", "Marck Script")
#font_add_google("Covered By Your Grace", "Nanum Pen Script")


IV.

Additional tips for tmpap and mapping tools:

#Note: probably need to run 
      # sudo apt-get install -y libudunits2-dev
      # in terminal then install each invdividual package in order below

#install.packages("units")
#install.packages("lwgeom")
#install.packages("raster")
#install.packages("stars")
#install.packages("sf")
#install.packages("leaflet")
#install.packages("leafsync")
#install.packages("leafem")
#install.packages("tmaptools")
#install.packages("tmap")

As of Ubuntu 20.04 LTS and RStudio 1.2.5042 and R  4.0.0 you need to run the terminal for libudunits2-dev BEFORE trying to install tmap.  

