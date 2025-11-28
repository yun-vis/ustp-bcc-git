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

# Git

- [Git Cheat Sheet (git-scm)](https://git-scm.com/cheat-sheet)
- [GIT CHEAT SHEET (GitHub)](https://education.github.com/git-cheat-sheet-education.pdf)

# GitHub Action

- [Quickstart for GitHub Actions](https://docs.github.com/en/actions/get-started/quickstart)
- [Learn X in Y minutes](https://learnxinyminutes.com/yaml/)
- [yaml.org](https://yaml.org/)
- [What is YAML?](https://www.yaml.info/learn/index.html)
- []
  
GH echo Action
```yaml
name: Echo Current Date

on:
  push:        # Runs on every push
    branches:
      - '**'   # All branches

jobs:
  echo-date:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Print current date
        run: |
          echo "Current date and time:"
          date 
```

First GitHub Action
```yaml
name: Spell Check Markdown Files

on:
  pull_request:
    branches:
      - '**'
    paths:
      - '**/*.md'

jobs:
  spellcheck:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Run Markdown Spellcheck
        uses: rojopolis/spellcheck-github-actions@v0
        with:
          config_path: spellcheck.yml
          task_name: Markdown
          output_file: spellcheck-output.txt

      - name: Upload Spellcheck Report
        if: '!cancelled()' # Do not upload artifact if job was cancelled
        uses: actions/upload-artifact@v4
        with:
          name: spellcheck-report
          path: spellcheck-output.txt
```

```yaml


matrix:
  - name: Markdown
    aspell:
      lang: en
      ignore-case: true
    encoding: utf-8
    pipeline:
      - pyspelling.filters.markdown:
          markdown_extensions:
            - pymdownx.superfences
      - pyspelling.filters.html:
          comments: false
          ignores:
            - code
            - pre
    sources:
      - '**/*.md'         # All Markdown files
    default_encoding: utf-8
```