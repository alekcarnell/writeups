
Level 0:
1. I booted up my Parrot VM
2. I pinged the provided domain name to see if it was up. 
3. I SSH'd into the box using ```ssh -p 2220 bandit0@bandit.labs.overthewire.org``` using password: "bandit0"
4. I proceeded to Level 1

Level 0:
1. I used `pwd` to make sure I was in the home directory, where the game said the "readme" file was located. 
2. After confirming I was in the correct directory, and then used `ls` to see if the "readme" file was there. It was. 
3. I then used `cat readme` to output the contents of the file, which was the credentials to use be used for the next account in the next level. 
4. The password to move on was: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

Level 1:
1. I opened a new terminal window and SSH'd back in using the new credentials provided. 
2. There was a file named "-". 
3. At first a tried reading it the ways that knew how. so I tried:
	1. `cat -`
	2. `nano -`
4. I then tried listing more information about it but that didn't give me any clue. 
5. I finally googled "opening a file named dash" and found out that you need to put in the directory in order for the command line to know what to do. 
6. I got the new password and moved on to level 3: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

Level 2: 
1. I checked `pwd` after logging in to see where I was. 
2. I then performed `ls` to see what was there. I found "A file with spaces"
3. I opened it using `cat a` and then did `Tab` to autocomplete the file name, which made it `A/ file/ with/ spaces`
4. I got the password and moved on to level 4: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

Level 3:
performed an `ls` and found the "inhere" directory 
2. There was nothing returned, so I ran `ls -a` and found a file named "...Hiding-From-You". 
3. I ran `cat ...Hiding-from-you` and got the new password: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

Level 4:
1. performed an `ls` and found the "inhere" directory 
2. Inside the directory were 8 files, named "-file01" through "-file08"
3. I performed `cat /home/bandit4/inhere/file01` and it returned a bunch of symbols and gibberish
4. I went through each file manually the same way until I got to file07 where the password was located. 
5. password: `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

Level 5: 
1. performed an `ls` and found `inhere`. I changed directory and performed another `ls`, this time found several directories named "maybeinhere"
2. the guide mentioned that the file was 1033b in size so i performed `find -type f -size 1033c` to locate the file. 
3. I then did `cat .file2` and got the password: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

Level 6: 
1. I performed a `cd ./` to go back to root before trying `find -type f -size 33c -group bandit6 -user bandit7`
2. I got a lot of permissions denied. I tried running as sudo but the current user isn't on the sudoers file and I didnt have permissions to put them there. 
3. I scrolled through the list of results and found something interesting in `etc/bandit_pass`. I `cd` there to take a closer look
4. there were 30+ folders all called "bandit01-bandit33"
5. I used the up arrow key to run my old `find` command and it returned `./bandit6`
6. I performed `cat ./bandit6` and got the password: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`
7. This was the same password as before, even though it was in a different spot. I tried logging in to the next room and it would not take it... I had made an error somewhere and had to go back.
8. I tried first going to the root of `/etc/` and running the `find` again. 
9. then I googled the `find` manual and tried a `find /` to look through the entire server which then returned a result
10. `cat /var/lib/dpkg/info/bandit7.password` gave me: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

Level 7:
1. The instructions said that the password would be inside of a file. I checked my working directory for the file and it was there. `ls -a` showed `data.txt`
2. because the password was inside the file, I knew I would need grep. 
3. `grep millionth data.txt` gave me `millionth	dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

level 8:
1. tried a variety of `uniq` commands by themselves, having not used it before. I was getting results but not a single line. I just went ahead and googled how to effectively use it to parse a file and found a useful command to pipe the results through, which gave me the correct password: `cat data.txt | sort | uniq -u`
2. `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

