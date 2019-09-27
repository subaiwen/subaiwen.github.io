---
title: "Jekyll installation"
layout: post
date: 2019-07-3 23:21
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- Jekyll
- Ruby
category: blog
author: subaiwen
description: Install Jekyll
---

## Overview:

Following [GitHub Pages + Jekyll 创建个人博客](https://www.jianshu.com/p/9535334ffd54) to install **Jekll** on Windows, I found the guide was out-of-date in the Chinese official Jekyll website where its content has not been updated for 3 years (2016). So I turned to its original [official site](https://jekyllrb.com/) for the [installation guide](https://jekyllrb.com/docs/installation/).

---

## Windows:

### 1. Download and Install Ruby+Devkit
Download and Install a Ruby+Devkit version from [RubyInstaller Downloads](https://rubyinstaller.org/downloads/). Use default options for installation.
It will pop up a installation terminal window, type `1` and `ENTER`. After the installaion process, `ENTER` again, the windown will disappear.

After this step, you can check if **Ruby** and **RubyGems** are properly installed in the command prompt window:
```
ruby -v
gem -v
```

### 2. Install **Jekyll**

```
gem install jekyll bundler
```

Check  if Jekyll installed properly: 

```
jekyll -v
```

> Jekyll official site has a list of requirements:  
> * Ruby ✔️
> * RubyGems ✔️
> * GCC and Make (`gcc -v`, `g++ -v` and `make -v`) ❌
> * 
> But it is good to go anyways.

---
## Mac
If you run:

```
gem install jekyll bundler
```
You will get an error:

```
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.3.0 directory.
```
The reason can be found in [Stackoverflow](https://stackoverflow.com/questions/14607193/installing-gem-or-updating-rubygems-fails-with-permissions-error).

### 1. rbenv
[Github: rbenv](https://github.com/rbenv/rbenv)
#### (1) Install rbenv.
```bash
brew install rbenv
```
Note that this also installs ruby-build, so you'll be ready to install other Ruby versions out of the box.

#### (2) Set up rbenv in your shell.
```bash
rbenv init
```
Follow the printed instructions to set up rbenv shell integration.

#### (3) Close your Terminal and open a new one so your changes take effect.

#### (4) Verify that rbenv is properly set up using this rbenv-doctor script:
```bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
```
That's it! Installing rbenv includes ruby-build, so now you're ready to install some other Ruby versions using rbenv install.

#### (5) Upgrading with Homebrew
To upgrade to the latest rbenv and update ruby-build with newly released Ruby versions, upgrade the Homebrew packages:

```bash
brew upgrade rbenv ruby-build
```

### 2. Jekyll
[Official Instruction](https://jekyllrb.com/docs/installation/macos/):

#### (1) Install ruby
```bash
rbenv install 2.6.3
rbenv global 2.6.3
ruby -v
```

#### (2) Install Jekyll: Local Install
```bash
gem install --user-install bundler jekyll
ruby -v
```
After getting the version of ruby, append your path file with the following, replacing the `X.X` with the first two digits of your Ruby version.

```bash
export PATH=$HOME/.gem/ruby/X.X.0/bin:$PATH
```
check that `GEM PATHS:` points to a path in your home directory:

```bash
gem env
```

> [Requirements](https://jekyllrb.com/docs/installation/):

> - **Ruby** version 2.4.0 or above, including all development headers (ruby version can be checked by running `ruby -v`)
> - **RubyGems** (which you can check by running `gem -v`)
> - **GCC** and **Make** (in case your system doesn’t have them installed, which you can check by running `gcc -v`,`g++ -v` and `make -v` in your system’s command line interface)

### 3. Trouble shooting
- [rbenv not changing ruby version](https://stackoverflow.com/questions/10940736/rbenv-not-changing-ruby-version):

```bash
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
```
Write this to the last setting in your `~/.bash_profile`.

- [Bundler::GemNotFound](https://stackoverflow.com/questions/23801899/bundlergemnotfound-could-not-find-rake-10-3-2-in-any-of-the-sources)

```
Could not find gem 'github-pages' in any of the gem sources listed in your Gemfile. (Bundler::GemNotFound)
```
```bash
bundle install
```

- Gem::LoadError when `jekyll s`

```bash
bundle exec jekyll serve
```

---
## Jekyll usage
```bash
source ~/.bashrc
cd $web
bundle exec jekyll serve
```
Go to the directory of your website and build a server.

```
JekyllAdmin mode: production
Server address: http://127.0.0.1:4000
Server running... press ctrl-c to stop.
```
According to the message, you can test your web in your sever with `http://127.0.0.1:4000`, and `ctrl-c` to stop.

```bash
open http://127.0.0.1:4000
```
`ctrl-z` suspend and `fg` to come back

---


## Next steps:
* Read [Command Line Usage](https://jekyllrb.com/docs/usage/) and [Tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/), [Cheatsheet](https://devhints.io/jekyll), [Rmarkdown support](https://bookdown.org/yihui/blogdown/jekyll.html)

