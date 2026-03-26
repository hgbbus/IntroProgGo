# Book Writing Notes

This file records the steps and notes for writing this book. In the process, I consulted Gemini heavyly for advice and suggestions. I may use other AI tools as well.

## Preparation

### Creating a Directory and Notes File

First, I created a directoy for the book project:

```bash
% mkdir IntroProgGo
% cd IntroProgGo
```

Then, I created a file to record the steps and notes for writing the book:

```bash
% touch bookwritingnotes.md
```

and started recording the steps and notes in this file.

### Installing Quarto

After asking AI for recommendations on tools for writing the book, I decided to use Quarto, which is a powerful tool for creating documents, presentations, and websites.

I downloaded the latest version of Quarto from the official website: https://quarto.org/docs/get-started/ and got a file named `quarto-1.9.36-macos.pkg`.

I installed Quarto by double-clicking the downloaded file and following the installation instructions.

Then, in the terminal, I verified the installation by running:

```bash
% quarto check
Quarto 1.9.36
[✓] Checking environment information...
      Quarto cache location: ~/Library/Caches/quarto
[✓] Checking versions of quarto binary dependencies...
      Pandoc version 3.8.3: OK
      Dart Sass version 1.87.0: OK
      Deno version 2.4.5: OK
      Typst version 0.14.2: OK
[✓] Checking versions of quarto dependencies......OK
[✓] Checking Quarto installation......OK
      Version: 1.9.36
      Path: /Applications/quarto/bin

[✓] Checking tools....................OK
      TinyTeX: (not installed)
      Chromium: (not installed)
      Chrome Headless Shell: (not installed)
      VeraPDF: (not installed)

[✓] Checking LaTeX....................OK
      Using: Installation From Path
      Path: /Library/TeX/texbin
      Version: 2023

[✓] Checking Chrome Headless....................OK
      Using: Chrome found on system
      Path: /Applications/Google Chrome.app/Contents/MacOS/Google Chrome
      Source: MacOS known location

[✓] Checking basic markdown render....OK

[✓] Checking R installation...........(None)

      Unable to locate an installed version of R.
      Install R from https://cloud.r-project.org/

[✓] Checking Python 3 installation....OK
      Version: 3.13.11
      Path: /Library/Frameworks/Python.framework/Versions/3.13/bin/python3
      Jupyter: (None)

      Jupyter is not available in this Python installation.
      Install with python3 -m pip install jupyter

[✓] Checking Julia installation...
```

Then, I installed the Quarto extension for Visual Studio Code. I then restarted Visual Studio Code to ensure the extension is properly loaded.

### Directory Structure

Per Gemini, I created the following directory structure (I certainly will modify it as needed later):

```
IntroProgGo/
├── _quarto.yml             # ⚙️ The Master Configuration (defines parts/chapters)
├── index.qmd               # 📖 The Welcome/Landing Page (Preface)
├── references.qmd          # 📚 Generated Bibliography/References
├── part1-basics/
│   ├── index.qmd           # 🧩 Intro to Part 1
│   ├── 01-variables.qmd    # 📝 Chapter 1
│   └── 02-functions.qmd    # 📝 Chapter 2
└── part2-concurrency/
    ├── index.qmd           # 🧩 Intro to Part 2
    └── 03-goroutines.qmd   # 📝 Chapter 3
```

The `.qmd` files are Markdown files with Quarto extensions. It is 100% compatible with standard Markdown (.md), but allows you to do advanced Quarto things later!

### Creating the Master Configuration File

The `_quarto.yml` file is the master configuration file for the book. It defines the structure of the book, including the parts and chapters.

Here is the content of the `_quarto.yml` file I created:

```yaml
project:
  type: book
  output-dir: _book

book:
  title: "Introduction to Programming Using Go"
  author: "Gongbing Hong"
  date: "2026-03-25"
  
  # This sets up your sidebar navigation with Parts and Chapters
  chapters:
    - index.qmd
    - part: "The Go Workspace"
      chapters:
        - part1-basics/index.qmd
        - part1-basics/01-variables.qmd
        - part1-basics/02-functions.qmd
    - part: "Concurrency"
      chapters:
        - part2-concurrency/index.qmd
        - part2-concurrency/03-goroutines.qmd
    - references.qmd

format:
  html:
    theme: cosmo
    toc: true               # Table of contents for each page
    code-copy: true         # Handy "Copy" button on Go code snippets
  pdf:
    documentclass: scrreprt # Excellent for academic printing
```

