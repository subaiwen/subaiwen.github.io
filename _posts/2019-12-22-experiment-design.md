---
title: "实验设计"
layout: post
date: 2019-12-22 12:32
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- Note
- 中文
category: blog
author: subaiwen
description: An intro to Experimental Design
---

> "Block what you can, and randomize what you cannot"

---
# 介绍
- 实验：对客体做一些事，观察客体的反应

- 实验设计：通过控制无关变量 (Block)，观察处理对客体造成的反应

- 原则：  
	- 控制 (Control)：控制无关变量，纯粹地比较处理组间的差异  
	- 随机 (Randomize)：随机分配处理给实验对象  
	- 重复 (Replicate)：在充足的样本里重复性投放每种处理  

---
# 流程
## 一、设计实验
#### 1. 实验目的
#### 实验目的：我想研究什么  
例子：
哪种教学方式更适合继续教育的学生，实体课还是网课?  

#### 提出假设  
> 反证法：你想证明你的某个结论 ($H_A$)，首先拒绝你不承认的结论 ($H_0$)。

假设结构：  
$H_0$：你想推翻的结论  
$H_A$：你想证实的结论  

例子:  
$H_0$：接受网课学生的平均成绩 = 接受实体课学生的平均成绩  
$H_A$：接受网课学生的平均成绩 $\ge$ 接受实体课学生的平均成绩  

#### 2. 元素
- 实验单位：接受实验的个体（例子: 学生）  
- 处理 (Treatment)：对实验单位的处理（例子：授课类型）  
- 影响 (Response/Effect)：处理对实验单位的影响（例子: 学生的接受程度）  
> 测量影响：怎么量化影响的大小（例子: 学生成绩变化）

- 无关变量：我们不关心但会对实验单位造成影响的变量（例子: 学生年龄）
> 变量：能影响实验单位的变量，即——处理 + 骚扰变量

#### 3. 样本
#### 总体：  
所有实验研究的对象。（例子：普遍的学生）  
#### 抽样：  
在不能对总体所有对象进行实验的情况下，需要从总体抽取一定人数的样本，并通过对样本进行实验，以得出同样适用于总体的实验结论。
> 决定样本大小的因素：  
> 	$\alpha$: 显著性水平 (Significance level)  
> 	$\beta$: 统计功效 (Power)  

### 4. 框架设计（Design Structure）
#### Block: 
* 每种Block就是一种无关变量
* 每种Block有多少个水平？  

#### 处理（Treatment）:  
* 处理有多少个水平？每个水平的值是多少？
* 实验组内怎么把各个水平的处理分配到每个实验单位上？  

**例子**  
![](https://tva1.sinaimg.cn/large/006tNbRwgy1gajhdh8gyqj31c00ec415.jpg)

## 二、数据收集
测量处理对实验单位的影响 (effect)。
* 如何测量影响的大小？
* 测量值与真实值是否有偏差？

## 三、数据分析
### 假设检验
用统计的方法检验你的假设是否成立/不成立。
> 统计检验前要检查数据是否符合检验的前提：
> 方差是否相等？
> 误差是否正态分布？

---
## Reference
[Concepts of Experimental Design](http://support.sas.com/resources/papers/sixsigma1.pdf)

