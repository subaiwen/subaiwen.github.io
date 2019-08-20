---
title: "My personal page :octocat:"
layout: post
date: 2019-07-1 1:56
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- Webpage
- Markdown
category: blog
author: subaiwen
description: The way to build up personal webpage
---

## Overview:

I ran into a very clean and nice [personal webpage](https://zhiyzuo.github.io/installation-rJava/) when I was solving the rJava problem in R. I Browsed his github page and found the original [template](https://github.com/sergiokopplin/indigo). So I decided to build up my own webpage based on this template.

---

## Steps:

### 1. Fork

I tried to fork the repository and rename it to a format **username.github.io** to initialize my webpage. But after renaming, the webpage did not work. In the setting, I saw the warning message: 
{% highlight html %}
Github Pages is currently disabled. User and Organization Pages are only published from the master or gh-pages branch.
{% endhighlight %}
After struggling for a while, I gave it up and decided to make it another way. I created a blank github page repository with **username.github.io** and it successfully published. Then upload the files that I downloaded from the [indigo repo](https://github.com/sergiokopplin/indigo) to my gh-page repo. It worked!

### 2. Modify

Everything, including **_config.yml**, **assets** fold and **about.md**, was smoothly edited. After some baby-step set-ups, [My personal page](https://subaiwen.github.io) is ready to go. And here I am writing my first blog!🍷🍷 

### 3. Manage

I downloaded [github desktop](https://desktop.github.com/) to locally manage the files. After cloning the repo to the github desktop, you can change and edit the files in your computer, and then easily sync the repo by 1 or 2 clicks in the desktop app.


### 4. 📋 Next steps

Still, there are a lot of things to improve✔️:

* The comment area (disqus) is in Korean, I want it to be English. (I disable the comment feature for now) ❌ [Solution from underdogliu](https://github.com/sergiokopplin/indigo/issues/388)
* I want to keep 'subject' to show my experience and coursework. But most of the work were recorded in **Rmarkdown**, **Html** or **Tex**. So I will need a tool ([Pandoc](https://pandoc.org/index.htm)) to convert the files to **Markdown** (If **Markdown** is the only file format to use). ❌
* I want to add a section about 'food'. I could change the name of 'project'. (I disable 'subject' for now) ❌
* I also want a rating area above the comment area.❌
![](http://ww2.sinaimg.cn/large/006tNc79ly1g4ld205l9zj30za07w3zg.jpg)
* Enable page search.❌

To achieve these features, I might need to modify the template. Some extra skill to equip:
* [jekyll](https://jekyllrb.com/), [中文](https://www.jekyll.com.cn/)
* [git](https://www.liaoxuefeng.com/wiki/896043488029600)

---
### Thanks:
🔗[Sérgio A. Kopplin](https://koppl.in/), his [template](https://koppl.in/indigo/)

🔗[Qingbaoying](https://github.com/qiubaiying), his tutorial for mac users [博客搭建详细教程](https://github.com/qiubaiying/qiubaiying.github.io/wiki/博客搭建详细教程)

🔗[梦幻之云](https://agcaiyun.github.io/), her tutorial for windows user [GitHub Pages + Jekyll 创建个人博客](https://www.jianshu.com/p/9535334ffd54)
