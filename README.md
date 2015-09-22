# gcc-4.9.3-OSX-vnmr-gcc-
A version of gcc-4.9.3 built on OS X that installs in /vnmr/gcc. Contains C, C++ and FORTRAN compilers.

##Built on OS X
This is a self-contained installation of gcc 4.9.3 that is installed in /vnmr/gcc but was built with Apple LLVM 6.0

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

This is built using Xcode 6.2 on OS X Mavericks (10.9.5) using Apple cc and c++.

###Download sources###


Get the sources fron the GNU mirrors. 
```
curl -sO http://gnu.mirror.iweb.com/libiconv/libiconv-1.14.tar.gz
curl -sO http://gnu.mirror.iweb.com/gmp/gmp-6.0.0a.tar.bz2
curl -sO http://gnu.mirror.iweb.com/mpfr/mpfr-3.1.3.tar.bz2
curl -sO http://gnu.mirror.iweb.com/mpc/mpc-1.0.3.tar.gz
isl
```

###Expand

```
tar xf gmp-6.0.0a.tar.bz2
tar xf mpfr-3.1.3.tar.bz2
tar xf mpc-1.0.3.tar.gz
tar xf gcc-4.9.3.tar.bz2
tar xf libiconv-1.14.tar.gz
```

###Build libiconv
```
cd libiconv-1.14
./configure -prefix=/vnmr/gcc/
make -j 4 && make install
cd ..
```

###Move the libraries required for GCC into the GCC tree.

Move the libraries and then build in a separate directory. As an aside the previous version had problems due to old mpc and gmp libraries in /usr/local. 

```
mv gmp-6.0.0 gcc-4.9.3/gmp
mv isl-0.14 gcc-4.9.3/isl
mv mpfr-3.1.3 gcc-4.9.3/mpfr
mv mpc-1.0.3 gcc-4.9.3/mpc
mkdir build-gcc && cd build-gcc
../gcc-4.9.3/configure --prefix=/vnmr/gcc/ --enable-languages=c,c++,fortran --with-system-zlib --with-libiconv-prefix=/vnmr/gcc
make -j 6
sudo mkdir /vnmr/gcc
make install
```

###System used:

uname -m = x86_64
uname -r = 13.4.0
uname -s = Darwin
uname -v = Darwin Kernel Version 13.4.0: Wed Mar 18 16:20:14 PDT 2015; root:xnu-2422.115.14~1/RELEASE_X86_64
Apple LLVM version 6.0 (clang-600.0.57) (based on LLVM 3.5svn)
Target: x86_64-apple-darwin13.4.0

Use scons-test repository to test if the compiler is installed ok.
