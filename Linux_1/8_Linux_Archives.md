# Instructions

On `Nautilus` storage server in `Stratos DC`, there is a storage location named `/data`, which is used by different developers to keep their data (non confidential data). One of the developers named `yousuf` has raised a ticket and asked for a copy of their data present in `/data/yousuf` directory on storage server. `/home` is a FTP location on storage server itself where developers can download their data. Below are the instructions shared by the system admin team to accomplish this task.

a. Make a `yousuf.tar.gz` compressed archive of `/data/yousuf` directory and move the archive to `/home` directory on Storage Server.

# Solution

ssh into the Nautilus Storage Server: `ssh natasha@172.16.238.15`

Check the files: `cd /data/yousuf`

To create a compressed archive named "yousuf.tar.gz" of the "/data/yousuf" directory and move it to the "/home" directory on the Storage Server, you can use the following commands: 

`tar -czvf yousuf.tar.gz /data/yousuf`

tar -czvf yousuf.tar.gz /data/yousuf: This command creates a compressed tar archive named "yousuf.tar.gz" of the "/data/yousuf" directory. Here's what each option does:

    -c: Create a new archive.
    -z: Compress the archive using gzip.
    -v: Verbose mode, display the progress and details of the archiving process.
    -f: Specify the name of the archive file.


![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/48ac523e-7df5-41d7-b82d-b289e8181c35)


`sudo mv yousuf.tar.gz /home`

Check files in /home: `cd /home`
