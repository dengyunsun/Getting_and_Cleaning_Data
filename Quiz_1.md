Quiz 1
======

|Attempts|Score|
|--------|-----|
|     1/3|15/15|


Question 1
----------
The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here: 

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv 

and load the data into R. The code book, describing the variable names is here: 

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf

How many properties are worth $1,000,000 or more?

### Answer
53

### Explanation

    > fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv"
    > download.file(fileUrl, destfile="./data/microdata.csv", method="curl")
    > microData <- read.table("./data/microdata.csv", sep=",", header=TRUE)
    > sum(!is.na(microData$VAL[microData$VAL==24]))
    > [1] 53
    
    
Question 2
----------
Use the data you loaded from Question 1. Consider the variable FES in the code book. Which of the "tidy data" principles does this variable violate?

### Answer
Tidy data has one variable per column.

### Explanation

    > fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv"
    > download.file(fileUrl, destfile="./data/microdata.csv", method="curl")
    > microData <- read.table("./data/microdata.csv", sep=",", header=TRUE)
    > microData$FES
    
    
Question 3
----------
Download the Excel spreadsheet on Natural Gas Aquisition Program here: 

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx

Read rows 18-23 and columns 7-15 into R and assign the result to a variable called:

    dat
    
What is the value of:

    sum(dat$Zip*dat$Ext,na.rm=T)
    
(original data source: http://catalog.data.gov/dataset/natural-gas-acquisition-program)
    
### Answer
36534720

### Explanation

    > fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx"
    > download.file(fileUrl, destfile="./data/nga.xlsx", method="curl")
    > dateDownloaded <- date()
    > library(xlsx)
    > colIndex <- 7:15
    > rowIndex <- 18:23
    > dat <- read.xlsx("./data/nga.xlsx", sheetIndex=1, header=TRUE, colIndex=colIndex, rowIndex=rowIndex)
    > sum(dat$Zip*dat$Ext,na.rm=T)
    > [1] 36534720
    

Question 4
----------
Read the XML data on Baltimore restaurants from here: 

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml 

How many restaurants have zipcode 21231?

### Answer
127

### Explanation

    > fileUrl <- "http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml"
    > doc <- xmlTreeParse(fileUrl, useInternal=TRUE)
    > rootNode <- xmlRoot(doc)
    > sum(xpathSApply(rootNode, "//zipcode", xmlValue)==21231)
    > [1] 127
    

Question 5
----------
The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here: 

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv 

using the fread() command load the data into an R object

    DT
    
Which of the following is the fastest way to calculate the average value of the variable

    pwgtp15
    
broken down by sex using the data.table package?

### Answer
DT[,mean(pwgtp15),by=SEX]

### Explanation

    > fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv"
    > download.file(fileUrl, destfile="./data/microdata3.csv", method="curl")
    > DT <- fread("./data/microdata3.csv")
    > file.info("./data/microdata3.csv")$size
    > system.time(DT[,mean(pwgtp15),by=SEX])
    > system.time(mean(DT[DT$SEX==1,]$pwgtp15))+system.time(mean(DT[DT$SEX==2,]$pwgtp15))
    > system.time(sapply(split(DT$pwgtp15,DT$SEX),mean))
    > system.time(mean(DT$pwgtp15,by=DT$SEX))
    > system.time(tapply(DT$pwgtp15,DT$SEX,mean))
    > system.time(rowMeans(DT)[DT$SEX==1])+system.time(rowMeans(DT)[DT$SEX==2])

