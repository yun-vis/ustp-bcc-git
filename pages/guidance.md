---
# permalink: /404.html
layout: single
classes: wide
title: "Convention"
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-12-03
---

# How to write a commit message

- [Conventional Commits](https://www.conventionalcommits.org)

```bash
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

For example,
```bash
refactor(user-domain): apply OOP principles to User hierarchy

Introduced an abstract base class `UserBase` to centralize shared
properties and behavior. Created derived classes `AdminUser`
and `RegularUser` to better model domain responsibilities.
Updated service layer to depend on the base abstraction.

No functional changes expected.
```