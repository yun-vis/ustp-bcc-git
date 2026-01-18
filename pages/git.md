---
# permalink: /404.html
layout: single
classes: wide
title: "GitHub"
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2026-01-17
---

# Basics

## Check out a remote Git branch

```bash
# List all remote branches
$ git branch -r
# List all remote and local branches
git branch -a 
# List all local branches
git branch 

# Checkout a remote branch as a local branch
$ git checkout -b local_branch_name origin/remote_branch_name
```

## Merge the changes from the master to a branch

```bash
$ git pull origin master
```

## Check out/switch a local Git branch

```bash
# Create a new branch
$ git checkout -b myNewBranchName
# Switch to an existing branch
$ git checkout myBranchName
```

## Remove a local branch

```bash
$ git branch -D myLocalBranchName
```

## Remove a remote branch

```bash
git push origin --delete myRemoteBranchName
```

## Add a tag

```bash
$ git tag -a v2.0.0 -m "release v2.0.0"
$ git push origin v2.0.0
```

## Delete a tag

```bash
# delete a local tag
git tag -d v2.0.0
# delete a remote tag
git push origin --delete v2.0.0
```
