---
# permalink: /404.html
layout: single
classes: wide
title: "GitHub Packages"
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-12-16
---

# GitHub Repository Rulesets

## Add settings.yml file

in .github/settings.yml
```yaml
# These settings are synced to GitHub by https://probot.github.io/apps/settings/

repository:
  # See https://docs.github.com/en/rest/reference/repos#update-a-repository for all available settings.

  # The name of the repository. Changing this will rename the repository
  name: ustp-cardgame-test

  # A short description of the repository that will show up on GitHub
  description: description of repo

  # A URL with more information about the repository
  homepage: https://yun-vis.net/ustp-cardgame-test/

  # A comma-separated list of topics to set on the repository
  topics: github, probot

  # Either `true` to make the repository private, or `false` to make it public.
  private: false

  # Either `true` to enable issues for this repository, `false` to disable them.
  has_issues: true

  # Either `true` to enable projects for this repository, or `false` to disable them.
  # If projects are disabled for the organization, passing `true` will cause an API error.
  has_projects: true

  # Either `true` to enable the wiki for this repository, `false` to disable it.
  has_wiki: true

  # Either `true` to enable downloads for this repository, `false` to disable them.
  has_downloads: true

  # Updates the default branch for this repository.
  default_branch: master

  # Either `true` to allow squash-merging pull requests, or `false` to prevent
  # squash-merging.
  allow_squash_merge: true

  # Either `true` to allow merging pull requests with a merge commit, or `false`
  # to prevent merging pull requests with merge commits.
  allow_merge_commit: true

  # Either `true` to allow rebase-merging pull requests, or `false` to prevent
  # rebase-merging.
  allow_rebase_merge: true

  # Either `true` to enable automatic deletion of branches on merge, or `false` to disable
  delete_branch_on_merge: true

  # Either `true` to enable automated security fixes, or `false` to disable
  # automated security fixes.
  enable_automated_security_fixes: true

  # Either `true` to enable vulnerability alerts, or `false` to disable
  # vulnerability alerts.
  enable_vulnerability_alerts: true

# Labels: define labels for Issues and Pull Requests
labels:
  - name: bug
    color: CC0000
    description: An issue with the system 🐛.

  - name: feature
    # If including a `#`, make sure to wrap it with quotes!
    color: '#336698'
    description: New functionality.

  - name: Help Wanted
    # Provide a new name to rename an existing label
    new_name: first-timers-only

# Milestones: define milestones for Issues and Pull Requests
milestones:
  - title: milestone-title
    description: milestone-description
    # The state of the milestone. Either `open` or `closed`
    state: open

# Collaborators: give specific users access to this repository.
# See https://docs.github.com/en/rest/reference/repos#add-a-repository-collaborator for available options
collaborators:
  - username: Aruaci
    permission: admin

  - username: ViktorHofer
    permission:

  # - username: bkeepers
  #   permission: push
  # - username: hubot
  #   permission: pull

  # Note: `permission` is only valid on organization-owned repositories.
  # The permission to grant the collaborator. Can be one of:
  # * `pull` - can pull, but not push to or administer this repository.
  # * `push` - can pull and push, but not administer this repository.
  # * `admin` - can pull, push and administer this repository.
  # * `maintain` - Recommended for project managers who need to manage the repository without access to sensitive or destructive actions.
  # * `triage` - Recommended for contributors who need to proactively manage issues and pull requests without write access.

# See https://docs.github.com/en/rest/reference/teams#add-or-update-team-repository-permissions for available options
teams:
  - name: core
    # The permission to grant the team. Can be one of:
    # * `pull` - can pull, but not push to or administer this repository.
    # * `push` - can pull and push, but not administer this repository.
    # * `admin` - can pull, push and administer this repository.
    # * `maintain` - Recommended for project managers who need to manage the repository without access to sensitive or destructive actions.
    # * `triage` - Recommended for contributors who need to proactively manage issues and pull requests without write access.
    permission: admin
  - name: docs
    permission: push

