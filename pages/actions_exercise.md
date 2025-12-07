---
# permalink: /404.html
layout: single
classes: wide
title: "GitHub Actions Exercise"
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-12-05
---

# A Web game deployment


## Step 0: Clone or fork the cardgame repository

- [ustp-cardgame-test](https://github.com/yun-vis/ustp-cardgame-test)
  

- React: A JavaScript library for building user interfaces.
- TypeScript: JavaScript with static typing.
- Vite: A fast build tool and dev server for modern web apps.
- Tailwind CSS: A utility-first CSS framework.
- GitHub Spark UI Framework (@github/spark): GitHub’s design system / component library.

To build the project locally, one need to install [node.js](https://nodejs.org/en/download/). At the time of testing Node.js (v24.11.1) and npm (11.6.2)

Error when executing npm -v: https://stackoverflow.com/questions/57673913/vsc-powershell-after-npm-updating-packages-ps1-cannot-be-loaded-because-runnin

Run the project locally

```bash
$ npm install # to add dependency
$ npm run build --if-present # build the project locally
```


## Step 1: Add a GitHub action

- Tasks:
  - Add a Github action that builds the application

in .github/workflows/build.yml
```yaml
name: Node.js CI

on:
  push:
    branches: [ "master" ]
  pull_request:
    branches: [ "master" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout a repo
      uses: actions/checkout@v4

    - name: Set up Node.js
      uses: actions/setup-node@v4
      with:
        node-version: 24
        cache: 'npm'

    - name: Install dependencies
      run: npm install

    - name: Build project
      run: npm run build --if-present
```

## Step 2: Mark the new GitHub action as required in Pull Requests

- Tasks:
  - The new GH action that just got added should be marked as required when submitting PRs. It needs to pass before you can merge the PR.

- Create the ruleset called "need passing ci"
  - Add "Target branches" -> add target -> include Default branch
  - enable "Require status checks to pass"
    - click on "Add checks" -> search for "build" and add it 
    - Click on "Any source" and choose "GitHub Actions"
  
## Step 2.5: Disallow direct pushes without a Pull Request

- Create the ruleset called "no push without pull request (pr)"
  - Add "Target branches" -> add target -> include Default branch
  - enable "Require a pull request before merging"

## Step 3: Add unit tests and update the GH action to run the tests

- Tasks:
  - Add unit tests (ask AI)
  - Update the existing GH action to run the tests and upload the test results
  - Make sure that PRs are only merge-able when the tests pass

in .github/workflows/build.yml
```yaml
name: Node.js CI

on:
  push:
    branches: [ "master" ]
  pull_request:
    branches: [ "master" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout a repo
      uses: actions/checkout@v4

    - name: Set up Node.js
      uses: actions/setup-node@v4
      with:
        node-version: 24
        cache: 'npm'

    - name: Install dependencies
      run: npm install

    - name: Run Tests # need to run test before build
      run: npm test # same as npm run test

    - name: Build project
      run: npm run build --if-present
```

in src/components/Component.test.tsx
```ts
// src/setupTests.ts
import { render, screen } from "@testing-library/react";
import MemoryCard from "./MemoryCard";

test("renders greeting", () => {
  render(<MemoryCard />);
  expect(screen.getByText("Hello")).toBeInTheDocument();
});
```

in src/setupTests.ts
```ts
// src/setupTests.ts
import '@testing-library/jest-dom'
```

in vitest.config.ts
```ts
// vitest.config.ts
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    environment: "jsdom",
    setupFiles: "src/setupTests.ts",

    outputFile: "test-results/results.xml",
    reporters: ["default", "junit"],
  },
});
```

in vite.config.ts, add
```json
export default defineConfig({
  test:{
    globals: true,
    environment: "jsdom",
    setupFiles: "./src/setupTests.ts",
  },
});
```

in package.json, add 
```json
{
    "scripts": {
        "test": "vitest run"
    },
    "devDependencies": {
        "@testing-library/jest-dom": "^6.9.1",
        "@testing-library/react": "^16.3.0",
        "jsdom": "^27.2.0",
        "vitest": "^4.0.14"
    },
}
```

Until this moment, running the project remotely is a pain, especially for updating dependency.

By dedefault, one will see the following after a succesful run
JUNIT report written to /home/runner/work/ustp-cardgame-t/ustp-cardgame-t/test-results/results.xml

- Upload the test results

in .github/workflows/build.yml, add
```yaml
    - name: Upload Test Results
      if: always() # Run this even if tests fail, so we can see WHAT failed
      uses: actions/upload-artifact@v4
      with:
        name: test-results
        path: test-results/results.xml
```

## Step 4: Add a deployment GitHub action

- Tasks:
  - Add a deployment GH action that deploys the app to a GitHub page
  - The action shouldn't run automatically, only when manually run via the "Actions" tab in the repository
  - Make sure that GitHub pages are enabled and authorized for the new deployment GH action
  - Post the link to the new GitHub page as a comment when completed.

- Go to the repository on GitHub:

Settings -> Pages -> Build and deployment -> Source: GitHub Actions
Settings -> Environment -> github-pages

in .github/workflows/deploy.yml, add
```yaml
# Sample workflow for building and deploying a Jekyll site to GitHub Pages
name: Deploy GitHub Pages

on:
  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build: # Build job

    runs-on: ubuntu-latest

    steps:
    - name: Checkout a repo
      uses: actions/checkout@v4

    - name: Set up Node.js
      uses: actions/setup-node@v4
      with:
        node-version: 24
        cache: 'npm'

    - name: Install dependencies
      run: npm install

    - name: Run Tests # need to run test before build
      run: npm test # same as npm run test

    - name: Upload Test Results
      if: always() # Run this even if tests fail, so we can see WHAT failed
      uses: actions/upload-artifact@v4
      with:
        name: test-results
        path: test-results/results.xml

    - name: Build project
      run: npm run build 

    - name: Setup Pages
      uses: actions/configure-pages@v5

    - name: Upload artifact
      uses: actions/upload-pages-artifact@v3
      with:
        path: './dist' # Vite builds to the 'dist' folder
        
  # Deployment job
  deploy:      
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    runs-on: ubuntu-latest

    needs: build

    steps:

    - name: Deploy to GitHub Pages
      id: deployment
      uses: actions/deploy-pages@v4
```


in vitest.config.ts
```ts
// vitest.config.ts
import { defineConfig } from "vitest/config";

export default defineConfig({
  base: "./",
});
```

## Step 5: Create a release branch

- Tasks:
  - Create a release branch with the following name: "release/1.0.0" from the latest code in main.

in .github/workflows/release.yml
```yaml
name: Create Specific Release Branch

on:
  workflow_dispatch:
    inputs:
      branch_name:
        description: 'The full name of the release branch to create (e.g., release/1.0.0)'
        required: true
        default: 'release/1.0.0'

permissions: 
  contents: write # This grants the necessary permission to create/push branches

jobs:
  create_release_branch:

    name: Create Release

    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          ref: 'master'
          # This line ensures the runner has write access to create a branch
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Set up Git
        run: |
          # Configure Git to use the GitHub Actions bot identity
          git config user.name github-actions[bot]
          git config user.email 41898282+github-actions[bot]@users.noreply.github.com

      - name: Create and Push New Release Branch
        id: create_branch
        run: |
          BRANCH_NAME="${{ github.event.inputs.branch_name }}"
          echo "Attempting to create branch: $BRANCH_NAME"          

          # Create the new branch from the current HEAD (which is the latest 'main')
          git branch $BRANCH_NAME
          
          # Push the new branch to GitHub
          git push origin $BRANCH_NAME
          
          echo "Branch '$BRANCH_NAME' successfully created from main."         
```

## Step 6: Add a release note for the release/1.0.0 branch

- Tasks:
  - Add a release note in the repository
  - Automatically create a tag
  - Generate release notes
  - Generate a discussion so that others can comment on the new release

in .github/workflows/auto_release.yml
```yaml
name: Automatic GitHub Release

on:
  push:
    # Trigger the workflow only when a commit is pushed to a release branch
    branches:
      - 'release/**' 

jobs:
  create_github_release:

    name: Auto Create Release

    runs-on: ubuntu-latest
    
    # Grant necessary permissions for the GITHUB_TOKEN to create the release/tag
    permissions:
      contents: write # Required to create the release, tag, and publish notes
      discussions: write # Required to optionally create a discussion

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Extract Version Tag
        id: get_version
        run: |
          # The branch name will be 'release/1.0.0'. We extract the '1.0.0' part.
          BRANCH_NAME="${{ github.ref_name }}"
          # Removes 'release/' prefix
          TAG_NAME=${BRANCH_NAME#release/}
          
          # Set the output variable 'tag' for use in the next step
          echo "tag=$TAG_NAME" >> $GITHUB_OUTPUT

      - name: Create Release & Tag
        uses: softprops/action-gh-release@v2 # A popular, feature-rich action for releases
        with:
          # 1. Automatically create a tag
          tag_name: v${{ steps.get_version.outputs.tag }} # Use the extracted version, prepended with 'v'
          name: Release v${{ steps.get_version.outputs.tag }}
          
          # 2. Generate release notes
          # Compares the current commit to the previous tag/release to generate a changelog
          generate_release_notes: true 
          
          # 3. Add a release note in the repository (This is the Release description itself)
          body: |
            ## Release Notes for v${{ steps.get_version.outputs.tag }}
            
            This is an automated release created from the ${{ github.ref_name }} branch.
            
            **Highlights:**
            * [Generated Notes Here]
            
            ---
            
            **Full Changelog:**
            ${{ steps.release.outputs.body }} # Placeholder for generated notes
          
          # 4. Generate a discussion
          # Creates a new discussion for the release in the 'Announcements' category
          discussion_category_name: Announcements 
          
          # Optional: Make it a draft first if you want to review it
          # draft: true 
          
          # Optional: Set this if this is a pre-release version
          # prerelease: false
```

- Since we create a new workflow, we can merge this change to builds/buildFeature to trigger the auto_release workflow


# Questions

- Can we have a sample solution?
- Why do we need ceate two seperate rulesets? What is the difference?
- Why I can not use npm ci?
- How does .github dependabot work here?
- How to update package.json if I don't run the project locally? At least I didn't manage.
- How to see the uploaded test results?

# References

- [Dependabot options reference](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference)