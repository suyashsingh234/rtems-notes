### rtems-notes

# make patch

git format-patch -1 master -o mypatch

-1 for last commit

-o for output directory


# send email
git send-email pathtopatch

# configure email
sudo apt-get install git-email

smtpserver = smtp.googlemail.com
    
smtpencryption = tls
    
smtpserverport = 587
    
smtpuser = youremail@gmail.com

##Clang Static Analyzer
It works with the existing build system

##LLVM Sanitizer
They are dynamic(run time)
