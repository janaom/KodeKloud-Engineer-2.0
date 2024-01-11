# Instructions

There is some data on `Nautilus App Server 3` in `Stratos DC`. Data needs to be altered in several of the files. On `Nautilus App Server 3`, alter the `/home/BSD.txt` file as per details given below:

a.  Delete all lines containing word `following` and save results in `/home/BSD_DELETE.txt` file. (Please be aware of case sensitivity)

b.  Replace all occurrence of word  `the` to `their` and save results in `/home/BSD_REPLACE.txt` file.

`Note:`  Let's say you are asked to replace word `to` with `from`. In that case, make sure not to alter any words containing the string itself; for example up**to**, contribu**to**r etc.

# Solution

ssh banner@stapp03

sudo su -

This command reads the contents of the file `/home/BSD.txt` and filters out the lines that contain the word "following", displaying them as output: `cat /home/BSD.txt |grep following`

This command uses the `sed` command to delete lines containing the word "following" from the file `/home/BSD.txt`. The filtered content is then redirected to the file `/home/BSD_DELETE.txt`: `sed '/\<following\>/d' /home/BSD.txt > /home/BSD_DELETE.txt`

This command reads the contents of the file `/home/BSD_DELETE.txt` and filters out the lines that contain the word "following", displaying them as output: `cat /home/BSD_DELETE.txt |grep following`

This command reads the contents of the file `/home/BSD.txt` and filters out the lines that contain the word "the", displaying them as output: `cat /home/BSD.txt |grep the`

This command uses the `sed` command to replace all occurrences of the word "the" (considered as a whole word) with the word "their" in the file `/home/BSD.txt`. The modified content is then redirected to the file `/home/BSD_REPLACE.tx`: `sed 's/\bthe\b/their/g' /home/BSD.txt > /home/BSD_REPLACE.txt`

This command reads the contents of the file `/home/BSD_REPLACE.txt` and filters out the lines that contain the word "the", displaying them as output: `cat BSD_REPLACE.txt |grep the`
