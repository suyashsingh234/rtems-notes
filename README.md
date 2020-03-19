# rtems-notes

#make patch

git format-patch -1 master

-1 for last commit


#send email
git send-email pathtopatch

#configure email
sudo apt-get install git-email
    smtpserver = smtp.googlemail.com
    smtpencryption = tls
    smtpserverport = 587
    smtpuser = youremail@gmail.com
   
