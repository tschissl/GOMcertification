---
title: "OKUM Certification"
author: "TCM"
date: "Tuesday, July 15, 2014"
output:
  word_document:
    fig_caption: yes
  pdf_document: default
  html_document:
    mathjax: default
    self_contained: yes
---
Automated analysing submitted data for OKUM based on defined outliers
========================================================
Thomas Meisel (2014-07-21)  

### defining the RM and measurand to be analysed

```r
refmat <- 'OKUM' # defining the RM
```

### general comments to the design 

The data for this interlaboratory comparison based certification of property values were analysed by 36 labs following the nested design approached as proposed the IAG certification protocol. Participating labs received 3 packages of OKUM and MUH-1 respectively and one package of GAS. The latter was supplied as a "traceablility" sample and is here used for quality control purposes. It was the task of the labs to prepare two independent sample preparations (i.e. digstions) of each packet and analyse the preparations on two different days. Labs thus should have submitted 12 values (3x2x2 PacketxPrepxDay). 
The outliers have been selected based in Youden plots, Mandel's k and detection limit criteria. In this file the property values and the uncertainties are calculated for all analytes of a specific candidate OKUM



```r
'%p%' <- function(x, y) {as.character(paste (x, y, sep =""))}
df <- data.frame(cbind(0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0))
names(df) <- c("date", "RM", "measurand", "mean.before", "mean.after", "median.before", "median.after", "median.after.noPP", "unit", "sL", "sbb", "sr", "u", "u.alternative", "t.value", "outlier", "labs remaining", "based on", "property value", "U") # needed only the first time
df <- df[!1,]
write.table(df, "df0.txt", row.names=FALSE) # needed only the first time
```






```r
# Data for certification project was gathered and joined in Excel. The files
# were exported from Excel as xxxx.csv files to make them universially
# readable. For this markdown the data is stored in the 'root/documents'
# directory.  Data is loaded ('GOMGather1.R') and merged ('GOMMerge.R') for
# GAS, OKUM and MUH-1 are merged together with a methods file
# ('OKUM.method') into a universal data.frame file named 'GOM'. All of this
# happens in the 'Makefile.R'
```

#### importing the data and assigning factors























