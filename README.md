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
