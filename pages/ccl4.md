---
# permalink: /404.html
layout: single
classes: wide
title: "CCL4 (GitLab)"
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2026-05-21
---

# Git Collaboration Concept

## Shared repository model

In the shared repository model, collaborators are granted push access to a single shared repository and topic branches are created when changes need to be made. Pull requests are useful in this model as they initiate code review and general discussion about a set of changes before the changes are merged into the main development branch. This model is more prevalent with small teams and organizations collaborating on private projects.

## Best Practices for Git Collaboration

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

## References

- [About collaborative development models](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/getting-started/about-collaborative-development-models)
- [Best Practices for Git and GitHub Collaboration](https://medium.com/@codenova/best-practices-for-git-and-github-collaboration-348cc3b774d1)


# GitLab Page Setup in CCLs

1. Visit the [USTP GitLab](https://git.nwt.fhstp.ac.at/) and login with your cridential
2. Home -> Project -> New Project > Create from template -> Pages/Plan HTML -> Do basic settings 
3. Enter the project main page -> Edit .gitlab-ci.yml using Edit in pipeline editor -> Up the content to
    ```bash 
    stages:
    - deploy

    pages:
      stage: deploy
      image: alpine:latest
      tags: ["docker"]
      script:
        - echo "Deploying GitLab Pages from ./public"
        - ls -la public
      artifacts:
        paths:
          - public
      rules:
        - if: '$CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH'
    ```
4. From the main project page -> Deploy -> Pages
5. Add a SSH key before cloning a project
    ```bash 
    $ git clone git@git.nwt.fhstp.ac.at:lbwu/mypage.git
    ```

# General GitLab Setup in CCLs

- Add a SSH key before cloning a project
  ```bash 
  $ git clone git@git.nwt.fhstp.ac.at:lbwu/mypage.git
  ```

1. Visit the [USTP GitLab](https://git.nwt.fhstp.ac.at/) and login with your cridential
2. Home -> Project -> New Project > Create from template -> Pages/Jekyll -> Do basic settings 
3. Bypass the onboarding wizard

In the left sidebar, go to Build > Pipelines.Click the Run pipeline button in the top right.Select your default branch (usually main or master) and click Run pipeline again

[Create a GitLab Pages website from a project template](https://docs.gitlab.com/user/project/pages/getting_started/pages_new_project_template/)


- CD/CI Settings:
  - Settings -> Ci/CD -> Runners -> Instance -> "Turn on instance runners for this project" = ON
  - Some settings in .gitlab-ci.yml are critical
    ```bash 
    default:
      image: docker:latest
      tags:
        - shared
    ```

# Unity and Wwise Integration

1. Download and Install [Unity Hub](https://docs.unity.com/en-us/hub) and install Unity version: Unity 6.3 LTS (6000.3.6f1) 
2. Download and Install [Audiokinetic Launcher](https://www.audiokinetic.com/en/download)
3. Launch Audiokinetic Launcher -> Wwise -> Install Wwise version 2025.1.4.9062
4. Audiokinetic Launcher -> Unity -> Select the project to integrate

Q: Does this need to be done at the very beginning of the project?
Q: How to ensure the same development environment setup work for all team members?

# Open Questions

- Do we put GitLab pages as a sub directory (Public) in the main project repository?
- The project folder hierarchy
- Ask momo to add me to the eCampus page -> done
- How to setup a runner? -> infrastrucure problem. solved.
- CCL3 still allows github repository. 
  - Do we enforce CCL4 on gitlab or not? 
  - The GitLab CD/CI pipeline seems to be quite problematic for now.
  - How to get rid of the CD/CI onbroading wizard?
  - Jekyll/Hugo as Pages templates?
