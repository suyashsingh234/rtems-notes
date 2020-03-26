# rtems-notes

## make patch

git format-patch -1 master -o mypatch

-1 for last commit

-o for output directory


## send email
git send-email pathtopatch

## configure email
sudo apt-get install git-email

smtpserver = smtp.googlemail.com
    
smtpencryption = tls
    
smtpserverport = 587
    
smtpuser = youremail@gmail.com

## Clang Static Analyzer

It works with the existing build system

### Building it

cd llvm-project

mkdir build (in-tree build is not supported)

cd build

cmake -DLLVM_ENABLE_PROJECTS=clang -G "Unix Makefiles" ../llvm

cmake -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_PROJECTS=lld -DCMAKE_INSTALL_PREFIX=/usr/local ../llvm

sudo make install

## LLVM Sanitizer

They are dynamic(run time)
