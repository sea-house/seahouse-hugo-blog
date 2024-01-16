---
title: "Overthewire Bandit"
date: 2024-01-16T12:07:11Z
draft: true
---

A link to the challenge
https://overthewire.org/wargames/bandit

The beginners CTF of overthewire

## Level14

All you need to do is use netcat to connect within bandit. You can then submit the previous password
```
nc localhost 30000
```

Flag
`BfMYroe26WYalil77FoDi9qh59eK5xNr`


## Level 15

This is using ssl encryption so I need to use openssl to connect. 
```
openssl s_client -connect localhost:30001
```

Flag
`cluFn7wTiGryunymYOu4RcffSxQluehd`


## Level 16

I'm going to need to se namp to work out which ports have a server listening on localhost. I'll then need to find out which of those ports speak SSL. After that I'll need to try to connect to each one in turn and see which gives back credentials, rather than just sending me back. 
```
nmap -sT -p 31000-32000 localhost
```
Output
```
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
```

This shows that there are 5 open ports. Now I need to work out which of these speaks SSL. Nmap has version detection tools I can use so I can improve the above search by running
```
nmap -sV -p 31000-32000 localhost
```
Output
```
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31790-TCP:V=7.40%T=SSL%I=7%D=5/15%Time=609FB484%P=x86_64-pc-linux-g
SF:nu%r(GenericLines,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20cu
SF:rrent\x20password\n")%r(GetRequest,31,"Wrong!\x20Please\x20enter\x20the
SF:\x20correct\x20current\x20password\n")%r(HTTPOptions,31,"Wrong!\x20Plea
SF:se\x20enter\x20the\x20correct\x20current\x20password\n")%r(RTSPRequest,
SF:31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\
SF:n")%r(Help,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x
SF:20password\n")%r(SSLSessionReq,31,"Wrong!\x20Please\x20enter\x20the\x20
SF:correct\x20current\x20password\n")%r(TLSSessionReq,31,"Wrong!\x20Please
SF:\x20enter\x20the\x20correct\x20current\x20password\n")%r(Kerberos,31,"W
SF:rong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\n")%r
SF:(FourOhFourRequest,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20c
SF:urrent\x20password\n")%r(LPDString,31,"Wrong!\x20Please\x20enter\x20the
SF:\x20correct\x20current\x20password\n")%r(LDAPSearchReq,31,"Wrong!\x20Pl
SF:ease\x20enter\x20the\x20correct\x20current\x20password\n")%r(SIPOptions
SF:,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password
SF:\n");
```
The same 5 ports are returned. 31518 and 31790 look like they speak SSL so let's use open_ssl to connect
```
openssl s_client -connect localhost:31518
cluFn7wTiGryunymYOu4RcffSxQluehd
```
Output
```
cluFn7wTiGryunymYOu4RcffSxQluehd
```
Not this one then as it's just returning what I sent
```
openssl s_client -connect localhost:31790
cluFn7wTiGryunymYOu4RcffSxQluehd
```
Output
```
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
```
Boom! Now I need to save this in a private key file
```
mkdir /tmp/seahouse1
cd /tmp/seahouse1
touch sshkey.private
nano sshkey.private
```
Then paste, save the above RSA private key. I also need to change permissions of this file so it can't be accessed by other users
```
chmod 600 sshkey.private
```
Finally I can ssh into the next level
```
ssh -i sshkey.private bandit17@localhost
```
I'm in!


## Level 17

There's only one line that has changed between passwords.old and passwords.new so I can use diff to find the difference. checking the diff help file, I can use the -q to report only where the files differ
```
diff passwords.old passwords.new
```
Output
```
42c42
< w0Yfolrc5bwjS4qw5mq1nnQi6mF03bii
---
> kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
```

Looks like one line has been changed
Flag
`kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`


## Level 18

I can't ssh into the level as the .bashrc has been modified. But I don't need to persist, I can just pass a cat command and get the contents of the readme file!
```
ssh -p 2220 bandit18@bandit.labs.overthewire.org cat readme
```
Output
```
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
```
Flag
`ueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x`


## Level 19

First run the binary file without arguments to find out how to use it:
```
./bandit20-do
```
Output
```
Run a command as another user.
  Example: ./bandit20-do id
```
So I can run a command as another user using this setuid binary file
the password is in /etc/bandit_pass but I'm guessing running the setuid binary bandit20-do will help me find the password as a different user. 