Then, I ran the following command to preview the book:

```bash
% quarto preview
```

This command starts a local server and opens the book in my web browser. I can see the structure of the book with the parts and chapters I defined in the `_quarto.yml` file.

There was a little issue with the numbering of the chapters, where the first `index.qmd` was numbered as 1 and the second `index.qmd` numbered as 2, etc.

The first `index.qmd` is the preface of the book, and it should not be numbered as a chapter. Let's put the following content in the `index.qmd` file to make it a proper preface:

```markdown
# Preface {.unnumbered}

Welcome to my introductory Go textbook for college students. In this book, you will learn...

Best things to put here:

    Welcome Message: Why are we learning Go? What makes Go special (compiled, statically typed, garbage collected)?

    Target Audience: Prerequisite knowledge (e.g., "Assumes you know basic Python/Java loops").

    Syllabus / Learning Outcomes: What will students be able to build by Week 15?
```

Notice the `{.unnumbered}` after the heading. This tells Quarto not to number this section as a chapter.

The other `index.qmd` files in the `part1-basics` and `part2-concurrency` directories are the introductions to the respective parts. They shouldn't be numbered as chapters either. They were similarly fixed by adding `{.unnumbered}` to their headings.

Not every book writer would choose to have an introduction for each part. I guess I will do the same by deleting the `index.qmd` files in the parts. Accordingly, remove the references to these files in the `_quarto.yml` file as well.

Similarly, I also fixed the `references.qmd` file by adding `{.unnumbered}` to its heading, since it should not be numbered as a chapter either.

### Setting Up Version Control with Git, GitHub, and GitHub Pages

First, I created the `.gitignore` file with the following content:

```
# Quarto output and freeze directories
_book/
_site/
.quarto/

# Go binaries and workspace files (if you write Go tests locally)
*.exe
*.exe~
*.dll
*.so
*.dylib
*.test
__debug_bin

# OS-specific files
.DS_Store
Thumbs.db
```

Then, I initialized a Git repository in the `IntroProgGo` directory and made the first commit:

```bash
# Initialize a new Git repository
% git init

# Set local git user email to a GitHub no-reply email
% git config user.email "hgbbus@users.noreply.github.com"

# Stage all files (respecting your new .gitignore!)
% git add .

# Create your first commit
% git commit -m "Initialize Quarto textbook with proper structure"

# Rename your branch to 'main' (modern standard)
# % git branch -M main        # Not needed; already named 'main' by default
```

Next, I created a new repository on GitHub named `IntroProgGo` and kept it "Public". I didn't add any of those suggested files.

Then, I connected my local Git repository to the GitHub repository and pushed the initial commit:

```bash
# Add the GitHub repository as a remote named 'origin'
% git remote add origin https://github.com/hgbbus/IntroProgGo.git

# Push my commits to GitHub (set upstream to 'main')
# % git branch -M main
% git push -u origin main

Now, I will add a local `README.md` file with the following content:

```markdown
# Introduction to Programming Using Go

This is the source code repository for the book "Introduction to Programming Using Go" by Gongbing Hong. The book is designed for college students learning programming for the first time. The language of instruction is Go, which is a powerful and efficient programming language developed by Google.
```

Then, I will commit and push the updates to GitHub:

```bash
% git add .
% git commit -m "Add README with book description"
% git push
```

Finally, I will set up GitHub Pages to host the book online. The classic way is to go to the "Settings" tab of the GitHub repository, then to the "Pages" section. Then select the branch `main` and the folder `_book` as the source for GitHub Pages, and save the settings.

But I will follow Gemini's advice to set up GitHub Actions to automatically build and deploy the book whenever I push changes to the `main` branch. This way, I don't have to manually trigger the deployment every time I update the book.

First, create a special folder named `.github/workflows` in the root of the repository:

```bash
% mkdir -p .github/workflows
```

Then, add a file named `publish.yml` in the `.github/workflows` folder with the following content:

```yaml
name: Deploy Quarto Book to GitHub Pages

on:
  push:
    branches:
      - main      # Triggers every time you push to the 'main' branch

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read  # Allows reading the repository contents (required for deployment)
  pages: write    # Allows writing to GitHub Pages (required for deployment)
  id-token: write # Allows generating an OpenID Connect token for authentication (required for deployment)

