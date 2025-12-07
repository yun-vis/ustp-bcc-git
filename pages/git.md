---
# permalink: /404.html
layout: single
classes: wide
title: "GitHub"
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-12-05
---

# Basics

## Check out a remote Git branch

```bash
# List all remote branches
$ git branch -r
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