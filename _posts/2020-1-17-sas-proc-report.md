---
title: "SAS: Report to Excel"
layout: post
date: 2020-01-17 22:13
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- SAS
category: blog
author: subaiwen
description: My notes of using SAS
---

## Overview
- [ODS: TAGSETS.EXCELXP VS. EXCEL](#ODS) 
- [PROC REPORT](#proc-report)


---
## ODS
ODS (Output Delivery System): delivers output in a variety of formats, including HTML, Rich Text Format (RTF), PostScript (PS), Portable Document Format (PDF), and SAS data sets.
> Traditional SAS output: designed for line–printer

Basic ODS syntax:

```SAS
ODS TRACE ON </<options>>; 
ODS destination <FILE=filename>; 
ODS OUTPUT output-object-name=SAS-data-set-name; 
ODS <destination>SELECT output-object-name|ALL|NONE; 
... PROC ...
ODS <destination> CLOSE;  ODS TRACE OFF; 
```

- **Destinations**: ODS can be used to route quality presentation files to various destinations, including LISTING (default), HTML, RTF, PRINTER, and PDF.
- **Object**: Output objects are created by ODS to store the formatted results of most SAS procedures.
- **Style**: Styles define the presentation attributes of a report, such as font and color.

Very often, we report SAS tables to Excel. In this case we can utilize **ODS TAGSETS.EXCELXP** and **ODS EXCEL**.

### 1. ODS Excel

Starting in SAS 9.4 maintenance release 3, [**ODS Excel**](https://documentation.sas.com/?docsetId=odsug&docsetTarget=p09n5pw9ol0897n1qe04zeur27rv.htm&docsetVersion=9.4&locale=en) destination creates `.xlsx` files that can be used with Microsoft Office 2010 or later. **ODS Excel** Formats With **SAS format**, such as `comma7.`.

**Syntax:**

```SAS
ods _all_ close;
ods excel file="/myshare/sample.xlsx" style= <style name> options(<option1 option2>); /*Opens Excel*/
...
ods excel close; /*Closes Excel */
```

#### Style
You can view the list of styles that are available at your site by submitting the statements below:

```
proc template;
     list styles;
run; quit;
```

### 2. ODS TAGSETS.EXCELXP 
**ExcelXP tagset** creates a Microsoft XML spreadsheet file, which can be used with Excel 2002 and later. **ExcelXP tagset** does not create a native Excel format (.xlsx). **ExcelXP tagset** Formats With [TAGTTR](https://docs.microsoft.com/en-us/office/troubleshoot/excel/format-cells-settings), i.e. `TAGATTR = <attribute>`.

**Syntax:**

```SAS
ODS TAGSETS.EXCELXP PATH="/path/" file="file.xls" style=<style> options(<option1> <option2>)
options(<option3>);
```

To see references to [Suboptions](https://support.sas.com/rnd/base/ods/odsmarkup/excelxp_help.html), submit the following code:  

```SAS
ODS tagsets.excelxp file="test.xml" options(doc="help");
```

**Example: TAGGATR Formats preserve comma’s & trailing zeroes**

```SAS
ods tagsets.excelxp path="/path/" file="excelxp.xls" style=printer options(header_data_associations="yes" autofit_height='yes')
options(embedded_titles="yes" embedded_footnotes='yes' absolute_column_width="37,9,9");
proc report data=section1 nowd spanrows split='|' style(header)=[fontweight=bold fontsize=10pt font=(Times, 8pt) ] ;
column name ("Special Census" number pc);
define name / "Subject" f=$table1f. font=(TimesNewRoman, 10pt)];
define number / 'Number' style(column)=[tagattr="format:#,##0" font=(TimesNewRoman, 10pt)] ; 
define pc / 'Percent' style(column)=[tagattr="format:##0.0" font=(TimesNewRoman, 10pt)] ;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gblwcsg05ij31n80iqdiy.jpg" width="600">
</p>

---

## Proc Report
To control the report in an Excel Workbook, we can utilize **ODS** combined with **PROC REPORT**. The SAS paper [**Unleash the Power of PROC REPORT
with the ODS EXCEL Destination**](https://www.sas.com/content/dam/SAS/support/en/sas-global-forum-proceedings/2018/2479-2018.pdf) has very detailed examples. Here I list some common used demos.

**Demo: Multiple Worksheets**

```SAS
ods tagsets.excelxp path="/path/" file="test.xls" style=<style> options(<option>)
ods tagsets.excelxp options(sheet_name = 'Sheet 1');
	proc report;
	...
	run;
	
ods tagsets.excelxp options(sheet_name = 'Sheet 2');
	proc report;
	...
	run;
ods tagsets.excelxp close;
```

**Example: Cell Merge**  
SPANROWS option works on column that uses GROUP or ORDER.

```SAS
ods excel file="&file" options(sheet_name="test");
ods escapechar='~';
ods text="~S={font_size=14pt font_weight=bold vjust=m}~SAS Community";
ods text="~S={font_size=14pt font_weight=bold vjust=m}";

proc report data=sashelp.class nowd spanrows ;
     column ('~S={foreground=purple}Measures' weight height)  
            ('~S={foreground=green}Information' name age sex);
     define sex / order style(column)={vjust=c just=c};
run;

ods excel close;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gbm1vrmt4nj30go0ggmyx.jpg" width="600">
</p>

[**Demo: Transpose**](http://support.sas.com/resources/papers/proceedings14/SAS388-2014.pdf)

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gbm2ywygvqj30n00bu0v2.jpg" width="600">
</p>

```SAS
proc report data=&data nowd;
     column league team,captain;
     define league / group;
     define team / across;
     define captain / display;
run;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/006tNbRwgy1gbm2zhu31uj30kw0a8gnr.jpg" width="400">
</p>

---
## Reference
- [Introduction to the Output Delivery System (ODS)](https://support.sas.com/content/dam/SAS/support/en/books/quick-results-with-the-output-delivery-system/58458_excerpt.pdf)
- [ODS TAGSETS.EXCELXP and ODS EXCEL SHOWDOWN](https://support.sas.com/resources/papers/proceedings17/0973-2017.pdf)
- [Using the New ODS EXCEL Destination in SAS® 9.4](https://support.sas.com/resources/papers/proceedings17/0169-2017.pdf)
- [Unleash the Power of PROC REPORT
with the ODS EXCEL Destination](https://www.sas.com/content/dam/SAS/support/en/sas-global-forum-proceedings/2018/2479-2018.pdf)
- [PROC REPORT STYLE IN HEADER](https://communities.sas.com/t5/ODS-and-Base-Reporting/PROC-REPORT-STYLE-IN-HEADER/td-p/91381)