concurrency:
  group: "pages"              # Ensures only one deployment runs at a time
  cancel-in-progress: false   # Allows multiple deployments to run concurrently (optional, set to true if you want to cancel previous deployments when a new one starts)

jobs:
  deploy:                     # This job handles the deployment process to GitHub Pages
    environment:
      name: "github-pages"
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Set up Quarto
        uses: quarto-dev/quarto-actions/setup@v2

      - name: Render Quarto Project
        uses: quarto-dev/quarto-actions/render@v2
        with:
          to: html     # Compiles the website format

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: _book/ # Tells GitHub to upload the rendered HTML folder

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Before pushing this workflow file, configure GitHub Repository Settings to allow GitHub Actions to deploy to GitHub Pages:
1. Go to the "Settings" tab of your GitHub repository.
2. In the left sidebar, click on "Pages".
3. Under "Build and deployment", look for the dropdown menu labeled "Source".
4. Change it to "GitHub Actions" instead of "Deploy from a branch".

Then, commit and push the workflow file to GitHub:

```bash
% git add .
% git commit -m "Add GitHub Actions workflow for automatic deployment to GitHub Pages"
% git push
```

The GitHub Actions workflow runs with some warnings:

```
Warning: Node.js 20 actions are deprecated. The following actions are running on Node.js 20 and may not work as expected: actions/checkout@v4, actions/configure-pages@v4, actions/deploy-pages@v4, actions/upload-artifact@v4. Actions will be forced to run with Node.js 24 by default starting June 2nd, 2026. Node.js 20 will be removed from the runner on September 16th, 2026. Please check if updated versions of these actions are available that support Node.js 24. To opt into Node.js 24 now, set the FORCE_JAVASCRIPT_ACTIONS_TO_NODE24=true environment variable on the runner or in your workflow file. Once Node.js 24 becomes the default, you can temporarily opt out by setting ACTIONS_ALLOW_USE_UNSECURE_NODE_VERSION=true. For more information see: https://github.blog/changelog/2025-09-19-deprecation-of-node-20-on-github-actions-runners/
```

Let's update the versions of the actions to the latest ones that support Node.js 24. The latest versions are:
- actions/checkout@v6
- actions/configure-pages@v5
- actions/deploy-pages@v4 (still v4, but it supports Node.js 24)
- actions/upload-artifact@v4

After updating the workflow file with the new versions, commit and push the changes again:

```bash
% git add .
% git commit -m "Update GitHub Actions workflow to use latest action versions that support Node.js 24"
% git push
```

Still some warnings. Forget about them for now.

Go to the "Settings" tab of the GitHub repository, then to the "Pages" section. You should see a message that says "Your site is ready to be published at https://hgbbus.github.io/IntroProgGo/". Click on the link to view your book online!

Commit and push any further changes to the `main` branch, and the GitHub Actions workflow will automatically build and deploy the updated book to GitHub Pages.

```bash
% git add .
% git commit -m "Minor update"
% git push
```

### Finalizing the Book Structure

Update the `_quarto.yml` file to reflect the final structure of the book:

```yaml
...

  # This sets up your sidebar navigation with Parts and Chapters
  chapters:
    - index.qmd
    - part: "Getting Started"
      chapters:
        #- part1-getting-started/index.qmd       
        - part1-getting-started/ch01-the-basics.qmd
        - part1-getting-started/ch02-data-types-computation.qmd
    - part: "Control Structures"
      chapters:
        #- part2-control-structures/index.qmd
        - part2-control-structures/ch03-conditionals.qmd
        - part2-control-structures/ch04-loops.qmd
...
```

Rename the corresponding chapter files and directories to match the new structure.

Commit and push the changes to GitHub:

```bash
% git add .
% git commit -m "Update book structure and chapter file names"
% git push
```

## Writing the Book

Work on the Preface. This is a work in progress, and I will keep updating it as I go along. The current content is just a placeholder.

No plan to write any thing for the introduction of the part for now. So, nothing for the `index.qmd` files in the parts.

### Part I: Getting Started

#### Ch01 The Basics

Work in progress. The current content is just a placeholder.

#### Ch02 Data, Types, and Computation

Work in progress. The current content is just a placeholder.
