+++ 
draft = true
date = 2026-06-29T18:38:56+08:00
title = "部署 Hugo 网站：使用 GitHub Pages"
description = ""
slug = ""
authors = []
tags = ["Hugo","GithubPages"]
categories = []
externalLink = ""
series = []
+++
使用 Actions 在 GitHub Pages 上托管静态网站

在 `.github/workflows/` 下 
```yaml
# .github/workflows/gh-pages.yml

name: GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  build-deploy:
    runs-on: ubuntu-24.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: gh-pages
          publish_dir: ./public
```
这样，在 push 到 main 后 GitHub 会自动构建网站，把 `public/` 上传到 `gh-pages` 分支，并在该分支部署