if you ls -l the files in the home directory you see that the bandit20-do file has elevated permissions.

In the etc/bandit_pass folder is a list of users (and probably their passwords) so I use the binary file on the bandit20 command
```
./bandit20-do cat /etc/bandit_pass/bandit20
```
Flag
`GbKksEFF4yrVs6il55v6gwY5aVje5f0j`

## Level 20

You need to setup a nc listener on localhost (bandit20@bandit) so open a terminal and set that up:
```
nc -lp 3132
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```
According to nc help file -l puts it into listen mode, -p specifies the port number (3132)

Now in a second terminal also in bandit20@bandit you can send the current password over to that port
```
./suconnect 3132
```
Output
```
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
```

and then you'll get the password on the listening server:
Flag
`gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr`


## Level 21

Find the bandit22 cronjob, use cat to read contents and find acommand running a fiel at /tmp/
cat that file to see the paswword
Flag
`Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI`


## Level 22

Find the bandit23 cronjob in /etc/cron.d
cat the file at /usr/bin/cronjob_bandit23.sh and see the following:
```
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

It's a bash file that checks the user echos it back, md5 hashes that string and cuts the - off the end. It then copies that as a password file

So if I run this command as bandit23 I should be able to find the file where the bandit23 is stored
```
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```
Output
```
8ca319486bfbbc3663ea0fbe81326349
```

```
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```
Output
Flag
`jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n`


## Level 23

Going to need to write a shell script for this one...

First up I follow the previous steps and find this file for the bandit24 cronjobs
```
cat /usr/bin/cronjob_bandit24.sh
```
Output
```
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```
It deletes all scripts in the /var/spool/$myname folder but skips hidden files then it assign root to owner function (thats what stat --format "%U" ./$i returns when I run it as bandit23). If owner = bandit23 then timeout sends signal 9 60 ./$i which I think kills the process after 90 seconds it then rm ./$i

So basically this script is executing every script in /var/spool/bandit24 then deleting it every 60 seconds

So if we had a file in there that could be run by any user then we could extract bandit24's password with those permissions.

First create a temp folder and change permissions 
```
mkdir -p /tmp/seahouse4
cd /tmp/seahouse4
touch getbtfpass.sh
chmod 777 getbtfpass.sh
ls -al getbtfpass.sh
```
Output
```
-rwxrwxrwx 1 bandit23 root 0 May 15 17:35 getbtfpass.sh
```

Right anyone can run this file. So now I can create the following bash file which writes bandit24s password to a password file in my directory
```
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/seahouse4/password
```
The password file needs to have full permissions so anyone including the cron script can write to it. 
```
touch password
chmod 666 password
ls -al password
```
Output
```
-rw-rw-rw- 1 bandit23 root 0 May 15 17:40 password
```
Finally copy it our bash script to the folder that gets croned every 60 seconds
```
cp getbtfpass.sh /var/spool/bandit24/getbtfpass.sh
```
And now we wait
```
cat password
```
Flag
`UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ`


## Level 24

I need to brute force a daemon listening on port 30002. I need to provid it with the last Flag as well as a 4 digit code. 

I could write a bash script that does this sending nc requests and going through the combinations
```
#! bin bash

passwd=UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ

for i in {0000..9999}; do
    echo "$passwd $i"
done | nc localhost 30002 > file.txt

