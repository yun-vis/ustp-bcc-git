---
# permalink: /404.html
layout: single
classes: wide
title: "GitHub Actions" 
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-12-05
---

GitHub Actions makes it easy to automate all your software workflows, now with world-class CI/CD. Build, test, and deploy your code right from GitHub. Make code reviews, branch management, and issue triaging work the way you want.

# Basic Concept

## DevOps: 

DevOps is a set of cultural philosophies, practices, and tools that combine software development and IT operations to automate and integrate processes, enabling faster, more reliable software delivery.
 
## CI/CD

CI/CD are software development methods. A CI/CD pipeline is an automated process that combines Continuous Integration (CI) and Continuous Delivery/Deployment (CD) to streamline software development, testing, and deployment. It automates the steps from a developer's code change to its release, enabling faster, more reliable updates and reducing manual errors. 

- Continuous Integration (CI): Developers frequently merge their code changes into a central repository, where automated builds and tests run to detect and fix bugs early.
- Continuous Delivery (CD): After passing CI, the code changes are automatically prepared for release to a production-ready environment, usually requiring a manual approval to deploy.
- Continuous Deployment (CD): An extension of Continuous Delivery, this automatically deploys the code changes to all users once they have passed all automated checks.
- Automated workflow: The pipeline consists of a series of automated jobs and stages that execute tasks like compiling code, running unit and security tests, and creating build artifacts.

# The components of GitHub Actions

- Workflows: A workflow is a configurable automated process that will run one or more jobs. Workflows are defined by a YAML file checked in to your repository and will run when triggered by an event in your repository, or they can be triggered manually, or at a defined schedule.
  
- Events: An event is a specific activity in a repository that triggers a workflow run. For example, an activity can originate from GitHub when someone creates a pull request, opens an issue, or pushes a commit to a repository.
  
- Jobs: A job is a set of steps in a workflow that is executed on the same runner. 
  
- Actions: An action is a pre-defined, reusable set of jobs or code that performs specific tasks within a workflow, reducing the amount of repetitive code you write in your workflow files.
  
- Runners: A runner is a server that runs your workflows when they're triggered. Each runner can run a single job at a time.
  
## Action

- Definition: A single, self-contained, and reusable task that performs a specific operation.
- Purpose: To perform a discrete, often repeated, task within a workflow.
- Structure: A single step within a job. Actions can be written by you, found in a marketplace (like GitHub Marketplace), or exist as a combination of multiple steps bundled together in a "composite action".
- Example: The actions/checkout@v2 action, which checks out your repository, is a common action used at the beginning of many workflows. 

## Workflow

- Definition: A complete automated process, triggered by an event, that orchestrates multiple jobs.
- Purpose: To automate a larger process, such as building, testing, and deploying software.
- Structure: Contains one or more jobs.
- Example: A "Release Process" workflow triggered by a push event that includes jobs for generating a release and deploying it. 

## Actions vs. Workflows

A workflow is a series of one or more jobs that run in response to a trigger, while an action is a single, reusable step that performs a specific task within a workflow. Workflows orchestrate a larger process, such as continuous integration and deployment, by calling upon multiple actions to complete it. Think of a workflow as the full project plan, and actions as the individual tasks in that plan, like "check out the code," "run tests," or "deploy to production". 

# Exercise

## Step 1: Create an empty repository

- Check [Quickstart for GitHub Actions](https://docs.github.com/en/actions/get-started/quickstart)
- Create an empty github repository

```bash
$ git clone git@github.com:myname/cicd-test.git
$ cd cicd-test
```

## Step 2: Create a README.md file and push the changes to the repository

```bash
Hello World!
```

## Step 3: Create a GitHub workflow

- Create a .github/workflows folder under the root folder to store GitHub workflows
  - For GitHub to discover any GitHub Actions workflows in your repository, you must save the workflow files in a directory called .github/workflows/
  - The workflow file must be an YAML file.
- Name your workflow file echoWorkflow.yml for now.

in .github/workflows/echoWorkflow.yml  
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

- Multi-line string literal block style: If the value of a key is a multi-line string, you can use the literal block style using the pipe (|) character or the folded block style using the greater than (>) character. The literal style is the most straightforward. It preserves newlines and indentation exactly as they are. This is particularly helpful when defining shell commands

- Push the changes to the master branch
- You will see at the Actions tag on GitHub, the action is triggered automatically once the changes have been pushed.
- Regarding the syntax, check [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax)

## Step 4: Create a Spellcheck GitHub workflow

- Create a new workflow file .github/workflows/spellcheck.yml
- Create a config file called spellcheck_config.yml under the root folder

in spellcheck.yml
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
          config_path: spellcheck_config.yml
          task_name: Markdown
          output_file: spellcheck-output.txt

      - name: Upload Spellcheck Report
        if: '!cancelled()' # Do not upload artifact if job was cancelled
        uses: actions/upload-artifact@v4
        with:
          name: spellcheck-report
          path: spellcheck-output.txt
```

in spellcheck_config.yml
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

- Push the changes to the master repository

## Step 5: Setup a spellcheck rule on GitHub

- Go to Settings -> Rulesets -> New ruleset -> New branch ruleset
- Create the ruleset called "need passing ci"
  - Add "Target branches" -> add target -> include Default branch
  - enable "Require status checks to pass"
    - click on "Add checks" -> search for "spellcheck" and add it 
    - Click on "Any source" and choose "GitHub Actions"

- Create the second ruleset called "no push without push request (pr)"
  - Add "Target branches" -> add target -> include Default branch
  - enable "Require a pull request before merging"

## Step 6: Test the SpellCheck workflow

- Add a typo (or even without a typo) in the README.md and push the change to the master branch
- The push should be blocked.

Note 1: This is because branch protection rules are blocking direct commits. GitHub’s Branch protection rules often prevent users from pushing directly to main. One must open a pull request instead.

Note 2: We can use git reset to remove the old commits and fix the problem before pushing it again. Check git usage.

```bash
git reset --soft HEAD~2
```

Note 3: When this rule is enabled on a protected branch (e.g., master): GitHub cannot validate status checks for a direct push, because status checks only run on a ref created by a pull request or a push to a non-protected branch. So GitHub enforces the only safe option: It forces all changes to go through a pull request, because that’s the only workflow where GitHub can run and verify status checks before the code lands on master.

- Checkout a new branch called featureBranch
- Add a typo in the README.md and push the change in this new branch. Push to the new branch should be successful since the check will be done when a pull_request happens. Check the file .github/workflows/spellcheck.yml 

- Once the push done, do a pull requst to the master branch. Now the workflow should be starting and does not pass the spellcheck due to the typo.
  
- Fix the typo and push the changes to the featureBranch

- Under the Pull requests, one should see that the new codes pass the spellcheck and is ready for merging back to master.


# References

- [Understanding GitHub Actions](https://docs.github.com/en/actions/get-started/understand-github-actions)
- [Quickstart for GitHub Actions](https://docs.github.com/en/actions/get-started/quickstart)
- [GitHub Actions documentation](https://docs.github.com/en/actions)
- [Workflows and actions](https://docs.github.com/en/actions/concepts/workflows-and-actions)
- [What is CI/CD?](https://www.geeksforgeeks.org/devops/what-is-ci-cd/)
- [DevOps Foundations: Continuous Delivery/Continuous Integration](https://www.linkedin.com/learning/devops-foundations-continuous-delivery-continuous-integration-14449917/)
- [The Official YAML Web Site](https://yaml.org/)
- [What is YAML? A beginner's guide](https://circleci.com/blog/what-is-yaml-a-beginner-s-guide/)
- [About pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)