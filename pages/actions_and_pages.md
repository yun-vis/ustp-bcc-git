---
layout: single
classes: wide
title: "GitHub Pages"
last_modified_at: 2025-11-28
---

# GitHub Actions for GitHub Pages

Below is a complete working GitHub Pages + GitHub Actions configuration using Minimal Mistakes as a remote theme, including:

- required _config.yml
- Gemfile
- GitHub Actions workflow
- repo structure
- optional configuration for collections, navigation, and plugins

## Step 1: Create a new GitHub repository

Go to the repository on GitHub:

Actions -> search GitHub Pages -> configure GitHub Pages Jekyll (no changes needed) -> commit changes

You will see a file .github/workflows/jekyll-gh-pages.yml is added to the repository
Clone the repository locally.

## Step 2: Minimal Working Repository Structure

Add _config.yml, Gemfile, and index.md in the the root folder, so the repository appears like below:

```pqsql
.
├─ _config.yml
├─ Gemfile
├─ index.md
└─ .github/
   └─ workflows/
      └─ jekyll-gh-pages.yml
```

## Step 3: Use Remote Theme

One can compare the original _config.yml in the theme and adapt it.

```yaml
title: My Minimal Mistakes Site
description: A site using Minimal Mistakes with GitHub Actions + remote themes.
url: "https://<your-user>.github.io/<your-repo>"   # Adjust this

theme: null   # REQUIRED because you're using a remote theme

remote_theme: "mmistakes/minimal-mistakes@4.24.0" # REQUIRED to specify the remote theme to use

plugins:
  - jekyll-remote-theme
  - jekyll-include-cache
  - jekyll-feed
  - jekyll-seo-tag

# Required for Minimal Mistakes
markdown: kramdown
kramdown:
  input: GFM

# Minimal Mistakes defaults
theme_variant: default
```

## Step 4: Gemfile

```ruby
source "https://rubygems.org"

gem "jekyll", "~> 4.3"
gem "webrick"
gem "jekyll-remote-theme"
gem "jekyll-include-cache"
gem "jekyll-feed"
gem "jekyll-seo-tag"
```

## Step 5: Create an Example Home Page index.md 

```markdown
---
layout: home
title: "Welcome"
---

This site uses the **Minimal Mistakes** theme through GitHub Actions and a remote theme.
```

## Step 6: Commit and Push the changes to the repository

## Step 7: Enable GitHub Pages

Go to the repository on GitHub:

Settings -> Pages -> Build and deployment -> Source: GitHub Actions
Settings -> Environment -> github-pages

## Step 8: Monitor the Actions on GitHub

Check if the pages is properly deployed uder "https://<your-user>.github.io/<your-repo>"
Once it is deployed, go to Settings -> Pages -> Your site is live at "http://..."


# Notes

- Some themes may use Jekyll Merges Config
  - For example, the _config.yml, you overwrite what you set and you inherit anything you don’t set.
  - Files in directories override, not merge.

# References