cat file.txt | grep -v Wrong
```

grep -v removes all of the Wrong answers when you cat the result file at the end. Interestingly you have to pipe in the for loop to the nc command and not start with that.
now add this file to a tmp direcrtory
```
mkdir /tmp/seahouse5
cd /tmp/seahouse5
touch passcracker.sh
nano passcracker.sh
chmod 777 passcracker.sh
```
run the bash script
```
bash passcracker.sh
```
Output
```
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG
```
Flag
`uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG`


## Level 25

tbh didn't get this one, followed the instructions here

basically you ssh using the private key on bandit25. Usually it jus boots you out, but because it uses more you can reduce the size of your terminal and enter vim or another text editor, then access the passwd file. 

Flag
`5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z`


## Level 26

Same as before you can exploit the more command that runs when you minimize the terminal.  First minimize the terminal and login using the last flag
```
ssh -p 2220 bandit26@bandit.labs.overthewire.org
5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z
```
As there more command is running you can use vim to edit but also send commands. first you'll need to change the shell from /usr/bin/showtext to /bin/bash
```
v
esc
:set shell ?
```
Output
```
shell=/usr/bin/showtext
```

Change it
```
:set shell=/bin/bash
```
 From here you can list files in bandit26's home directory by using the ls command in a vim way. 
```
:! ls
```
Output
```
bandit27-do  text.txt
```
Okay so there's the text.txt for the login page and a bandit27-do file, what does that do?
```
:! bandit27-do
```
Okay runs commands as bandit27. We already know from previous examples that passwords for users are stored in /etc/bandit_pass/bandit27 so I can use this file to cat the password
```
:! ./bandit27-do cat /etc/bandit_pass/bandit27
```
Flag
`3ba3118a22e93127a4ed485be72ef5ea`


## Level 27

Make a temporary directory for the repo
```
mkdir /tmp/seahouse6
cd /tmp/seahouse6
```
Clone the repo into it (use the last level's flag for the password when prompted)
```
git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
cd repo
ls
cat README
```
Output
```
The password to the next level is: 0ef186ac70e04ea33b4c1853d2526fa2
```
Flag
`0ef186ac70e04ea33b4c1853d2526fa2`


## Level 28

Same instructions before, if you read the markdown file you'll see
```
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```

So the password has been obfuscated. Git is a version control tool though, I wonder if it's always been this way?
```
git log
```
Output
```
commit edd935d60906b33f0619605abd1689808ccdd5ee
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    fix info leak

commit c086d11a00c0648d095d04c089786efef5e01264
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    add missing data

commit de2ebe2d5fd1598cd547f4d56247e053be3fdc38
Author: Ben Dover <noone@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    initial commit of README.md
```
The commits are very telling, I bet commit c086d11a00c0648d095d04c089786efef5e01264 has what we need.  
```
git diff edd935d60906b33f0619605abd1689808ccdd5ee c086d11a00c0648d095d04c089786efef5e01264
```
Output
```
## credentials
 - username: bandit29
-- password: xxxxxxxxxx
+- password: bbc96594b4e001778eee9975372716b2
bbc96594b4e001778eee9975372716b
```
Flag
`bbc96594b4e001778eee9975372716b2`


# Level 29

Same deal as before. this time if we look at the diff between commits we see that the pasword hasn't been put into prod. I wonder if there's a branch out there that has non-prod stuff in it?

```
git remote show
git remote show origin
git checkout dev 
cat README.md
```

Flag
`5b90576bedb2cc04c86a9e924ce42faf`


# Level 30

huh only a README.md an empty file and one commit. Showing branches only shows HEAD and master. But what if I have a look in the hidden .git folder?
```
ls -a
cd .git
ls
```
hmm there's a packd-refs file in there which has a reference to a secret refs
```
cat packer-refs
```
Output
```
# pack-refs with: peeled fully-peeled 
3aefa229469b7ba1cc08203e5d8fa299354c496b refs/remotes/origin/master
f17132340e8ee6c159e0a4a6bc6f80e1da3b1aea refs/tags/secret
```
A ref is like a branch but without a history of commits. this topic suggest that i can use     git show --name-only <aTag> to list a tag and it's associated commit
```
git show --name-only f17132340e8ee6c159e0a4a6bc6f80e1da3b1aea
```
Output
```
80e1da3b1aea
47e603bb428404d265f59c42920d81e5
```

I wonder if i can show this secret tag
```
git show --name-only secret
```
Flag
`47e603bb428404d265f59c42920d81e5`


# Level 31

README.md says I need to push a file key.txt to the remote repository to the master branch it needs to have the can I come in as content

First create the file
```
echo 'May I come in?' > key.txt
```

Pushing directly to the master branch does nothing, it seems something is stopping our new .txt file being accounted for. If you checkout the gitignore file it seems there's a wildcard entry for *.txt files. So let's force add
```
git add -f key.txt
```
Yep that works now let's push it
```
git commit -m "force added key.txt"
git push origin master
```
Flag
`56a9bf19c63d650ce78e6ec0354ee45e`