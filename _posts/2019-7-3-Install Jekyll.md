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

## Steps:

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

## Next steps:
* Read [Quickstart](https://jekyllrb.com/docs/) and [Tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/), [Cheatsheet](https://devhints.io/jekyll), [Rmarkdown support](https://bookdown.org/yihui/blogdown/jekyll.html)

---

Hum, I found I could not run the commands in Powersheel. I should go back to the [book](https://learnpythonthehardway.org/book/appendixa.html) and stop playing with my page for a while :sob:. Man, you are unemployed now! Learn and do something useful！
