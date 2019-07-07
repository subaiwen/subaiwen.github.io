---
title: "Powershell usage for python learning"
layout: post
date: 2019-07-7 19:05
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- Powershell
- Python
category: blog
author: subaiwen
description: Powershell
---

## Overview:
Powershell commands I used when learning [Python](https://learnpythonthehardway.org/python3/).

---

## Commands:
👶
```bash
ni $(1..52 | %{"ex$_\ex$_.py"}) -f
```
52 exercises in total. Create 52 empty **.py** files in 52 folds for each exercise. \

>`ni = New-Item`    
>`-f = -Force`

👶
```bash
ii ex1\ex1.py
```
Open **ex1.py** in defualt software (sublime).
>`ii = Invoke-Item`

👶
```bash
python -m pydoc sys
```
Get Python help file for **sys**.
>`-m`: module

👶
```bash
echo "A text file with some text" > ex1\ex1.txt
type ex1\ex1.txt
```
Create a text file with some text in it.
>`echo`: Sends the specified objects to the next command in the pipeline.  
>`type`/`more`： print out the file