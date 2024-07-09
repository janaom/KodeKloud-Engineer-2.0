# Instructions

The Nautilus application development team was working on a git repository `/opt/official.git` which is cloned under `/usr/src/kodekloudrepos`  directory present on `Storage server` in `Stratos DC`. The team want to setup a hook on this repository, please find below more details:

- Merge the `feature` branch into the `master` branch`, but before pushing your changes complete below point.
- Create a `post-update` hook in this git repository so that whenever any changes are pushed to the `master` branch, it creates a release tag with name `release-2023-06-15`, where `2023-06-15` is supposed to be the current date. For example if today is `20th June, 2023` then the release tag must be `release-2023-06-20`. Make sure you test the hook at least once and create a release tag for today's release.
- Finally remember to push your changes.


# Solution

`ssh natasha@ststor01`

`sudo su -`

`vi /opt/official.git/hooks/post-update`

`chmod +x /opt/official.git/hooks/post-update`

```sh
#!/bin/bash

# Get the current branch
current_branch=$(git symbolic-ref HEAD 2>/dev/null | cut -d"/" -f 3)

# Check if the current branch is master
if [ "$current_branch" == "master" ]; then
  # Get the current date in the format YYYY-MM-DD
  current_date=$(date +'%Y-%m-%d')

  # Define the tag name
  tag_name="release-$current_date"

  # Create the tag
  git tag $tag_name

  # Push the tag to the remote repository
  git push origin $tag_name
fi
```

`cd /usr/src/kodekloudrepos/official`

`git checkout master`

`git pull`

`git merge feature`

`git push origin master`

You should see a similar result:

```bash
[root@ststor01 official]# git pull
From /opt/official
 * [new tag]         release-2024-07-09 -> release-2024-07-09
```
