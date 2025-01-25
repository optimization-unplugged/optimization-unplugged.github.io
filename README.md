# Setup GitHub actions for Quarto and GitHub Pages

1. On https://github.com/optimization-unplugged/optimization-unplugged.github.io/settings/pages, select "Deploy from a branch", and "gh-pages", "root".
2. Create a file on main branch under /.github/workflows/ called quarto-pages.yml. Contents below.

```yaml
on:
  push:
    branches:
      - main

name: Quarto render and publish

# you need these permissions to publish to GitHub pages
permissions: 
    contents: write
    pages: write

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
        
      - name: Set up Quarto
        uses: quarto-dev/quarto-actions/setup@v2
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          version: "1.5.57"
          # To install LaTeX to build PDF book 
          # tinytex: true 

      # NOTE: If Publishing to GitHub Pages, set the permissions correctly (see top of this yaml)
      - name: Publish to GitHub Pages (and render) 
        uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # this secret is always available for github actions
```
