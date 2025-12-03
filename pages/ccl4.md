---
# permalink: /404.html
layout: single
classes: wide
title: "GitHub"
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-11-26
---

# Shared repository model

In the shared repository model, collaborators are granted push access to a single shared repository and topic branches are created when changes need to be made. Pull requests are useful in this model as they initiate code review and general discussion about a set of changes before the changes are merged into the main development branch. This model is more prevalent with small teams and organizations collaborating on private projects.

# Best Practices for Git Collaboration

- Commit Messages Matter
  - Use action words
  - Keep it short and precise
- Branching Strategy
  - Trunk-Based Development: Developers work on small changes directly in the main branch, which is great for continuous integration.
- Code Reviews with Pull Requests
- Keep Repositories Clean
  - Delete old branches that are no longer needed.
  - Avoid pushing large files by adding them to your .gitignore.
  - Use git rebase to keep commit history clean and avoid unnecessary merge commits.
  - Keeping your repository tidy is essential for better performance and smoother collaboration!

# References

- [About collaborative development models](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/getting-started/about-collaborative-development-models)
- [Best Practices for Git and GitHub Collaboration](https://medium.com/@codenova/best-practices-for-git-and-github-collaboration-348cc3b774d1)
