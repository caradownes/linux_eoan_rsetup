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


III.  Extra Fonts

#extra fonts


#https://github.com/yixuan/showtext
library(showtext)
font_add_google("Schoolbell", "bell")

set.seed(123)
## Manually open a graphics device if you run this code in RStudio
## x11()
hist(rnorm(1000), breaks = 30, col = "steelblue", border = "white",
     main = "Histogram of Normal Random Numbers", xlab = "", ylab = "Frequency")

showtext_begin()
text(2, 70, "N = 1000", family = "bell", cex = 2.5)
showtext_end()


library(showtext)
## Loading Google fonts (http://www.google.com/fonts)
font_add_google("Gochi Hand", "gochi")
font_add_google("Schoolbell", "bell")
font_add_google("Covered By Your Grace", "grace")
font_add_google("Rock Salt", "rock")

## Automatically use showtext to render text for future devices
showtext_auto()

## Tell showtext the resolution of the device,
## only needed for bitmap graphics. Default is 96
## showtext_opts(dpi = 96)

set.seed(123)
x = rnorm(10)
y = 1 + x + rnorm(10, sd = 0.2)
y[1] = 5
mod = lm(y ~ x)

## Plotting functions as usual
## Open a graphics device if you want, e.g.
## png("demo.png", 700, 600, res = 96)
## If you want to show the graph in a window device,
## remember to manually open one in RStudio
## See the "Known Issues" section
x11()

op = par(cex.lab = 2, cex.axis = 1.5, cex.main = 2)
plot(x, y, pch = 16, col = "steelblue",
     xlab = "X variable", ylab = "Y variable", family = "gochi")
grid()
title("Draw Plots Before You Fit A Regression", family = "bell")
text(-0.5, 4.5, "This is the outlier", cex = 2, col = "steelblue",
     family = "grace")
abline(coef(mod))
abline(1, 1, col = "red")
par(family = "rock")
text(1, 1, expression(paste("True model: ", y == x + 1)),
     cex = 1.5, col = "red", srt = 20)
text(0, 2, expression(paste("OLS: ", hat(y) == 0.79 * x + 1.49)),
     cex = 1.5, srt = 15)
legend("topright", legend = c("Truth", "OLS"), col = c("red", "black"), lty = 1)

par(op)



