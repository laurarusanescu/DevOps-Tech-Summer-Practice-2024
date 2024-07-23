______Work with Users________
UID 0 is reserved for root.  
1-99 are reserved for predefined accounts (like games, mail, www-data, sys, bin and more)  
100-999 reserved for system and administrative accounts.  
1000+ are reserved for users  

GID 0 is reserved for root.  
1-99 are reserved for system and application use.  
100+ are for users.  

~~~clear && cat /etc/passwd  
The structure of this file is as follows:  
username:password:UID:GID:description:homedir:shell  
username - obvious, it is the name of the user  
password - password. Many years ago this was the place where password actually was stored. But for security reasons it chaned and now passwords are available in different form, somewhere else. We will go there later. x in this field means that password is encrypted and stored in the other file.  
UID - unique User IDentifier.  
GID - unique Group IDentifier.  
description - this field can contain many information. The real first and last name. Address. Phone number. Role in the organization. You name it.  
homedir - the location named as home directory. This is the "home" of the user, where he logs and stores his data.  
shell - simply speaking, the shell is the program which takes the commands sent by user, interprets it, and return the answer  

______Useradd______
We can add to the comands useradd os adduser the following arguments:  
-d - create home directory in specified location, if we want to change  
-m - create the home directory  
-p - password  
-s - provide shell  
-c - comments, or description of the account 

Change password:  
grep testuser /etc/shadow  
passwd testuser  
