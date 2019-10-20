# Ubuntu FTP Server using vsftpd
Welcome to my very first blog post! As this is my first post please let me know if any mistakes were made. Feel free to contact me at mkr@kodeklubben.dk. 

By the end og this post you would have an up and running local ftp server with virtual users accessing it. In the future i would provide a blog post about making the ftp server accessible from a public domain or ip address. 

## Installing packages
We are going to use apt to install vsftpd. If you are not familiar with apt it is ubuntu's advanced packaging tool. You can read more about it [here](https://www.tecmint.com/useful-basic-commands-of-apt-get-and-apt-cache-for-package-management/).

In case you didn't know vsftpd stands for _very secure file transfer protocol deamon_ which is what we want to build in this post. First of all we have to install vsftpd by running the following command in the terminal.

```sh
$ sudo apt-get install vsftpd
```
## Setting up the virtual users database
This step of using virtual users and creating a database is inspired from this [article](https://help.ubuntu.com/community/vsftpd).

```sh 
$ mkdir /etc/vsftpd
```

```sh 
$ touch /etc/vsftpd/vusers.txt
```

```sh 
$ nano /etc/vsftpd/vusers.txt
```

output from file should be as the following

```
username
password
username2
password2
```

### Creating a database
To create the actual database we need to acquire the db_util package by running the following command in the terminal.
```sh 
$ sudo apt-get install db_util
```

```sh 
$ db_load -T -t hash -f vusers.txt virtual-user.db
```

```sh 
$ chmod 600 vsftpd-virtual-user.db
```

```sh 
$ rm vusers.txt
```

### Creating a PAM file

```sh 
$ sudo touch /etc/pam.d/vsftpd.virtual
```

```sh 
$ sudo nano /etc/pamd.d/vsftpd.virtual
```

``` 
#%PAM-1.0
auth       required     pam_userdb.so db=/etc/vsftpd/vsftpd-virtual-user
account    required     pam_userdb.so db=/etc/vsftpd/vsftpd-virtual-user
session    required     pam_loginuid.so
```

## Configuring vsftpd


## Defining users home directory

```sh 
$ mkdir -p /home/vftp/{username,username2}
```

```sh 
$ chown -R ftp:ftp /home/vftp
```

## Restart ftp server
Restart the ftp server with the new configuration.

```sh 
$ sudo service vsftpd restart
```