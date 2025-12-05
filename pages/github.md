---
# permalink: /404.html
layout: single
classes: wide
title: "GitHub"
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-12-03
---

# Basics

- Pull request: A pull request is a proposal to merge a set of changes from one branch into another. In a pull request, collaborators can review and discuss the proposed set of changes before they integrate the changes into the main codebase.

Note: A pull request is a feature added by hosting platforms, such as:

- GitHub
- GitLab → Merge Request
- Bitbucket
- Azure DevOps

A pull request is a web-based workflow that allows you to:

- propose changes
- request review from other people
- run automated checks
- discuss changes
- merge changes into a branch (usually main)
- Git does not implement this — GitHub does.
 
# Create a Pull Request

- Push your local commits to your new branch:
  
```bash
git checkout -b feature/featureBranch
<make edits>
git commit -m "Add ruleset"
git push origin feature/featureBranch
```

Note: In Git branch names, the slash / doesn’t have special functional meaning — it’s simply used as a naming convention to organize branches into folders.

- After pushing a new branch, GitHub shows: Compare & pull request on the Code tag. Press it.
- If no issues appear, one can click on merging the featureBranch to the master branch

