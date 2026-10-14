---
title: "How I Built This Site"
date: 2026-10-14
summary: "A walkthrough of how I set up this portfolio site using Hugo and the Celadon theme, deployed with GitHub Pages."
---

If you like this portfolio site want to build a similar one yourself, here's what I did.

I wanted a portfolio site that was fast, clean, and easy to maintain with the docs-as-code tools I already use: 
1. Markdown markup language for writing content
2. Microsoft VS Code editor for editing files
3. Git for version control
4. GitHub for hosting my repository
5. Hugo static site generator (SSG) for building HTML
6. GitHub Pages for hosting

I chose the [Hugo Celadon](https://themes.gohugo.io/themes/hugo-celadon/) theme for its minimal, professional look.

Here's a high-level walkthrough of the process.

## 1. Set up the Celadon sample site locally

I installed Hugo by following the instructions at [gohugo.io/installation](https://gohugo.io/installation/). On Windows, it was as simple as running `winget install Hugo.Hugo.Extended` in PowerShell (Git should be installed first).

Then I followed the instructions at [Celadon's GitHub repo](https://github.com/Yajie-Xu/hugo-celadon) to create the initial folder structure:

```bash
hugo new site my-portfolio
cd my-portfolio
git init
git submodule add https://github.com/Yajie-Xu/hugo-celadon.git themes/celadon
cp themes/celadon/exampleSite/hugo.toml hugo.toml
```

One thing their README doesn't mention is that when you copy the `hugo.toml` file from the `themes/exampleSite` folder, you need to edit the first two lines to `themesDir = "themes"` and `baseURL = '/'` for it to work outside the example site folder.

From there, running `hugo server` launches a local preview at `http://localhost:1313/` with live reloading, so any changes you save show up instantly.

## 2. Edit Content and Push to GitHub

I edited the site to my liking and added my portfolio pieces. I used Claude AI help with this step, which made it fast and easy.

After editing the site content, I created a `.gitignore` file to leave out some build files from being added to the repo, then added and commited all my files to the repo:

```bash
echo public/ > .gitignore
echo .hugo_build.lock >> .gitignore
git add .
git commit -m "Initial site setup with Celadon theme"
```

On [github.com/new](https://github.com/new), I created a new repository without checking any of the initialization boxes (no README, no `.gitignore`, no license), then followed the instructions for pushing an existing repo.

## 3. Deploy with GitHub Pages

To publish the site, I used GitHub Actions instead of the older "Deploy from a branch" approach. This involved:

1. Adding a `.github/workflows/hugo.yml` file with the [Hugo deployment workflow](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
2. Going to the repo's **Settings > Pages** and selecting **GitHub Actions** as the source
3. Pushing the workflow file and watching the build in the **Actions** tab

Again, I used Claude AI to help with step 1, which made it fast and easy.

## 4. Get a cleaner URL

By default, GitHub Pages serves your site at `https://<username>.github.io/<repo-name>/`. To get the shorter `https://<username>.github.io/` URL, I renamed the repository to `<username>.github.io` under **Settings > General**. After re-running the workflow, the site was live at its final URL.