Level 9: 
1. I first just ran `strings` against the data file to see what would come up. A bunch of lines and a few with the = characters and, what looked like, password. 
2. I then piped that into a `grep =` which revealed that the password that I saw just from the strings output was right: `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

level 10: 
1. Level informed me that the file was base64 encoded so I just ran `base64 -d data.txt` and got: `The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`

Level 11: 
1. "The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions"
2. I know that this is a ceaser cipher. I open the file and it looks like a simple ceaser cipher so I just copied it and pasted it into a cracker online to get the password: `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`

Level 12:
1. I didn't actually have any experience with compression or archive files at this point so I had to google this level extensively. What it came down to though was that I needed to learn 1) the `file` command to identify what kind of compression the file had and 2) the different commands for working with each extension like gz, bz2, tar, bin, and a hexdump. Looking up a guide for this level specifically helped a lot and I felt it necassary to actually understand what was being asked. This was a "russian doll" situation where a file was compressed over and over again and I just needed to undo it in the order that it was done. 
2. the sequence of unzipping was hexdump > gz > bz2 > gz > tar
3. `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`

Level 13: 
1. SSH'd from the current ssh session into Bandit14 using the SSH key in the home directory
2. Per the instructions from the website, tried `cat /etc/bandit_pass/band14` and got the password: `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`

Level 14: 
1. Logging in again to a localhost server on port 30000, using the current password. 
2. The site gives you a lot of options to connect like ssh, telnet, openssl. I tried ssh at first, thinking I needed to use my current "bandit15" username, but that was not working. 
3. I tried using telnet and just did `telnet 127.0.0.1 30000` and I got a connection. I then pasted the old password in the server replied with the new password: `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`

Level 15:
1. Same as before, except I tried openssl since it asked to do it over an encrypted connection. 
2. Tried `openssl s_client -connect 127.0.0.1:30001` and got a connection. 
3. After pasting and submitting the old password, the server replied with the new one: `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`

Level 16:
1. same as before except now we have to find the port to connect to our selves. 
2. I just tried some basic nmap syntax to get going: `nmap 127.0.0.1 -p 31000-32000` and got:
		PORT      STATE SERVICE
		31046/tcp open  unknown
		31518/tcp open  unknown
		31691/tcp open  unknown
		31790/tcp open  unknown
		31960/tcp open  unknown
3. I was going to brute force this at first, since there weren't that many. But I figured I would search online to see if there was anything else I could do to get the answer directly, in case there was ever a situation where there were thousands of ports or something. 
4. I just added `-sV` to the syntax to show services and versions and got:
	PORT      STATE SERVICE     VERSION
	31046/tcp open  echo
	31518/tcp open  ssl/echo
	31691/tcp open  echo
	31790/tcp open  ssl/unknown
	31960/tcp open  echo
5. I decided to just brute force the 2 servers that showed SSL
6. The password is an RSA key file. I saved it to my local machine in a file and changed the permissions of it using `sudo chmod 600` and then ssh'd into the next room using `ssh bandit17@bandit.labs.overthewire.org -p 2220 -i ssh_key`

Level 17: 
1. Used `diff passwords.new passwords.old` to compare the two files in the home directory to find the password
2. `x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`

Level 18:
1. when logging in, you get automatically logged back out. 
2. I logged back in to the old room bandit17 and looked at the `.bashrc` file. 
3. After doing extensive googling, it looks like for this excercise, I am not supposed to change that. I instead just need to ask for the info in my ssh request. 
4. `ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat ~/readme"`
5. password: `cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8`

