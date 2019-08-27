---
title: "Statistical Modeling: Variable Transformation"
layout: post
date: 2019-08-26 20:32
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- R
- Model
category: blog
author: subaiwen
description: Variable transformation
---

## Overview:
A few concerns about variable transformation in statistical modeling precess. 

---

## Independent Variable:
- There is no assumption about normality on independent variable.  
- Sometimes they may be transformed, not for the sake of normality, but for making the relationship with the Y variable more clear, which may help improve the predictive ability of the the model.

#### 1. Standardizing
- When independent variables are very different scales, centering and scaling can help reduce multicolinearity problems.

#### 2. Skewness
- It is useful, however, to understand the distribution of predictor variables to find influential outliers or concentrated values.

## Dependent Variable



---

### Reference
[http://fmwww.bc.edu/repec/bocode/t/transint.html](http://fmwww.bc.edu/repec/bocode/t/transint.html)  
[Answer: The Analysis Factor](https://www.theanalysisfactor.com/the-distribution-of-independent-variables-in-regression-models/)  
[Answer: Research Gate](https://www.researchgate.net/post/Should_I_transform_non-normal_independent_variables_in_logistic_regression)
