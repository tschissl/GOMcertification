Analysing submitted data for OKUM, GAS or MUH-1
========================================================
Thomas Meisel (2014-06-26)  
The data for this interlaboratory comparison based certification of property values were analysed by 36 labs following the nested design approached as proposed the IAG certification protocol. Participating labs received 3 packages of OKUM and MUH-1 respectively and one package of GAS. The latter was supplied as a "traceablility" sample and is here used for quality control purposes. It was the task of the labs to prepare two independent sample preparations (i.e. digstions) of each packet and analyse the preparations on two different days. Labs thus should have submitted 12 values (3x2x2 PacketxPrepxDay). 

#### importing the data and assigning factors

```r
source("Makefile.R")
```

### defining the RM and measurand to be analysed

```r
refmat <- 'GAS' # defining the RM
# measurand.name <- 'Zr' # defining the measurand
```





```r
### function plot_method defined here. Automated plot of methods boxplots sorted by increasing method median. Enter with plot_method=('xx') #######
plot_method <- function(x) {
  element <- x
  '%p%' <- function(x, y) {as.character(paste (x, y, sep =""))}
  prep <-  'Prep.'
  method <- 'Method.'
  anal.prep <- prep %p% measurand.name
  anal.method <- method %p% measurand.name
  anal <- GOM[[element]]
  anal.prep <- GOM[[anal.prep]]
  anal.method <- GOM[[anal.method]]
  analyte <- data.frame(GOM$Lab, GOM$names, anal, anal.prep, anal.method )
  analyte <- na.omit(analyte)
  p <- ggplot(reorder_by(anal.method, ~anal, analyte, median), aes(anal.method, anal))+ geom_boxplot(outlier.shape = NA) + geom_jitter(aes(colour=anal.prep), size = 4.5, position = position_jitter(width = .2))
  p + xlab("method") + ylab(unit) + labs(title = measurand.name) + labs(colour = "Prep") + mytheme
}
```

```r
'%p%' <- function(a, b) {as.character(paste (a, b, sep =""))} # the measureands have different endings depending on which RM: OKUM not extention, MUH extention ".1" and GAS has the extention ".2"
```



```r
# col <- OKUM.outlier[,c(1,3,5,7,9,11,13,15,17,19,21,23,25,27,29,31,33,35,37,39,41,43,45,47,49,51,53,55,57,59,61,63,65,67,69,71,73,75,77,79,81,83,85,87,79,91,93,95,97,99,101,103,105,107,109)]
col <- OKUM.outlier[,c(1,3,5,7)]
col.names <- colnames(col)
for (m in col.names) {
  measurand.name <- m
  switch(
    refmat,
    GAS = rm1 <- 2,
    MUH = rm1 <- 1,
    OKUM = rm1 <- 0
  )
  if(rm1 > 0) 
  {measurand <- measurand.name %p% '.' %p% rm1
  } else 
  {
    measurand <- measurand.name
  }
  MorT <- grep(measurand.name, colnames(GOM), fixed=TRUE) # finding the position of the measurand.name in the Columnheaders of dataframe
  ifelse(MorT[1]< 21, MorT <- 'M', MorT<-'T') # testing if measurand is a major or trace element/compound (col:5-20 majors)
  ifelse(MorT == "T", unit <- 'mg/kg', unit <- 'g/100g') # testing which unit is needed  
  print(measurand)
  plot_method <- function(x) {
  element <- x
  '%p%' <- function(x, y) {as.character(paste (x, y, sep =""))}
  prep <-  'Prep.'
  method <- 'Method.'
  anal.prep <- prep %p% measurand.name
  anal.method <- method %p% measurand.name
  anal <- GOM[[element]]
  anal.prep <- GOM[[anal.prep]]
  anal.method <- GOM[[anal.method]]
  analyte <- data.frame(GOM$Lab, GOM$names, anal, anal.prep, anal.method )
  analyte <- na.omit(analyte)
  p <- ggplot(reorder_by(anal.method, ~anal, analyte, median), aes(anal.method, anal))+ geom_boxplot(outlier.shape = NA) + geom_jitter(aes(colour=anal.prep), size = 4.5, position = position_jitter(width = .2))
  p + xlab("method") + ylab(unit) + labs(title = measurand.name) + labs(colour = "Prep") + mytheme
}
  # plot_method(measurand)
  print(ggplot(col, aes(x=col[[measurand.name]], y=col[[measurand.name]])) + geom_point())
        }
```

```
## [1] "SiO2.2"
```

```
## Warning: Removed 6 rows containing missing values (geom_point).
```

![plot of chunk plot methods](figure/plot methods1.png) 

```
## [1] "TiO2.2"
```

```
## Warning: Removed 4 rows containing missing values (geom_point).
```

![plot of chunk plot methods](figure/plot methods2.png) 

```
## [1] "Al2O3.2"
```

```
## Warning: Removed 6 rows containing missing values (geom_point).
```

![plot of chunk plot methods](figure/plot methods3.png) 

```
## [1] "Fe2O3T.2"
```

```
## Warning: Removed 4 rows containing missing values (geom_point).
```

![plot of chunk plot methods](figure/plot methods4.png) 

```r
plot_method(measurand)
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
## Warning: Removed 1 rows containing missing values (geom_point).
## Warning: Removed 8 rows containing missing values (geom_point).
```

![plot of chunk plot methods](figure/plot methods5.png) 


```r
nm <- names(col)
for (i in seq_along(nm)) {
  print(ggplot(col,aes_string(x = nm[i], y=nm[i]))+ geom_point())
        }
```


```r
col <- OKUM.outlier[,c(1,3,5,7)]
col.names <- colnames(col)
for (m in col.names) {
  measurand.name <- m
   print(measurand.name)
   print(ggplot(col, aes(x=col[[measurand.name]], y=col[[measurand.name]])) + geom_point())
        }
```

```
## [1] "SiO2"
```

```
## Warning: Removed 6 rows containing missing values (geom_point).
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-51.png) 

```
## [1] "TiO2"
```

```
## Warning: Removed 4 rows containing missing values (geom_point).
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-52.png) 

```
## [1] "Al2O3"
```

```
## Warning: Removed 6 rows containing missing values (geom_point).
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-53.png) 

```
## [1] "Fe2O3T"
```

```
## Warning: Removed 4 rows containing missing values (geom_point).
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-54.png) 

