+++
date = 2018-01-23
draft = false
tags = ["Rstudio", "Server", "Linux", "R"]
title = "Configure Rstudio Server on Ubuntu 16.04"
math = false
+++

{{% toc %}}

# Configure Ubuntu

## install `ubuntugis` on Ubuntu
> ref: https://github.com/r-spatial/sf

To install the dependencies on Ubuntu, either add ubuntugis-unstable to the package repositories:

```
sudo apt-get install software-properties-common python-software-properties
sudo add-apt-repository ppa:ubuntugis/ubuntugis-unstable
sudo apt-get update
sudo apt-get install libudunits2-dev libgdal-dev libgeos-dev libproj-dev 
```

or install dependencies from source; see e.g. an older travis config file for hints.

## install `libxml2-dev` and `libcurl4-openssl-dev`

Before install R package `RCurl` and `XML`, first we need to install `libxml2-dev` and `libcurl4-openssl-dev`

```
sudo apt-get install libxml2-dev
sudo apt-get install libcurl4-openssl-dev
```

## install `FreeType`

****************************************************
Error: freetype-config not found.
Please install FreeType with freetype-config script.
If you have not installed FreeType, you can
download the source code from http://freetype.org/

In Debian/Ubuntu-like systems, you can use
  `sudo apt-get install libfreetype6-dev`
to install FreeType

For rpm-based systems, try
  "sudo yum install freetype-devel"
****************************************************


# Install Latest R

## Install R on Ubuntu 16.04

1. Add R repository

First, we’ve got to add a line to our /etc/apt/sources.list file. This can be accomplished with the following. Note the “xenial” in the line, indicating Ubuntu 16.04. If you have a different version, just change that.

```
sudo echo "deb http://cran.rstudio.com/bin/linux/ubuntu xenial/" | sudo tee -a /etc/apt/sources.list
```

2. Add R to Ubuntu Keyring

```
gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9
gpg -a --export E084DAB9 | sudo apt-key add -
```

3. Install R-Base

Most Linux users should be familiar with the old…

```
sudo apt-get update
sudo apt-get install r-base r-base-dev
```


## install `R` packages
Run the following code, need ~ 10 min to install all these packages.

```r
options("repos" = c(CRAN = "http://cran.rstudio.com/"))
install.packages(c("data.table", "knitr", "RCurl", "XML", "ggmap", "raster", "openair", "rasterVis", "viridisLite", "sf", "geomnet", "ggrepel", "showtext", "geosphere", "kableExtra", "ggsn", "tidyverse"))
```

## Update packages

```r
# list all packages where an update is available
old.packages()

# update all available packages
update.packages()

# update, without prompts for permission/clarification
update.packages(ask = FALSE, dependencies = c('Suggests'))
```

Nice sollution as follows:

```r
all.packages <- installed.packages()
r.version <- paste(version[['major']], '.', version[['minor']], sep = '')
 
for (i in 1:nrow(all.packages))
{
    package.name <- all.packages[i, 1]
    package.version <- all.packages[i, 3]
    if (package.version != r.version)
    {
        print(paste('Installing', package.name))
        install.packages(package.name)
    }
}
```

## Configure R with Chinese Env.
> Ref: https://stackoverflow.com/questions/39304082/show-chinese-character-in-ggplot2
> Ref: http://www.lypblog.com/index.php/archives/30/

```
echo $LANG

sudo apt-get update && apt-get install language-pack-zh-hans
sudo apt-get install fonts-wqy-zenhei
sudo apt-get install fonts-arphic-bkai00mp fonts-arphic-bsmi00lp fonts-arphic-gbsn00lp fonts-arphic-gkai00mp fonts-arphic-ukai fonts-arphic-uming fonts-cns11643-kai fonts-cns11643-sung fonts-cwtex-fs fonts-cwtex-heib fonts-cwtex-kai fonts-cwtex-ming fonts-cwtex-yen
```


# Use `RStudio Server`

## Install `RStudio Server`
Download RStudio Server v1.1.414
https://www.rstudio.com/products/rstudio/download-server/

64bit
Size:  60.6 MB MD5: c14e861b185be22e5e05ff41ab389c76 Version:  1.1.414 Released:  2018-01-17
```
sudo apt-get install gdebi-core
wget https://download2.rstudio.org/rstudio-server-1.1.414-amd64.deb
sudo gdebi rstudio-server-1.1.414-amd64.deb
```

## Use `Rstudio server`

Helps:

> - https://support.rstudio.com/hc/en-us/sections/200150693-RStudio-Server
> - https://www.digitalocean.com/community/tutorials/how-to-set-up-rstudio-on-an-ubuntu-cloud-server
> - https://support.rstudio.com/hc/en-us/articles/200552306-Getting-Started
> - http://docs.rstudio.com/ide/server-pro/authenticating-users.html

### Creating RStudio User
It is not advisable to use the root account with RStudio, instead, create a normal user account just for RStudio. The account can be named anything, and the account password will be the one to use in the web interface.

```
sudo adduser rstudio
```

RStudio will use the user's home directory as it's default workspace.

### Using R Studio Server

By default RStudio Server runs on port `8787` and accepts connections from all remote clients. After installation you should therefore be able to navigate a web browser to the following address to access the server:

For example

```
http://<server-ip>:8787
http://172.104.104.180:8787/auth-sign-in
```

rstudio wang