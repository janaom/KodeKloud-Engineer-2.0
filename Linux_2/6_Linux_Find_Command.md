# Instructions

During a routine security audit, the team identified an issue on the Nautilus App Server. Some malicious content was identified within the website code. After digging into the issue they found that there might be more infected files. Before doing a cleanup they would like to find all similar files and copy them to a safe location for further investigation. Accomplish the task as per the following requirements:

a. On `App Server 2` at location `/var/www/html/media` find out all files (not directories) having `.php` extension.

b. Copy all those files along with their `parent directory structure` to location `/media` on same server.

c. Please make sure not to copy the entire `/var/www/html/media` directory content.

# Solution

ssh steve@stapp02

Find all files with a .php extension in the /var/www/html/media directory:`find /var/www/html/media -type f -name "*.php"`

sudo su

Copy the files to the /media directory on the same server while preserving the directory structure:

mkdir -p /media

find /var/www/html/media -type f -name "*.php" -exec cp --parents {} /media \;

This command creates the /media directory (`mkdir -p /media`) if it doesn't exist. Then, it uses `find` to search for files with the .php extension and copies them to the /media directory while preserving the directory structure (`-exec cp --parents {} /media \;`).

Verify that the files and their parent directory structure were copied to the /media directory: `ls -R /media`
