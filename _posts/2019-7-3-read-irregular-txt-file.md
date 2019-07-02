---
title: "Read irregular .txt file"
layout: post
date: 2019-07-3 21:46
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- R
- Text mining
- Data cleaning
- rJava
- regex
category: blog
author: subaiwen
description: Read irregular .txt file
---

## Overview:

My friend #8 turned to me for a script that can automatically extract informative data from his irregular <span style="font-family:M+ 1M; font-size:12pt;">.txt</span> ([sample.txt](https://github.com/subaiwen/subaiwen.github.io/tree/master/assets/posts/2019-7-3/no8.txt)) and output it as <span style="font-family:M+ 1M; font-size:12pt;">.xlsx</span> with multiple sheets. As he wish. Let's go!

---

## Tasks:
 * Name the <span style="font-family:M+ 1M; font-size:12pt;">.xlsx</span> with the first row of <span style="font-family:M+ 1M; font-size:12pt;">.txt</span>, which is company name and location
 * Extract all the <span style="font-family:M+ 1M; font-size:12pt;">STEERS</span> relative tables and write them as sheets with its table name in <span style="font-family:M+ 1M; font-size:12pt;">.xlsx</span>
 * Filter data of each table to keep the rows where <span style="font-family:M+ 1M; font-size:12pt;">Wt Range</span> is in the range betweeen 700 and 900.



## Steps: 3 functions

### 1. Get file name

```r
# file name
get.filename <- function(path){
  filename <- readtext::readtext(path)$text %>% 
  	str_extract(".+?\n") %>% 
  	str_remove("[:blank:]+") %>% str_remove("\n")
  	return(filename)
}
```

### 2. Get data
```r
# tables
get.data <- function(path){
  wd <- str_extract(path,"^..+/")
  data.txt <- readtext::readtext(path)$text %>% str_extract_all("STEERS(.|\n)+?\n\n") %>% unlist()
  n <- length(data.txt)
  data.list <- list()
  data.name.list <- c()
  for (i in 1:n){
    data.name.list[i] <- data.txt[i] %>% str_extract('STEERS.+?\n') %>% str_remove('\n') %>%
      str_remove(" \\(.+\\)") %>% str_remove("^STEERS - ")
    file.path <- paste0(wd,'STEER',i,".txt")
    data.txt[i] %>%
      str_remove("\n\n") %>%
      str_remove('STEERS.+?\n') %>%
      writeLines(file.path)
    data.lines <- readLines(file.path)
    file.remove(file.path)
    # parse
    # var
    var <- data.lines[1] %>% str_extract_all('[:alpha:]+[:blank:]?[:alpha:]+') %>% unlist()
    # data
    data <- data.frame(matrix(ncol = length(var), nrow = 0))
    colnames(data) <- var
    rows <- list()
    for (k in 2:length(data.lines)){
      rows[[k]] <- data.lines[k] %>% str_extract_all('([:digit:]|\\.|-|[:alpha:])+') %>% unlist()
      data[k-1,] <- rows[[k]][1:length(var)]
    }
    data.list[[i]] <- data
  }
  names(data.list) <- data.name.list
  return(data.list)
}
```

### 3. Filter data
```r
# extract information in need
filter.data <- function(data.list,lower = 700, upper = 900){
  names <- names(data.list)
  n <- length(data.list)
  data.list.copy <- data.list
  data.list.new <- list()
  for (i in 1:n){
    n.row <- nrow(data.list[[i]])
    for (k in 1:n.row){
      if (str_detect( data.list[[i]][k,]$`Wt Range`, '-')){
        n.1 <- data.list[[i]][k,]$`Wt Range` %>% str_extract('^[:digit:]+') %>% as.numeric()
        n.2 <- data.list[[i]][k,]$`Wt Range` %>% str_extract('[:digit:]+$') %>% as.numeric()
        data.list.copy[[i]][k,]$`Wt Range` <- (n.1+n.2)/2
      }
    }
    data.list.new[[i]] <- data.list[[i]][as.numeric(data.list.copy[[i]]$'Avg Wt') > lower & as.numeric(data.list.copy[[i]]$'Avg Wt') < upper,]
  }
  names(data.list.new) <- names
  return(data.list.new)
}
```

### 4. Output
At the beginning, I tried to use <span style="font-family:M+ 1M; font-size:12pt;">library(xlsx)</span>. It worked several months ago, but this time I got the following warning message:

```
WARNING: Initial Java 12 release has broken JNI support and does NOT work. Use stable Java 11 (or watch for 12u if avaiable).
ERROR: Java exception occurred during rJava bootstrap - see stderr for Java stack trace.
Exception in thread "main" java.lang.NullPointerException
	at java.base/jdk.internal.reflect.Reflection.verifyMemberAccess(Reflection.java:130)
	at java.base/java.lang.reflect.AccessibleObject.slowVerifyAccess(AccessibleObject.java:673)
	at java.base/java.lang.reflect.AccessibleObject.verifyAccess(AccessibleObject.java:666)
	at java.base/java.lang.reflect.AccessibleObject.checkAccess(AccessibleObject.java:638)
	at java.base/java.lang.reflect.Field.checkAccess(Field.java:1075)
	at java.base/java.lang.reflect.Field.get(Field.java:416)
Error: package or namespace load failed for ‘xlsx’:
 .onLoad failed in loadNamespace() for 'xlsx', details:
  call: .jcheck(silent = FALSE)
  error: java.lang.NullPointerException.jcall(f, "Ljava/lang/Object;", "get", .jcast(ic, "java/lang/Object"))<S4 object of class "jobjRef">
In addition: Warning message:
package ‘xlsx’ was built under R version 3.4.4 
```

It seemed something went wrong with my <span style="font-family:M+ 1M; font-size:12pt;">Java 12</span>. So I downloaded <span style="font-family:M+ 1M; font-size:12pt;">Java 11</span> and tried to [change the default <span style="font-family:M+ 1M; font-size:12pt;">Java</span> (JDK) version](https://stackoverflow.com/questions/21964709/how-to-set-or-change-the-default-java-jdk-version-on-os-x). It didn't work. 
It might work if I uninstall <span style="font-family:M+ 1M; font-size:12pt;">Java 12</span> ([instruction](https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html#A1096903))([tool](https://www.java.com/en/download/uninstalltool.jsp)). But I was just too lazy to try. It already took me a while to install it 😭.

> Prior to this, I follow the [instruction](https://zhiyzuo.github.io/installation-rJava/) by [Zhiya Zuo](https://zhiyzuo.github.io/) to install <span style="font-family:M+ 1M; font-size:12pt;">Java 12</span>. Quite honestly, I should have install older version.

So I turned to another package <span style="font-family:M+ 1M; font-size:12pt;">writexl</span>. It can do the same thing, but is not based on <span style="font-family:M+ 1M; font-size:12pt;">rJava</span>. Perfect!

```r
# output
out <- function(path){
  file.name <- get.filename(path)
  data <- get.data(path)
  data.need <- filter.data(data)
  writexl::write_xlsx(data.need,paste0(str_extract(path,"^.+/"),file.name,".xlsx"))
}
```
```r
# example
library(tidyverse)
path <- "~/Downloads/no8.txt"
out(path)
```
📎 Output: [Eastern MO Commission Company - Bowling Green, MO.xlsx](https://github.com/subaiwen/subaiwen.github.io/tree/master/assets/posts/2019-7-3/no8.xlsx/)

---

## More
* Also extract the column with no column name, which has description about the rows.
* Wrap the functions into a package.

But I won't do these unless he treat me a bubble tea 😌