branches:
  - name: master
    # https://docs.github.com/en/rest/reference/repos#update-branch-protection
    # Branch Protection settings. Set to null to disable
    protection:
      # Required. Require at least one approving review on a pull request, before merging. Set to null to disable.
      required_pull_request_reviews:
        # The number of approvals required. (1-6)
        required_approving_review_count: 1
        # Dismiss approved reviews automatically when a new commit is pushed.
        dismiss_stale_reviews: true
        # Blocks merge until code owners have reviewed.
        require_code_owner_reviews: true
        # Specify which users and teams can dismiss pull request reviews. Pass an empty dismissal_restrictions object to disable. User and team dismissal_restrictions are only available for organization-owned repositories. Omit this parameter for personal repositories.
        dismissal_restrictions:
          users: []
          teams: []
      # Required. Require status checks to pass before merging. Set to null to disable
      required_status_checks:
        # Required. Require branches to be up to date before merging.
        strict: true
        # Required. The list of status checks to require in order to merge into this branch
        contexts: []
      # Required. Enforce all configured restrictions for administrators. Set to true to enforce required status checks for repository administrators. Set to null to disable.
      enforce_admins: true
      # Prevent merge commits from being pushed to matching branches
      required_linear_history: true
      # Required. Restrict who can push to this branch. Team and user restrictions are only available for organization-owned repositories. Set to null to disable.
      restrictions:
        apps: []
        users: []
        teams: []
```

## Create CODEOWNERS

in .github CODEOWNERS
```
/.github/settings.yml @yun-vis @Aruaci
```

# GitHub Packages

GitHub Packages is a software package hosting service that allows you to host your software packages privately or publicly and use packages as dependencies in your projects. Support Npm, Nuget, Maven, Gradle, RubyGems, Docker.

- GitHub's APIs, GitHub Actions, and webhooks 

## Package vs. Container Registry

Github Container Registry is desgined for container images


# Concept

- [Package registry]: A package registry is a centralized, versioned repository for software components (packages) and their metadata, acting as a library where developers can find, store, and share reusable code, ensuring easy installation, dependency management, and secure supply chains for projects like npm for JavaScript, Maven for Java, or PyPI for Python. It's a key part of software development, allowing teams to manage dependencies and distribute their own private or public packages. 


## Docker

What is Docker? Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker's methodologies for shipping, testing, and deploying code, you can significantly reduce the delay between writing code and running it in production.

- [What is an image? | Docker Docs](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/)

- [What is a container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)

# Terminology

- GitHub Container Regisry (GHCR): The Container registry stores container images within your organization or personal account, and allows you to associate an image with a repository.

- PAT (Personal Access Token)

- Docker Compose: Docker Compose is a tool for defining and running multi-container applications. It is the key to unlocking a streamlined and efficient development and deployment experience.

- codeowner

- Require review from Code Owners
- Rulesets -> Require a pull request before merging -> Require review from Code Owners
Not using branch protection anymore... the old way

# References

- [GitHub Codespaces](https://github.com/features/codespaces)

- [GitHub Discussions](https://github.com/features/discussions) 
  Discussions need to be enabled

- [Introducing draft pull requests](https://github.blog/news-insights/product-news/introducing-draft-pull-requests/)

- [The github.dev web-based editor](https://docs.github.com/en/codespaces/the-githubdev-web-based-editor)
press period to enable *.dev vs. codespaces

- [Introduction to GitHub Packages](https://docs.github.com/en/packages/learn-github-packages/introduction-to-github-packages)
  study github packages until next week

- [GitHub-hosted runners](https://docs.github.com/en/actions/concepts/runners/github-hosted-runners) 
  github-hosted runner: run on my own machines

- [Dependabot quickstart guide](https://docs.github.com/en/code-security/getting-started/dependabot-quickstart-guide)

- Docker installation: https://www.youtube.com/watch?v=740YJZZu7QY

