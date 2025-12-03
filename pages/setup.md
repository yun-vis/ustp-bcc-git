---
# permalink: /404.html
layout: single
classes: wide
title: "Git"
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-11-26
---

# Installation

## Windows

Download and run the distribution from [https://git-scm.com/](https://git-scm.com/)
[Git SCM to Windows](https://gitforwindows.org/)

# Git Usage

- [Git Cheat Sheet (git-scm)](https://git-scm.com/cheat-sheet)
- [GIT CHEAT SHEET (GitHub)](https://education.github.com/git-cheat-sheet-education.pdf)

## Some helpful commands

- $ diff filea.py fileb.py
- $ diff -u filea.py fileb.py
- $ diff filea.py fileb.py  // create *.diff 
- $ patch filea.py < fileb.diff 
- [diff](http://man7.org/linux/man-pages/man1/diff.1.html)
- [patch](http://man7.org/linux/man-pages/man1/patch.1.html)


# WSL (runing Windows Subsystem for Linux)

- [Installation](https://learn.microsoft.com/en-us/windows/wsl/)
- Issue: for AMD CPU, one needs to enable virtualization in BIOS
  - Enter BIOS setting -> Advanced CPU Settings -> enable SVM Mode
- Install "Remote Development" extension from VS Code
- Click on the extension icon at left-bottom and connect to WSL
- Open the folder -> click on "Show Local"

# Exercises

- [LeetCode](https://leetcode.com/)
- [HackerRank](https://www.hackerrank.com/)

# References

- [Pro Git](https://git-scm.com/book/en/v2)
- [Git tutorial](https://git-scm.com/docs/gittutorial)