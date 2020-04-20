# rtems-notes

## -fuse-ld

use command -fuse-ld=/linker/ to change linker temporarily

example gcc -fuse-ld=lld main.c

## make patch

git format-patch -1 master -o mypatch

-1 for last commit

-o for output directory


## send email
git send-email pathtopatch

## configure email

Change directory to where you want the llvm directory placed.

git clone https://github.com/llvm/llvm-project.git

sudo apt-get install git-email

smtpserver = smtp.googlemail.com
    
smtpencryption = tls
    
smtpserverport = 587
    
smtpuser = youremail@gmail.com

## Clang Static Analyzer

It works with the existing build system

### Building it (Ubuntu)

Ensure at least 2 GB free space in root partition

cd llvm-project

#### use lld as linker instead of ld

mkdir build (in-tree build is not supported)

cd build

cmake -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_PROJECTS=lld -DCMAKE_INSTALL_PREFIX=/usr/local ../llvm

sudo make install

sudo ln -s /usr/local/bin/ld.lld /usr/bin/ld --force

cd ..

#### build clang and llvm 

mkdir build2

cd build2

sudo apt-get install g++

cmake -DLLVM_ENABLE_PROJECTS=clang -G "Unix Makefiles" ../llvm

sudo make


#### add to path
add the following to your path:

The location of the clang binary.

The locations of the scan-build and scan-view programs

scan-build, scan-view and clang should be in "path to llvm sources/llvm-project/build2/bin" 
    
open terminal

gedit ~/.bashrc

at the last line add

export PATH=path to llvm sources/llvm-project/build2/bin:$PATH

## LLVM Sanitizer

They are dynamic(run time)

mkdir build_sanitizer

cd build_sanitizer

cmake ../llvm

cmake --build .   (notice the dot)

sudo cmake --build . --target install

cd ..

mkdir build-compiler-rt

cd build-compiler-rt

cmake ../compiler-rt -DLLVM_CONFIG_PATH=/path/to/llvm-config inside build_sanitizer

make

now paste build-compiler-rt/lib/linux directory to correct location if error message

## scan-build

remove all "const" from /quick-start/rtems/5/sparc-rtems5/erc32/lib/include/rtems/score/cpustdatomic.h

export C_INCLUDE_PATH=<YOUR_TOOLCHAIN_PATH>/sparc-rtems5/include/:<YOUR_TOOLCHAIN_PATH>/lib/gcc/sparc-rtems5/7.3.0/include/

export PATH=<YOUR_TOOLCHAIN_PATH>/bin:${PATH}

scan-build --use-cc=<YOUR_TOOLCHAIN_PATH>/bin/sparc-rtems5-gcc --use-c++=<YOUR_TOOLCHAIN_PATH>/bin/sparc-rtems5-g++ --analyzer-target=sparc-rtems-unknown $HOME/quick-start/src/rtems/configure --target=sparc-rtems5 --disable-posix --enable-smp --enable-cxx --disable-networking --enable-drvmgr --disable-tests CFLAGS="-D__CLANG_ATOMICS -D__rtems__"  --enable-rtemsbsp=erc32

scan-build --use-cc=<YOUR_TOOLCHAIN_PATH>/bin/sparc-rtems5-gcc --use-c++=<YOUR_TOOLCHAIN_PATH>/bin/sparc-rtems5-g++ --analyzer-target=sparc-rtems-unknown make


#### berc32

```
export C_INCLUDE_PATH=$HOME/quick-start/rtems/5/sparc-rtems5/include/:$HOME/quick-start/rtems/5/lib/gcc/sparc-rtems5/7.5.0/include/

export PATH=$HOME/quick-start/rtems/5/bin:${PATH}

scan-build --use-cc=$HOME/quick-start/rtems/5/bin/sparc-rtems5-gcc --use-c++=$HOME/quick-start/rtems/5/bin/sparc-rtems5-g++  --analyzer-target=sparc-rtems-unknown $HOME/quick-start/src/rtems/configure --target=sparc-rtems5 --disable-posix --enable-smp --enable-cxx --disable-networking --enable-drvmgr --disable-tests CFLAGS="-D__CLANG_ATOMICS -D__rtems__"  --enable-rtemsbsp=erc32

scan-build --use-cc=$HOME/quick-start/rtems/5/bin/sparc-rtems5-gcc --analyzer-target=sparc-rtems-unknown make

```

#### at697f

```
export C_INCLUDE_PATH=$HOME/quick-start/rtems/5/sparc-rtems5/include/:$HOME/quick-start/rtems/5/lib/gcc/sparc-rtems5/7.5.0/include/:$HOME/quick-start/src/rtems/bsps/sparc/leon2/include:$HOME/quick-start/src/rtems/bsps/sparc/leon3/include

export PATH=$HOME/quick-start/rtems/5/bin:${PATH}

scan-build --use-cc=$HOME/quick-start/rtems/5/bin/sparc-rtems5-gcc --use-c++=$HOME/quick-start/rtems/5/bin/sparc-rtems5-g++  --analyzer-target=sparc-rtems-unknown $HOME/quick-start/src/rtems/configure --target=sparc-rtems5 --disable-posix --enable-smp --enable-cxx --disable-networking --enable-drvmgr --disable-tests CFLAGS="-D__CLANG_ATOMICS -D__rtems__"  --enable-rtemsbsp=at697f

scan-build --use-cc=$HOME/quick-start/rtems/5/bin/sparc-rtems5-gcc --analyzer-target=sparc-rtems-unknown make

```

#### leon2

```
export C_INCLUDE_PATH=$HOME/quick-start/rtems/5/sparc-rtems5/include/:$HOME/quick-start/rtems/5/lib/gcc/sparc-rtems5/7.5.0/include/

export PATH=$HOME/quick-start/rtems/5/bin:${PATH}

scan-build --use-cc=$HOME/quick-start/rtems/5/bin/sparc-rtems5-gcc --use-c++=$HOME/quick-start/rtems/5/bin/sparc-rtems5-g++  --analyzer-target=sparc-rtems-unknown $HOME/quick-start/src/rtems/configure --target=sparc-rtems5 --disable-posix --enable-smp --enable-cxx --disable-networking --enable-drvmgr --disable-tests CFLAGS="-D__CLANG_ATOMICS -D__rtems__"  --enable-rtemsbsp=leon2

scan-build --use-cc=$HOME/quick-start/rtems/5/bin/sparc-rtems5-gcc --analyzer-target=sparc-rtems-unknown make
```
paths maybe different than those given

scan-build also needs to run with the configure 
