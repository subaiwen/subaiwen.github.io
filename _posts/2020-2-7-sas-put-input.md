---
title: "SAS: PUT VS. INPUT"
layout: post
date: 2020-02-07 01:22
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- SAS
category: blog
author: subaiwen
description: My notes of using SAS
---

## Overview

When it comes to converting data type or variable type in SAS, it is always challenging to figure out when to use `PUT` and when to use `INPUT`. 

The `PUT` function can be applied to a *character* or *numeric* input, but the output of the Put function is always *character*.

The `INPUT` function can only be applied to a *character* target, but the output can be *character* or *numeric*.

>“In the old, old days” data sources and destinations were usually text.

>`INPUT` statements, and `INFORMAT`s, convert external “text files” into either SAS® character or numeric variables in a SAS data set– converting from “text only” to two types of variables

>`PUT` statements, and `FORMAT`s, are used to convert SAS character and numeric variables “into character” – two types into one.

## Illustration
A. `PUT()` converts character variable to another character variable.  
B. `PUT()` converts numeric variable to a character variable with numeric value.  
C. `PUT()` converts character variable with a user defined format to another character variable.  

D. `INPUT()` converts character variable with numeric value and informat to a numeric variable.  
E. `INPUT()` converts character variable with numeric value and informat to a character variable.  
F. `INPUT()` converts character variable with numeric value and informat to a numeric variable.  

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbnah8zwpzj31280lq42n.jpg" width="600">
</p>



---
## Reference

- [An Animated Guide: Using the Put and INput functions](https://www.lexjansen.com/nesug/nesug08/ff/ff09.pdf)
- [Converting variable types—use PUT() or INPUT()?](https://blogs.sas.com/content/sgf/2015/05/01/converting-variable-types-do-i-use-put-or-input/)

