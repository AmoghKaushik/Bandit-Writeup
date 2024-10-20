# Bandit-Writeup
Over the wire: Bandit

I had never used the Linux command line interface before this, but after a fair bit of reading about how to install WSL and some minor technical issues I was able to run Ubuntu just fine.

Level 0

The introduction involved logging in into the game server using ssh. I learnt how to use ssh from wikihow and connected with the username bandit0 on bandit.labs.overthewire.org on port 2220. I typed in the command ‘ssh bandit0@bandit.labs.overthewire.org -p 2220’ after which I entered the password bandit0 when prompted.
To move onto the next level,I had to find the password which was located in the file called readme to find which I needed to use the ls command which lists the contents of the directory.
I next used the cat command to read the contents of the file readme after which I got the password to move on to the next level: NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL

Level 1

I exited level 0 and entered the ssh command to get to level 1. To move onto level 2 I needed to find the required password which was stored in the file named ‘-’ .
First I used the command cat - but that didn’t work, then I learned about dashed filenames and entered the command cat ./ - and that worked and hence I received the password to move onto the next level :rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi

Level 2

I needed to find the password to move onto the next level from a file called ‘spaces in the filename’. I used the command cat spaces in this filename and of course that didn’t work. The system treated every word separated by a space as its own filename or directory.
I then learned that using a backslash before every space in the filename can help me avoid this and so I entered the command  cat spaces\ in\ this\ filename which gave me the password to move onto the next level: aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG

Level 3

Now the password I needed was in a hidden file in the inhere directory, I first used the command ls to show the contents of the home directory then changed the working directory to inhere using the command cd inhere. I used the command ls in order to see the contents of the directory and couldn’t see anything as the file I needed was hidden.
I then used the command ls -a which showed me even the hidden files. I could see a file named .hidden. I the entered the command cat .hidden to get the password for the next level: 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe

Level 4

