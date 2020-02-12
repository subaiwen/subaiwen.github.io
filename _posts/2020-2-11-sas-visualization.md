---
title: "SAS: Visualization"
layout: post
date: 2020-02-11 22:18
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- SAS
category: blog
author: subaiwen
description: My notes of using SAS
---

## Overview

- [Single-Cell Plotting](#Single-Cell-Plotting)
- - [One-Variable](#One-Variable)
- - [Two-Variables](#Two-Variables)
- - [Examples](#Examples-Single-Cell-Plotting)

- [Multiple-Cell Plotting](#Multiple-Cell-Plotting)
- - [SGPanel](#SGPANEL)
- - [SGScatter](#SGSCATTER)
- - [Examples](#Examples-Multiple-Cell-Plotting)

- [Reporting](#Reporting)
- - [ODS LAYOUT](#ODS-LAYOUT)

---

## Single-Cell Plotting
**Syntax:**

```SAS
PROC SGPLOT DATA = data-set <options>; 
	PLOTNAME-1 <plot statements> < / OPTIONS>;
	PLOTNAME-2 <plot statements> < / OPTIONS>;
	...
RUN;
```

### One Variable
#### Continuous

| Plot      | Syntax						                    |
|-----------|-----------------------------------------------|
| Histogram | `HISTOGRAM var </ OPTIONS>;`                  |
| Density   | `Density var </ OPTIONS>;`                    |
| Box Plot  | `HBOX var </ OPTIONS>;`<br>`VBOX var </ OPTIONS>;`|

#### Descrete

| Plot      | Syntax						                    |
|-----------|-----------------------------------------------|
| Bar Chart | `HBAR var </ OPTIONS>;`<br>`VBAR var </ OPTIONS>;` |


### Two Variables
#### continuous x, continuous y

| Plot      | Syntax						                    |
|-----------|-----------------------------------------------|
| Scatter Plot | `SCATTER X=x-var Y=y-var </ OPTIONS>;`|
| High-Low  | `HIGHLOW <X=x-var|Y=y-var> HIGH=numeric-var LOW=numeric-var </ OPTIONS>;`|
| Series    | `SERIES X=x-var Y=y-var </ OPTIONS>;`         |
| Line Chart| `VLINE x-var /RESPONSE=y-var </ OPTIONS>;`<br>`HLINE x-var /RESPONSE=y-var </ OPTIONS>;`         |
| HEATMAP	  | `HEATMAP X=x-var Y=y-var </ options>;`        |
| HEATMAPPARM	  | `HEATMAPPARM X=x-var Y=y-var </ options>;`        |
| Bubble	  | `BUBBLE X=x-var Y=y-var </ options>; `        |
| Box Plot  | `HBOX var </ OPTIONS>;`<br>`VBOX var </ OPTIONS>;`|
| Needle    | `NEEDLE X=x-var Y=y-var </ options>; `|


#### discrete x, continuous y

| Plot      | Syntax						                    |
|-----------|-----------------------------------------------|
| Bar Chart | `HBAR x-var / RESPONSE=y-var </ OPTIONS>;`<br>`VBAR x-var / RESPONSE=y-var </ OPTIONS>;` |
| Dot Plot | `Dot x-var / RESPONSE=y-var </ OPTIONS>;`|
| Waterfall| `WATERFALL x-var / RESPONSE=y-var </ OPTIONS>;`|
| Pie Chart| `PROC SGPIE`<br>`PIE x-var / RESPONSE=y-var </ OPTIONS>;`|
| Donut    | `PROC SGPIE`<br>`DONUT x-var / RESPONSE=y-var </ OPTIONS>;`|


#### discrete x , discrete y

| Plot      | Syntax						                    |
|-----------|-----------------------------------------------|
| Bubble	  | `BUBBLE X=x-var Y=y-var </ options>; `        |
| Text		  | `TEXT X=x-var Y=y-var TEXT=y-var </ options>; `|

### 3. Maps


### Examples (Single-Cell Plotting)
**Histogram:** 
 
```SAS
PROC SGPLOT DATA=sashelp.pricedata;
	TITLE "This is the tile";
	WHERE REGIONNAME = 'Region1';
	HISTOGRAM PRICE /BINWIDTH=10 DATALABEL=count FILLATTRS=(COLOR='#CDCDCD');
	DENSITY price/ TRANSPARENCY=0.7 LINEATTRS=(COLOR='#3F3059');
RUN;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbst4kgnpvj30zc0qeacl.jpg" width="530">
</p>

**Bar Chart:**

```SAS
PROC SGPLOT DATA=sashelp.cars(WHERE= (make in ('Acura', 'Volvo') ));
	HBAR type/ RESPONSE=mpg_city GROUP=drivetrain STAT=mean;
RUN;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbsuzoqz7qj30ze0qedho.jpg" width="530">
</p>

**High-Low:**

```SAS
PROC SGPLOT DATA=sashelp.stocks;
	WHERE stock='IBM' AND YEAR(date)=2005;
	HIGHLOW X=date HIGH=high LOW=low / CLOSE=close LINEATTRS=(color='maroon');
run;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbsqa32rsfj30z80q8dhy.jpg" width="530">
</p>

**Bubble:**

```SAS
PROC SGPLOT DATA = sashelp.class;
	BUBBLE X = height Y = weight SIZE =age / GROUP = sex;
RUN;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbst6fsmt7j30zc0qggox.jpg" width="530">
</p>

**Scatter:**

```SAS
%LET color1 = '#3F3059';        
%LET color2 = cxA23A2E;
TITLE "Origin in {Europe, USA}";
PROC SGPLOT DATA=sashelp.cars;
WHERE origin^='Asia' && type^="Hybrid";                 
   STYLEATTRS DATACONTRASTCOLORS = (&color1 &color2);
   SCATTER X=weight Y=mpg_city / GROUP=Origin MARKERATTRS=(SYMBOL=CircleFilled);
   KEYLEGEND / LOCATION=inside POSITION=TopRight ACROSS=1;
RUN;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbsswdmij2j30zc0qcn0i.jpg" width="530">
</p>

**Dot Plot:**

```SAS
PROC SGPLOT DATA=sashelp.cars; 
  DOT TYPE / RESPONSE=horsepower LIMITS=both STAT=mean 
    MARKERATTRS=(SYMBOL=circlefilled SIZE=9); 
    XAXIS grid;
    YAXIS DISPLAY=(nolabel) OFFSETMIN=0.1;
    KEYLEGEND / LOCATION=inside POSITION=topright ACROSS=1;
RUN;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbsxlgmt1dj30ze0qggnz.jpg" width="530">
</p>

**Series:**

```SAS
PROC SGPLOT DATA=sashelp.stocks;
	TITLE "Stock Prices 1986-2005"; 
	SERIES X=date Y=close / GROUP=stock GROUPLP=stock;
RUN;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbstuogi7ej30ze0qg11r.jpg" width="530">
</p>

**Line Chart:**

```SAS
PROC SGPLOT DATA=sashelp.class;
	TITLE "Height by Age and Sex";
	VLINE AGE / RESPONSE=height STAT=mean MARKERS GROUP=sex;
RUN;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbsvgd0yjwj30zg0qijuh.jpg" width="530">
</p>

**Regression:**

```SAS
TITLE "Regression plot";
PROC SGPLOT DATA=sashelp.cars;
	REG X=horsepower Y=enginesize / GROUP=cylinders;
	WHERE cylinders IN (4 6 8);
RUN;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbsu65fecoj30zc0qin2z.jpg" width="530">
</p>

**Box Plot:**

```SAS
PROC SGPLOT DATA=sashelp.cars; 
	TITLE "Price by Car Type"; 
	HBOX msrp / GROUP=type MEANATTRS=(SYMBOL=star) CATEGORY=type;
RUN;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbsulnp8e1j30zg0qi778.jpg" width="530">
</p>

**Heat Map:**

```SAS
PROC SGPLOT DATA=sashelp.heart; 
	HEATMAP X=weight Y=cholesterol / NXBINS=80 NYBINS=80;
RUN;
```

<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbsv9lqqa1j30ze0qeag4.jpg" width="530">
</p>

---

## Multiple-Cell Plotting
### SGPANEL
The SGPANEL procedure creates a panel of graph cells for the values of one or more classification variables. For example, if a data set contains three variables (A, B and C) and you want to compare the scatter plots of B*C for each value of A, then you can use the SGPANEL procedure to create this panel. The SGPANEL procedure creates a layout for you automatically and splits the panel into multiple graphs if necessary.

**Syntax:**  
 
```SAS
PROC SGPANEL DATA=data <options>;
	PANELBY var;
	<options>;
	<graph> ...;
RUN;
```

### SGSCATTER
The SGSCATTER procedure creates a paneled graph of scatter plots for multiple combinations of variables, depending on the plot statement that you use.

**Syntax:**

```SAS
PROC SGSCATTER DATA=data <options>;
	<statement>
RUN;
```

### Examples (Multiple-Cell Plotting)

**SGPanel:**

```SAS
TITLE1 "PRODUCT SALES";
PROC SGPANEL DATA=SASHELP.PRDSALE;
  PANELBY quarter;
  ROWAXIS LABEL="sales";
  VBAR product / RESPONSE=predict STAT=mean
                 TRANSPARENCY=0.3;
  VBAR product / RESPONSE=actual STAT=mean
                 BARWIDTH=0.5 TRANSPARENCY=0.3;
RUN; 
TITLE1;
```
<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbtpq9wh7fj30u00u0n1o.jpg" width="530">
</p>

```SAS
title1 "Distribution of Cholesterol Levels";
PROC SGPANEL DATA=sashelp.heart;
  PANELBY weight_status sex / LAYOUT=lattice
                              NOVARNAME;
  HBOX cholesterol;
RUN; 
title1;
```
<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbtqi9eydwj30z80qggoc.jpg" width="530">
</p>

**SGScatter:**

```SAS
PROC SGSCATTER DATA=sashelp.cars;
  COMPARE Y=mpg_highway
          X=(weight enginesize horsepower)
          / GROUP=type;
RUN;
```
<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbtr35a4ytj30ze0qin2n.jpg" width="530">
</p>

```SAS
PROC SGSCATTER DATA=sashelp.iris
               (WHERE=(species EQ "Virginica"));
MATRIX PETALLENGTH PETALWIDTH SEPALLENGTH
       / ELLIPSE=(TYPE=mean)
         DIAGONAL=(HISTOGRAM KERNEL);
RUN;
```
<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbtr6dxs82j30u20u0jxe.jpg" width="530">
</p>

---

## Reporting
### ODS LAYOUT
<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbtovkble9j30ey0h2gmr.jpg" width="280">
</p>

**ODS LAYOUT ABSOLUTE:**  
Absolute layout is perfectly suited for static types of output that can be printed on a single page where you want output placed in a specific location.

```SAS
ODS LAYOUT ABSOLUTE <options>;
   ODS REGION;
      ...;
      
   ODS REGION;
      ...;
ODS LAYOUT END;
```

**ODS LAYOUT GRIDDED:**  
Gridded layout for dynamically sized regions can accommodate dynamic data, can span more than one page, and allows for easier alignment. Programs created using gridded layout are easier to maintain than those created using absolute layout.

```SAS
ODS LAYOUT GRIDDED ROWS=n-row COLUMNS=n-col <options>;
   ODS REGION ROW=row-index COLUMN=col-index;
      ...;
      
   ODS REGION ROW=row-index COLUMN=col-index;
      ...;
ODS LAYOUT END;
```


### Examples
**ODS LAYOUT GRIDDED:**

```SAS
OPTIONS NONUMBER NODATE;
ODS PDF FILE='test.pdf';
TITLE 'THIS IS TITLE1';
FOOTNOTE 'THIS IS FOOTNOTE1';
ODS LAYOUT GRIDDED;

ODS REGION ROW=1 COLUMN=1;
TITLE 'THIS IS THE REGION TITLE';
FOOTNOTE 'THIS IS THE REGION FOOTNOTE';
PROC PRINT DATA=sashelp.class(OBS=10);
RUN;

ODS REGION ROW=2 COLUMN=1 WIDTH=3in;
PROC SGPLOT DATA=sashelp.stocks;
    TITLE "Stock Prices 1986-2005"; 
    SERIES X=date Y=close / GROUP=stock GROUPLP=stock;
RUN; 

ODS LAYOUT END;
ODS PDF CLOSE; 
```
<p align="center">
  <img src="https://tva1.sinaimg.cn/large/0082zybpgy1gbtokgsyfxj30u010e0y7.jpg" width="580">
</p>

---

## Appendix
- [Color Names Defined by SAS/GRAPH](https://support.sas.com/content/dam/SAS/support/en/books/pro-template-made-easy-a-guide-for-sas-users/62007_Appendix.pdf)
- [Marker Attributes and Symbols](https://documentation.sas.com/?docsetId=grstatproc&docsetTarget=p0i3rles1y5mvsn1hrq3i2271rmi.htm&docsetVersion=9.4&locale=en)
- [SGPLOT Procedures Tip Sheet](https://support.sas.com/rnd/app/ODSGraphics/TipSheet_SGPLOT.pdf)

## Reference
### Single-Cell
- [SAS Programming for R Users](https://support.sas.com/edu/schedules.html?id=3033)
- [Github::SAS Programming for R Users (Course Materials)](https://github.com/sassoftware/sas-prog-for-r-users)
- [SAS-doc::SGPLOT Procedure](https://documentation.sas.com/?docsetId=grstatproc&docsetTarget=p073bl97jzadkmn15lhq58yiy2uh.htm&docsetVersion=9.4&locale=en)
- [Gallery of Plots](https://documentation.sas.com/?docsetId=grstatproc&docsetTarget=n19gxtzyuf79t3n16g5v26b73ckv.htm&docsetVersion=9.4&locale=en#p0rbhrtx1zharwn1a1l805qx77c7)
- [Getting Started with the SGPLOT Procedure](https://www.sas.com/content/dam/SAS/support/en/sas-global-forum-proceedings/2019/3167-2019.pdf)
- [Using PROC SGPLOT for Quick High Quality Graphs](https://www.lexjansen.com/wuss/2008/how/how05.pdf)
- [Fifty Ways to Change your Colors (in ODS Graphics)](https://www.pharmasug.org/proceedings/2016/DG/PharmaSUG-2016-DG04.pdf)
- [What colors does PROC SGPLOT use for markers?](https://blogs.sas.com/content/iml/2017/02/06/group-colors-sgplot.html)
- [Bar Charts with SGPLOT](http://people.uncw.edu/blumj/stt592/ppt/Bar%20Charts%20with%20SGPLOT.pdf)
- [SAS-blogs::SGPLOT](https://blogs.sas.com/content/?s=sgplot)

### Multiple-Cell
- [ODS Layout Tip Sheet](http://support.sas.com/rnd/base/ods/Tipsheet_ods_layout.pdf)
- [ODS LAYOUT Overview](https://documentation.sas.com/?docsetId=odsug&docsetTarget=p0z7mlae4n6vf7n1ldq07tigzuqh.htm&docsetVersion=9.4&locale=en)
- [SGPANEL Procedure](https://documentation.sas.com/?docsetId=grstatproc&docsetVersion=9.4&docsetTarget=p17wwoehcyc6mxn1hpcgxy8ixw6w.htm&locale=en)
- [SGSCATTER Procedure](https://documentation.sas.com/?docsetId=grstatproc&docsetVersion=9.4&docsetTarget=p0lfzklhx36ylln1t9sssgzuf64m.htm&locale=en)
