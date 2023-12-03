# Instructions

The backup server in the `Stratos DC` contains several template XML files used by the Nautilus application. However, these template XML files must be populated with valid data before they can be used. One of the daily tasks of a system admin working in the xFusionCorp industries is to apply string and file manipulation commands!

Replace all occurances of the string `About` to `Marine` on the XML file `/root/nautilus.xml` located in the backup server.

# Solution

ssh into backup server: `ssh clint@172.16.238.16`

To check if the file exists, run this: `sudo bash -c 'if [ -f /root/nautilus.xml ]; then echo "File exists."; else echo "File does not exist."; fi'`

Or become the root: `sudo su -`

Check all the lines in the "/root/nautilus.xml" file that include the word "About”: `cat /root/nautilus.xml  |grep About`

Use the following command to replace all occurrences of the string "About" with "Marine" in the XML file "/root/nautilus.xml": `sed -i 's/About/Marine/g' nautilus.xml`

- `sed`: A command-line tool for stream editing.
- `i`: Modifies the file in-place (i.e., edits the file directly).
- `'s/About/Marine/g'`: The `s` command in `sed` is used for substitution. It replaces all occurrences of "About" with "Marine" in each line of the file. The `g` flag ensures that all occurrences are replaced, not just the first occurrence.

Check the result

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/e1933d9d-b413-48ed-a703-42b3f5041bd5)