I was stuck on this for a while but after a bit of research, I found that the command file ./* shows the type of each file in the current directory and thus I was able to find out that the only human readable file (which was of the type ASCII text) in the inhere directory was ‘-file07’. After using the command cat ./-file07, I received the password to move onto the next level: lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR

Level 5

This level required me to use the find command to find a file of size 1033 bytes. I learned that the find command can be used with suitable options such as -size and -type to get the required results, I also learned that the option ! - executable can help me find non executable files as needed in the level. For this, I entered in the command:
find -size 1033c -type f ! -executable, which gave me the required file and the directory: ./maybehere07/.file2. Here ‘c’ specifies bytes instead of ‘b’ and ‘f’ specifies standard files. 
Entering the command cat ./maybehere07/.file2 gets me the password: P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU

Level 6

This level required me to find a file which was owned by a specific group and a user and was of specific size.
I used the options -user and -group to do so and entered the command: 
find -type f -size 33c -user bandit7 -group bandit6 which didn’t give me anything after which I learned using a ‘/’ will enable me to search in the full server starting from the parent directory instead of limiting me to search in the current directory which was what i initially tried.
Thus , I now used the command find / -type f -size 33c -user bandit7 -group bandit6 which gave me the required file: /var/lib/dpkg/info/bandit7.password, after using the command:
cat /var/lib/dpkg/info/bandit7.password, i received the password for the next level: z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S

Level 7

I needed to find the password which was stored in the file data.txt next to the word millionth. I used the grep command to get this one as data.txt ended up being a very long file so it was not viable to manually find the entry. I thus used the command: grep millionth data.txt and got the password for the next level:  TESKZC0XvTetK0S9xNwm25STk5iWrBvP

Level 8

This level required a unique line for the password and thus I used the sort command which sorts all lines alphabetically with uniq -u which displays only unique commands and not the repeated ones. The command I entered was: sort data.txt | uniq -u, which gave me the password as EN632PlfYiZbn3PhVK3XOGSlNInNE00t

Level 9

I had to find the password in data.txt in the few human readable strings preceded by several ‘=’ characters. I used the strings -a command with grep “=”. Strings separated the printable strings from the non printable ones and then grep sorted out all the strings with ‘=’ characters. The command entered was: strings -a data.txt | grep “=”. And the password for the next level was achieved: G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s

Level 10

For this level, I visited the base64 wikipedia page and some other sources to learn about it. I learned about the command base64 -d which is used to decode data and thus I used it as 
cat data.txt | base64 -d to get the password for the next level:  6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM

Level 11

This question presented a unique case so far where all alphabets were rotated by 13 positions. I came to know that this is known as the Caesar cypher (after Julius Caesar).
Initially, I was considering only using cat command to print the data in the file and then manually decoding it, but that was time consuming. Instead, I thought that I could use the tr command to decode it. 
It took me a bit of time but I could finally understand after seeing examples as to how to do it.
The command I entered was: tr ‘A-Za-z’ ‘N-ZA-Mn-za-m’ which gave me the password: JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv

Level 12

This was a frustrating level, at first, I had to read through several sources of information regarding hexdump and the xxd command, even after which the system denied me the permission of the command I wanted to execute. I realised I needed to work in the required directory for my command to work where I would have adequate permission. After a lot of trial and error (and failure), I finally found how to make it work: 
mkdir /tmp/whosthis
cp data.txt /tmp/whosthis
xxd -r data.txt > xyz
After this, I needed to decompress the file but apparently it didn’t work with .txt extension, i had to use the mv command to change the extension to .gz, after which it worked as:
mv xyz xyz.gz
file xyz
The file command told me the extension now was gzip so I used the command:
gzip -d xyz.gz 
I now had a file named xyz which was still compressed, but now in bzip2 format, I used the commands:
mv xyz xyz.bz2
bzip2 -d xyz.bz2
Again the file was compressed, now in gzip format, now the commands were,
mv xyz xyz.gz
gzip -d xyz.gz
I checked the format of the file again, it showed tar archive, after reading a bit about tar, i entered the command: 
mv xyz xyz.tar
tar -xf xyz.tar 
ls
I could see a file named data5.bin which was also tar archive, so I repeated the required command for it,
mv data5.bin data5.tar
tar -xf data5.tar
This now gave me data6.bin which was a bzip2 file, so I did the required:
mv data6.bin data6.bz2
bzip2 -d data6.bz2
again, it gave me a tar archive, so i entered: 
mv data6 data6.tar
tar -xf data6.tar
It now showed a gzip compressed file: data8.bin so I repeated the steps yet again:
mv data8.bin data8.gz
gzip -d data8.gz
Finally, i received a file named data8 which had the password: wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw

Level 13

After the painstakingly long level 12, level 13 was relatively easy. I learned that I could use the ssh -i command which is used for identification of the user by the server, it tells the server the user has the private ssh key for the next level even though not necessarily the password.
I typed in: ssh -i sshkey.private bandit14@localhost
For some reason, I was getting connected to port 22 which was not intended so I specified the port 2220 and entered the command: ssh -i sshkey.private bandit14@localhost -p 2220, which worked and I moved onto level 14.

Level 14

After entering the command: cat /etc/bandit_pass/bandit14, I was given the password for the next level: fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq. To get the password for the next level, it was required that I submit the password of the current level to port 30000 on localhost. I could accomplish this using the nc (netcat) command which is used to read and write on networks using Transmission control protocol(TCP) and User datagram protocols(UDP) : 
nc localhost 30000 and then entering the required password which gave the next level password as: jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt

Level 15

It took me a while to understand how the openssl command works but once I figured it out it was rather simple. I used the command: openssl s_client -connect localhost:30001
After which, I entered the current level password and received the password for next level: JQttfApK4SeyHwDlI9SXGR50qclOAil1

Level 16

I needed to scan ports in the range 31000-32000, so I used the command: 
nmap -p 31000-32000 -sV localhost
Here nmap stands for network mapping, -p is used to specify the range of ports, and -sV is used to identify versions of services.
This showed that port 31790 was a viable port and thus I used:
openssl s_client -connect localhost:31790
After which, I entered the password for the current level but didn’t receive a password, I received an RSA key instead because of which I was a little confused as to what to do next.
I had to look up a guide for getting further. After lot of effort, i learnt i had to create a file using touch command and then use the vim text editor to save it and then use openssl command to get to the next level.




