---
title: managing-jekyll-githubpages
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: blog, resource
tags:
  - jekyll
  - github-pages
  - web-development
  - static-site
  - tutorial
status: active
---


## 1. **Setting Up Jekyll and GitHub Pages**

   1. **Dependencies**: Ensure Ruby and Bundler are installed.
   2. **Install Jekyll**: Run:
```bash
gem install jekyll bundler
```
   1. **Install Dependencies**: From your blog's root directory, use:
```bash
bundle install
```

## 2. **Basic File Structure Overview**

   - **`_config.yml`**: Core config file for site settings like title, theme, author info, etc.
   - **`_posts`**: Folder for blog posts. Each post must be named in `YYYY-MM-DD-title.md` format.
   - **`_tabs`**: Custom pages (e.g., About, Archives, Tags) displayed as tabs on your blog.
   - **`assets/`**: Holds images and other static files, such as avatar images and libraries.
   - **`_plugins/`**: Custom plugins like `posts-lastmod-hook.rb` to add functionalities like last-modified dates.

## 3. **Adding Content**

   - **Create a New Post**: In `_posts`, create a Markdown file:
 ```markdown
 ---
 layout: post
 title: "Your Post Title"
 date: 2024-10-26
 categories: [category1, category2]
 tags: [tag1, tag2]
 ---
 Content of the post...
     ```

   - **Tabs**: Customize About, Archives, Categories, and Tags pages in `_tabs` with Markdown.

## 4. **Customize Configuration**

   - **Update `_config.yml`**:
     - **Site Title**: `title: "Your Site Title"`
     - **URL**: `url: "https://philipbhaworth.github.io"`
     - **Base URL**: `baseurl: ""` (since it’s a GitHub Pages site)
   - **Social and Contact Data**: Edit `_data/contact.yml` and `_data/share.yml` for social links or sharing buttons.

## 5. **Running the Site Locally**

   Run Jekyll locally to preview changes:
```bash
bundle exec jekyll serve
```
   Visit `http://localhost:4000` to view.

## 6. **Deploying to GitHub Pages**

   - **Push Changes**: Commit your changes and push to GitHub.
   - GitHub Pages will automatically build and deploy your site.