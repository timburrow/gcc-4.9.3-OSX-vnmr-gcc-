# gcc-4.9.3-OSX-vnmr-gcc-
A version of gcc-4.9.3 built on OS X that installs in /vnmr/gcc. Contains C, C++ and FORTRAN compilers.

##Built on OS X
This is a self-contained installation of gcc 4.9.3 that is installed in /vnmr/gcc

Install:
```bash
sudo mkdir /vnmr/
sudo tar jxf gcc64.tar.bz2 -C /vnmr
```

Set the PATH:

```
export PATH=/vnmr/gcc/bin:${PATH}
or
setenv PATH /vnmr/gcc/bin:${PATH}
```

##How I built this

I started from gcc-4.9.0 (installed in /usr/local) and Xcode 6.2

###Download sources###

I started from gcc-4.9.0 (installed in /usr/local) and Xcode 6.2


Get the sources fron the GNU mirrors. I didn't use binutils in the end.
```
#curl -sO http://ftp.gnu.org/gnu/binutils/binutils-2.25.tar.bz2
curl -sO http://gnu.mirror.iweb.com/libiconv/libiconv-1.14.tar.gz
curl -sO http://gnu.mirror.iweb.com/gmp/gmp-6.0.0a.tar.bz2
curl -sO http://gnu.mirror.iweb.com/mpfr/mpfr-3.1.3.tar.bz2
curl -sO http://gnu.mirror.iweb.com/mpc/mpc-1.0.3.tar.gz
```

###Expand

```
tar xf gmp-6.0.0a.tar.bz2
tar xf mpfr-3.1.3.tar.bz2
tar xf mpc-1.0.3.tar.gz
tar xf gcc-4.9.3.tar.bz2
#tar xf binutils-2.25.tar.bz2
tar xf libiconv-1.14.tar.gz
```

###Move the libraries required for GCC into the GCC tree.

Move the libraries and then build in a separate directory

```
mv gmp-6.0.0 gcc-4.9.3/gmp
mv isl-0.14 gcc-4.9.3/isl
mv mpfr-3.1.3 gcc-4.9.3/mpfr
mv mpc-1.0.3 gcc-4.9.3/mpc
mv libiconv-1.14 gcc-4.9.3/libiconv
mkdir build-gcc && mv build-gcc
 ../gcc-4.9.3/configure --enable-languages=c,c++,fortran --prefix=/vnmr/gcc64/ --with-system-zlib
make -j 6
sudo mkdir /vnmr/gcc
make install
```

###System used:

uname -m = x86_64
uname -r = 13.4.0
uname -s = Darwin
uname -v = Darwin Kernel Version 13.4.0: Wed Mar 18 16:20:14 PDT 2015; root:xnu-2422.115.14~1/RELEASE_X86_64

Use scons-test repository to test if the compiler is installed ok.
