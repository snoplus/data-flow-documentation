---
layout: default
title: Git interaction
nav_exclude: true
---

## Git interaction

Production accounts have no ssh keys tied to them.

To clone a repository, you must use the git URL rather than the typical git@github.com syntax. For example, to clone rat:
```bash
git clone https://github.com/snoplus/rat
```
There are many git tutorials online to help you understand the interaction with git. For details on the preferred git hub interaction for SNO+, see [DocDB 1462](https://www.snolab.ca/snoplus/private/DocDB/cgi/ShowDocument?docid=1462).

-----
You can use `git pull ssh master` on Cedar to prevent breaking git after we source our environment script.
