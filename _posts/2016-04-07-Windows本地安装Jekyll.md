---
layout: post
title:  "Windows本地安装Jekyll"
date:   2016-04-05 09:00:13
categories: [环境搭建]
permalink: /archivers/install-jekyll-locally
---
# 参考文章

https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/



You can set up a local version of your Jekyll GitHub Pages site to test changes to your site locally. We highly recommend installing Jekyll to preview your site and help troubleshoot failed Jekyll builds.

In this article:

- [Requirements](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#requirements)
- [Step 1: Create a local repository for your Jekyll site](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-1-create-a-local-repository-for-your-jekyll-site)
- [Step 2: Install Jekyll using Bundler](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-2-install-jekyll-using-bundler)
- [Step 3 (optional): Generate Jekyll site files](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-3-optional-generate-jekyll-site-files)
- [Step 4: Build your local Jekyll site](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-4-build-your-local-jekyll-site)
- [Keeping your site up to date with the GitHub Pages gem](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#keeping-your-site-up-to-date-with-the-github-pages-gem)
- [Next steps: Configuring Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#next-steps-configuring-jekyll)
- [Further reading](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#further-reading)

Jekyll is not officially supported for Windows. For more information, see "[Jekyll on Windows](http://jekyllrb.com/docs/windows/#installation)" in the official Jekyll documentation.

### Requirements

We recommend using [Bundler](http://bundler.io/) to install and run Jekyll. Bundler manages Ruby gem dependencies, reduces Jekyll build errors, and prevents environment-related bugs. To install Bundler, you must install[Ruby](https://www.ruby-lang.org/).

1. Open Git Bash.

2. Check whether you have Ruby 2.0.0 or higher installed:

   ```
   `ruby --version`
   ruby 2.X.X
   ```

3. If you don't have Ruby installed, [install Ruby 2.0.0 or higher](https://www.ruby-lang.org/en/downloads/).

4. Install Bundler:

   ```
   gem install bundler
   # Installs the Bundler gem
   ```

5. If you already have a local repository for your Jekyll site, skip to [Step 2](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-2-install-jekyll-using-bundler).

### Step 1: Create a local repository for your Jekyll site

1. If you haven't already downloaded Git, install it. For more information, see "[Set up Git](https://help.github.com/articles/set-up-git/)."

2. Open Git Bash.

3. On your local computer, initialize a new Git repository for your Jekyll site:

   ```
   git init my-jekyll-site-project-name
   Initialized empty Git repository in /Users/octocat/my-site/.git/
   # Creates a new file directory on your local computer, initialized as a Git repository

   ```

4. Change directories to the new repository you created:

   ```
   cd my-jekyll-site-project-name
   # Changes the working directory

   ```

5. If your new local repository is for a [Project pages site](https://help.github.com/articles/user-organization-and-project-pages/), create a new `gh-pages` branch:

   ```
   git checkout -b gh-pages
   Switched to a new branch 'gh-pages'
   # Creates a new branch called 'gh-pages', and checks it out

   ```

### Step 2: Install Jekyll using Bundler

To track your site's dependencies, Ruby will use the contents of your Gemfile to build your Jekyll site.

1. Check to see if you have a Gemfile in your local Jekyll site repository:

   ```
   ls
   Gemfile

   ```

   If you have a Gemfile, skip to step 4. If you don't have a Gemfile, skip to step 2.

2. If you don't have a Gemfile, open your favorite text editor, such as [Atom](https://atom.io/), and add these lines to a new file:

   ```
   source 'https://rubygems.org'
   gem 'github-pages', group: :jekyll_plugins

   ```

3. Name the file `Gemfile` and save it to the [root directory](https://en.wikipedia.org/wiki/Root_directory) of your local Jekyll site repository. Skip to step 5 to install Jekyll.

4. If you already have a Gemfile, open your favorite text editor, such as [Atom](https://atom.io/), and add these lines to your Gemfile:

   ```
   source 'https://rubygems.org'
   gem 'github-pages', group: :jekyll_plugins

   ```

5. Install Jekyll and other [dependencies](https://pages.github.com/versions/) from the GitHub Pages gem:

   ```
   bundle install
   Fetching gem metadata from https://rubygems.org/............
   Fetching version metadata from https://rubygems.org/...
   Fetching dependency metadata from https://rubygems.org/..
   Resolving dependencies...

   ```

### Step 3 (optional): Generate Jekyll site files

To build your Jekyll site locally, preview your site changes, and troubleshoot build errors, you must have Jekyll site files on your local computer. You may already have Jekyll site files on your local computer if you cloned a Jekyll site repository. If you don't have a Jekyll site downloaded, you can generate Jekyll site files for a basic Jekyll template site in your local repository.

If you want to use an existing Jekyll site repository on GitHub as the starting template for your Jekyll site, fork and clone the Jekyll site repository on GitHub to your local computer. For more information, see "[Fork a repo](https://help.github.com/articles/fork-a-repo/)."

1. If you don't already have a Jekyll site on your local computer, create a Jekyll template site:

   ```
   bundle exec jekyll new . --force
   New jekyll site installed in /Users/octocat/my-site.

   ```

2. To edit the Jekyll template site, open your new Jekyll site files in a text editor. Make your changes and save them in the text editor. You can preview these changes locally without committing your changes using Git.

   If you want to publish your changes on your site, then you must commit your changes and push them to GitHub using Git. For more information on this workflow, see "[Good Resources for Learning Git and GitHub](https://help.github.com/articles/good-resources-for-learning-git-and-github/)" or see this [Git cheat sheet](https://help.github.com/articles/git-cheatsheet).

### Step 4: Build your local Jekyll site

1. Navigate into the [root directory](https://en.wikipedia.org/wiki/Root_directory) of your local Jekyll site repository.

2. Run your Jekyll site locally:

   ```
   bundle exec jekyll serve
   Configuration file: /Users/octocat/my-site/_config.yml
              Source: /Users/octocat/my-site
         Destination: /Users/octocat/my-site/_site
   Incremental build: disabled. Enable with --incremental
        Generating...
                      done in 0.309 seconds.
   Auto-regeneration: enabled for '/Users/octocat/my-site'
   Configuration file: /Users/octocat/my-site/_config.yml
      Server address: http://127.0.0.1:4000/
    Server running... press ctrl-c to stop.

   ```

3. Preview your local Jekyll site in your web browser at `http://localhost:4000`.

### Keeping your site up to date with the GitHub Pages gem

Jekyll is an [active open source project](https://github.com/jekyll/jekyll) and is updated frequently. As the GitHub Pages server is updated, the software on your computer may become out of date, resulting in your site appearing different locally from how it looks when published on GitHub.

1. Open Git Bash.
2. Run this update command:
   - If you followed our setup recommendations and installed [Bundler](http://bundler.io/), run `bundle update github-pages` or simply `bundle update` and all your gems will update to the latest versions.
   - If you don't have Bundler installed, run `gem update github-pages`

### Next steps: Configuring Jekyll

To configure your pages site further, see "[Configuring Jekyll](https://help.github.com/articles/configuring-jekyll)." To set up a project pages site, see[Jekyll's official documentation on project pages URLs](http://jekyllrb.com/docs/github-pages/#project-page-url-structure).

# 下载ruby、DevKit

http://rubyinstaller.org/downloads/

# 报错

## gem install bundler报错

报错如下：
```python
gem install bundler
ERROR:  While executing gem ... (Gem::RemoteFetcher::FetchError)
Errno::ECONNRESET: An existing connection was forcibly closed by the remote host. - SSL_connect (https://api.rubygems.org/quick/Marshal.4.8/bundler-1.11.2.gemspec.rz)
```

解决方案：

```python
gem sources --add http://gems.ruby-china.org/ --remove https://rubygems.org/
http://gems.ruby-china.org/ added to sources
https://rubygems.org/ removed from sources
```

方案说明：

[RubyGems](http://rubygems.org/) 一直以来在国内都非常难访问到，在本地你或许可以翻墙，当你要发布上线的时候，你就很难搞了！

因此国内有网站提供完整 [RubyGems](https://rubygems.org/) 镜像，你可以用此代替官方版本：

https://ruby.taobao.org

https://gems.ruby-china.org

前者已经停更，建议使用后者。后者基于国内 CDN + 国外服务器的方式，能确保几乎无延迟的同步。（推荐使用http而不是https）



## bundle install报错

报错如下：

```python
An error occurred while installing RedCloth (4.2.9), and Bundler cannot
continue.
Make sure that gem install RedCloth -v '4.2.9' succeeds before bundling.
```

解决方案：

 从http://rubyinstaller.org/downloads/下载DevKit，在C:\whatever\whatever\RubyDevelopKit下

1、ruby dk.rb init

2、配置 config.yml

```python
# Example:
#
# ---
# - C:/ruby19trunk
# - C:/ruby192dev
#
---
- C:\Ruby23-x64
- C:\Ruby23-x64
```

3、ruby dk.rb review

4、ruby dk.rb install

5、bundle install