+++
date = 2017-11-07
draft = false
tags = ["linux", "R"]
title = "Compile Linux Packages on HPC without root Access"
math = false
+++

{{% toc %}}

# Procedure
Complile latest R in RedHat Linux (HPC) without root access.

Main Ref: Thanks to PJ. Mostly Harmless's blog: [Building R-devel on RedHat Linux 6](http://pj.freefaculty.org/blog/?p=315)

## Install `zlib`
```
cd ~/src
 wget http://zlib.net/zlib-1.2.11.tar.gz
 tar xzvf zlib-1.2.11.tar.gz
 cd zlib-1.2.11
 ./configure --prefix=$HOME/packages

make
make install
```

## Adjust the environment
```
 export PATH=$HOME/packages/bin:$PATH
 export LD_LIBRARY_PATH=$HOME/packages/lib:$LD_LIBRARY_PATH 
 export CFLAGS="-I$HOME/packages/include" 
 export LDFLAGS="-L$HOME/packages/lib" 
```

## Get new `bzlib` support
```
cd ~/src
wget http://www.bzip.org/1.0.6/bzip2-1.0.6.tar.gz
tar xzvf bzip2-1.0.6.tar.gz
cd bzip2-1.0.6
make -f Makefile-libbz2_so
make clean
make -n install PREFIX=$HOME/packages
make install PREFIX=$HOME/packages
```

**Error**:

> /usr/bin/ld: /home/pauljohn/packages/lib/libbz2.a(bzlib.o): 
> relocation R_X86_64_32S against `BZ2_crc32Table` can not be used 
> when making a shared object; recompile with **-fPIC**

**Solution**:

I went into the `bzip2` directory and inserted `-fPIC` as a `CFLAGS` in the  **Makefile**. Then I ran make and make install `PREFIX=$HOME/packages` again

## Get `liblzma`
```
cd ~/src
wget --no-check-certificate https://tukaani.org/xz/xz-5.2.3.tar.gz
tar xzvf xz-5.2.3.tar.gz
cd xz-5.2.3
./configure --prefix=$HOME/packages
make -j3
make install
```

## Get `pcre`
```
cd ~/src
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.38.tar.gz
tar xzvf pcre-8.38.tar.gz
cd pcre-8.38
./configure --enable-utf8 --prefix=$HOME/packages
make -j3
make install
```

## Get `curl`
```
cd ~/src
wget --no-check-certificate https://curl.haxx.se/download/curl-7.47.1.tar.gz
tar xzvf curl-7.47.1.tar.gz
cd curl-7.47.1
./configure --prefix=$HOME/packages
make -j3
make install
```

## Get `openssl`
```
cd ~/src
wget wget www.openssl.org/source/openssl-1.0.2d.tar.gz
tar xf openssl-1.0.2d.tar.gz
cd openssl-1.0.2d
make clean
./config --prefix=$HOME/packages/openssl --openssldir=$HOME/package/openssl shared -Wl,--enable-new-dtags,-rpath,'$(LIBRPATH)'
make
make install

# make clean
# ./Configure --prefix=$HOME/packages/openssl linux-x86_64
# make -sj4 build_libs
# make -s test
# make install_sw 

echo "export PATH=$HOME/packages/openssl/bin:$PATH" >> ~/.bashrc
echo $LD_LIBRARY_PATH 
echo $PATH
```

## Get `readline`
```
cd ~/src
wget http://ftp.gnu.org/gnu/readline/readline-4.3.tar.gz
tar xzvf readline-4.3.tar.gz
cd readline-4.3
./configure --prefix=$HOME/packages
make 
make install
```

## Get `R`
```
cd ~/src
wget https://cran.r-project.org/src/base/R-3/R-3.4.2.tar.gz
tar -zxvf R-3.4.2.tar.gz
cd R-3.4.2
./configure --prefix=$HOME/packages --enable-R-shlib --with-readline=no --with-x=no
make
make install
```

Done!!!

## install `hdf4`
Already on HPC:

```
export PATH=$PATH:/share/soft/rhel6.2/hdf-4.2.8/bin
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/share/soft/rhel6.2/hdf-4.2.8/lib
```

## install `hdf5`
```
cd ~/src
wget --no-check-certificate https://support.hdfgroup.org/ftp/HDF5/current/src/hdf5-1.10.1.tar.gz
tar zxf hdf5-1.10.1.tar.gz
cd hdf5-1.10.1
./configure --prefix=$HOME/packages
make
make install
```

## install `gdal`
```
cd ~/src
wget http://download.osgeo.org/gdal/2.2.2/gdal-2.2.2.tar.gz
tar zxf gdal-2.2.2.tar.gz
cd gdal-2.2.2
# ./configure --prefix=$HOME/packages --with-hdf4=/share/soft/rhel6.2/hdf-4.2.8/ --with-hdf5=/wps/home/wangjh/packages/ --with-python
./configure --prefix=$HOME/packages --with-python
make
make install
```

## install `libxml2`
```
cd ~/src
wget ftp://xmlsoft.org/libxml2/libxml2-2.9.7.tar.gz
tar zxf libxml2-2.9.7.tar.gz
cd libxml2-2.9.7
./configure --prefix=$HOME/packages --without-python
make
make install
```



## Install R packages

```
options("repos" = c(CRAN = "http://cran.rstudio.com/"))
install.packages("tidyverse")
```

Make sure you're using this R:
```
echo "export PATH=$HOME/packages/bin:$PATH" >> ~/.bashrc
echo $LD_LIBRARY_PATH 
echo $PATH
```
