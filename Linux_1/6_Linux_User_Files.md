# Instructions

There was some users data copied on `Nautilus App Server 1` at `/home/usersdata` location by the `Nautilus` production support team in `Stratos DC`.
 Later they found that they mistakenly mixed up different user data there. Now they want to filter out some user data and copy it to another location. Find the details below:

On `App Server 1` find all files (not directories) owned by user `javed` inside `/home/usersdata` directory and copy them all `while keeping the folder structure` (preserve the directories path) to `/blog` directory.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

Check files: `ls -la | grep javed`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b7396938-be06-493a-988a-2cb5b57f32fb)


Run this command: `find /home/usersdata -type f -user javed -exec cp --parents {} /blog \;`

Let's break down the command:

- `find`: The command used to search for files and directories.
- `/home/usersdata`: The directory where the search will be performed.
- `type f`: Specifies that only files should be considered, excluding directories.
- `user javed`: Filters the search to files owned by the user "javed".
- `exec cp --parents {} /blog \;`: Executes the `cp` command to copy the found files to the "/blog" directory while preserving the directory structure. The `{}` represents the found files, and `-parents` ensures that the directories' path structure is maintained during the copy.