Level 19:
1. This one was pretty hard for me to understand, as linux permissions are a bit of a blind spot for me at the time of writing this. For my own sake, I will write out what the problem is asking in terms I can understand. Also used a [reference](https://mayadevbe.me/posts/overthewire/bandit/level20/)
2. The problem is asking me to recognize that there is a binary file in the home directory named "bandit20-do". I need to check the current owner and permissions of the file by running `ls -la`
3. this shows me that the file is owned by bandit19, however it is executed as bandit20. 
4. Running the binary shows that it executed another command, as bandit20, which we can now access. `./bandit20-do cat /etc/bandit_pass/bandit20`
5. password: `0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`

Level 20: 
1. This one confused me quite a bit at first. After some research, I realized I needed to set up a simple TCP server using Netcat that transmitted the current password. To do this, I first tried `echo "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc 127.0.0.1 30000`
2. This was returning a message from the server which said "Wrong password, try again"
3. That tripped me up, because it seemed like I had gotten it "right" but was just submitting the wrong password. This was not the case though. I had "arbitrarily" chosen the 30000 port which was already in use from the previous rooms. Furthermore, my Netcat syntax wasn't listening properly. I had to google my way out of this and read up on the netcat manual. 
4. I reset everything and tried new Netcat syntax `echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 61337`
5. I added the `-n` to prevent a new line from being made, which may have been contributing to some errors. I also properly set the `-l` in the netcat command so that it would listen on the specified port. 
6. Now I ran the `./suconnect20` binary and got a response and the new password!
7. `EeoULMCra2q0dSkYj561DX7s1CpBuOBt`

Level 21:
1. This one was a lot easier for me. 
2. checked cronjobs `cd /etc/cron.d`
3. `ls`
4. `cat cronjob_bandit22.sh` gives `bandit22 /usr/bin/cronjob_bandit22.sh`
5. `cat /usr/bin/cronjob_bandit22.sh` gives `#!/bin/bash chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`
6. `cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` gives the password: `tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q`

Level 22: 
1. Like the last level, I went into /etc/cron.d to find what was being executed, then went to /usr/bin to find the specific script. 
2. I ran the script and it made a temporary directory like in the last level. So I navigated to that directory and saw the password, but it was the same as the last level. 
3. I fgured I would try using it anyway to log in to the last level, but no dice. 
4. I logged back in to the current level and studied the script more and immediately thought that I was supposed to hack the script. So I tried editing it with `nano` but was told that permissions were denied. 
5. I then thought to myself, "well I know what it does. Just run your own code"
6. So in the terminal I ran `echo I am user bandit23 | md5sum | cut -d ' ' -f 1` which gave me the hash of what would have been the new directory: `8ca319486bfbbc3663ea0fbe81326349`
7. I then ran `cat /tmp/8ca319486bfbbc3663ea0fbe81326349` and got the password: `0Zf11ioIjMVN551jX3CmStKLYqjk54Ga`

Level 23:
1. This level really didn't make sense to me. After hours of thinking this over, I just looked up a guide. I know that, in the spirit of hacking and pentesting, there shouldn't be much hand holding. But for educational purposes, I am not sure how I was ever supposed to come to the solution with the limited direction that this game provided. 
2. I got the first half correct on my own. Firstly `cat cronjob_bandit24` to learn the location of the script
3. `cat /usr/bin/cronjob_bandit24.sh`
4. This shows me the script:
```
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
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
 
5. I understand what the script is doing. It's traveling to `/var/spool/bandit24/foo` and deleting everything in that directory that does not belong to bandit24. 
6. Out of curiosity, I tried going to `/var/spool/bandit24/foo` to see what was there with a `ls -a`, but it was denied. 
7. At this point I tried so many things. At one point I got close by creating a script in the /tmp/ directory and tried `chmod 777` to get it to run. Which it would but I didn't know what kind of script I needed to make. 
8. After looking up the guide, the next step was `echo "cat /etc/bandit_pass/bandit24 > /tmp/customscript.txt" > customerscript.sh`
9. Then `chmod 777 customscript.sh`
10. `cat /tmp/customscript.sh` which gives: `gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8`
11. I get what each of those steps do. But I don't get why it works. There is no explanation as to what kind of exploit this is. When you run the first part of the command `cat /etc/bandit_pass/bandit24`, you get permissions denied. I don't understand why that is denied, but how it's okay to pipe the output of that command into a txt file. I wish there was at least some kind of breakdown as to why that works. 

Level 24:
1. This level at a glance seems a lot easier than the previous. I first checked that I had the right idea by opening a connection using `Netcat 127.0.0.1 30002`
2. I got the correct response: `I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.`
3. I then put in the current password and a random pin to see what, if any, errors might come back. Perhaps something to give me a clue on formatting or something. It just said wrong, try again. No worries. 
4. I navigated to /tmp where I had permissions to make a file and created a file called `brute.sh`
5. Turns out someone already did this, so when I went to `nano` "my" file, I saw someone else work haha. I saw some of it, but quickly closed and deleted it so I could do this on my own. 
6. I had to google some syntax stuff for bash but my main idea ended up being correct. And my code was WAY smaller than the predecessor file, which felt good. 
7. 
```
#!/bin/bash
for pin in {0000..9999}; do
        echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $pin" 
done | nc 127.0.0.1 30002
```

8. Running the script iterates and stops once the password is returned: `iCi86ttT4KSNe1armKiwbQNmB3YJP3q4`

Level 25:
1. Immediately upon reading this, it reminds me of the privilege escalation exploit using vim that I did on TryHackMe. So I will try that first. 
2. That was not the case because we don't have access to the Bandit26 account (even though there are the ssh keys in the home directory). So it looks like we still need to break into this account. 
3. I use `cat /etc/passwd` to show the users and their shell which returns: `bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext`
4. I try `cat /usr/bin/showtext` and get: 
```
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```
5. From here, I thought as hard as I could for an entire morning, but eventually I had to look up a guide. And I was shocked at what I found. I had to shrink the size of my terminal so that the `exec more ~/text.txt` in the script would trigger. 
6. After that, I could break out of the shell by typing `v` to enter vim. 
7. Once in Vim, I could choose a different file to edit, and since I was logged in as Bandit26, I could do `e: /etc//bandit_pass/bandit26` which did open up and revealed: `s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ`
8. 
> [!NOTE] Fascinating level
> While I did have to look up a guide to this particular problem, I was kind of on the right track in the beginning. I was breaking out using Vim. I just didn't know how to trigger it and my mind is still kind of blown that something like the size of the terminal could be used in an exploit. I didn't get the satisfaction of figuring it out but I will never forget this level for how it blew my mind

Level 26:
1. I logged back in and got back into Vim like last time. 
2. I spent a lot of time trying to figure out what Vim could do. I tried searching around the file system, which I could do, but it was not getting me very far as I did not have permissions. But I did find a binary file `bandit27-do` in the home directory! I made a note of that. 
3. Whenever I broke out of Vim, it was sending me back to the shell that would just close by itself. I forgot why for a bit. 
4. I did have to google, but I was googling ways to set up a shell and troubleshoot getting the shell from Vim. I found out that I could set the shell from Vim with `:set shell=/bin/bash`
5. Now when I tried `:!sh` again, I actually got a shell!
6. Remembering what I found, I tried executing that binary. It was in the directory I was in when I did an `ls`, so that was handy. 
7. I tried running it and it said "Try running command as another user". I had to google this, thinking I needed to run the binary as a different user. Then, I realized that the binary is the thing executing as a different user. So I just needed to have the binary read out the password from the expected directory with `./bandit27-do cat /etc/bandit_pass/bandit27`.
8. password: `upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB`

Level 27:
1. Had to get my syntax for Git right so I could specify the correct port. I did `git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo /tmp/level27`
2. I went ahead and `cd` into the repo and started poking around. 
3. First tried `cat README` right there in the root of the repo and found the password: `Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN`

Level 28:
1. `git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo /tmp/level28`
2. `cd` into the repo folder and started poking around. 
3. `cat README.md` and it showed the user and password, but the password was just `xxxxxxxxxxxx`
4. I was `cat` many files, and finally took a look at `packed-refs` and saw something that kind of looked like a password. I opened a new terminal and tried using it to login to the next level. 
5. It did not work, so I kept looking. 
6. went in to `cat /logs/HEAD` and found something interesting: `817e303aa6c2b207ea043c7bba1bb7575dc4ea73 Ben Dover`
7. Tried using that to login. No dice. 
8. I ran `grep -rnw 'tmp/level28/' -e 'bandit29'`, and looked for bandit29 but also "password" and "username" and some others but the only thing that came up was that initial README.md file. 
9. New idea. maybe the password is actually all those x's. So I tried that. It was incorrect. 
10. I went back into the README.md file and tried messing with it. It is editable which is interesting. After thinking a little harder, I figured this would have to be Git related and tried to pull the Git log. 
11. `git log -p` gave the password: `4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7`

Level 29:
1. `git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo /tmp/level29`
2. It had another README.md so I opened it. The password was listed right in the file so I tried it for the next level and it worked
3. password: `qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL`
4. Circling back, obviously this is because someone else left their work open and it didn't get cleaned up. I have to assume that the answer to the room would be to pull an older version of the repo to get the password that was cleaned up. 
5. So I did `git checkout` on the older hash, and indeed that was the case. 

Level 30:
1. `git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo /tmp/level30`
2. Checked the root folder and, again, README.md file. `cat README.md` gave me `just an epmty file... muhahahah`... interesting. 
3. I checked the logs and there was only the one commit. 
4. Performed a `git checkout acfc3c67816fc778c4aeb5893299451ca6d65a78` and checked the README.md file again and it was the same message. 
5. Tried `git log -p` for more output and can see that there is a difference in the README.md file. 
6. Tried `git branch -r`, but nothing interesting was returned
7. Tried `git tag` and `secret` came up
8. Tried `git show secret` and got the password: `fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy`

Level 31:
1. downloaded the repo and opened the README.md
2. ```
```
This time your task is to push a file to the remote repository.
	Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

3. Made file with `touch key.txt` and added the text with `nano`
4. Tried `git add key.txt` but it through an error saying it was not allowed. 
5. I deleted the .gitignore file in the repo first then tried again. success. 
6. `git commit -m "uploading a file`
7. `git push origin master`
8. Looks like the files got validated but they were wrong and it threw an error telling me to try again. 
9. I went back into my file to make sure I copied the content right. I left the quotes in so I tried taking those out. 
10. performed `add`, `commmit`, and `push` again and this time got the password: `3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K`

Level 32:
1. The final level. An escape level. I noticed when logging in that it said, "welcome top the uppercase shell"
2. I tried `pwd` and `ls` and got `permission denied`
3. I spammed all kinds of commands but they were all denied. 
4. i did google if "uppercase shell" meant anything, but didnt see any results that suggested it did. And I didnt want a guide for the last level so I went back to thinking. 
5. I noticed that it is converting all my commands to uppercase before telling me that they are denied. 
6. the `#` followed by anything is allowed by itself and does not get rejected. Interesting.
7. `~` reveals that there is the directory `/home/bandit32` in the filesystem. It is just denied to us. 
8. `./*` reveals the message `WLECOME TO THE UPPERCASE SHELL`. So it looks like we can execute scripts. But I am not sure how to find them. It seems like I have to breakout using only symbols. So i start googling symbols. 
9. `$PATH` gives us `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin: not found`
10. `$$` gives us the current process ID `2273428` of the shell itself
11. GOT IT! `$0` breaks out of the shell and allows us to run commands in lowercase
12. There is not a next room, supposedly, so I am not sure if there is a password to get, but I am looking around. 
13. `cat /etc/bandit_pass/bandit33` gets it! `tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0`

level 33:
1. `cat README.txt`
2. "Congratulations on solving the last level of this game!

	At this moment, there are no more levels to play in this game. However, we are constantly working
	on new levels and will most likely expand this game with more levels soon.
	Keep an eye out for an announcement on our usual communication channels!
	In the meantime, you could play some of our other wargames.

	If you have an idea for an awesome new level, please let us know!"
3. FEELS GOOD!


