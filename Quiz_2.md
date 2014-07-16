Quiz 2
======

|Attempts|Score|
|--------|-----|
|     1/3|15/15|

Question 1
----------
Register an application with the Github API here https://github.com/settings/applications. Access the API to get information on your instructors repositories (hint: this is the url you want "https://api.github.com/users/jtleek/repos"). Use this data to find the time that the datasharing repo was created. What time was it created? This tutorial may be useful (https://github.com/hadley/httr/blob/master/demo/oauth2-github.r). You may also need to run the code in the base R package and not R studio.

### Answer
2013-11-07T13:25:07Z

### Explanation

    > library(httr)
    > oauth_endpoints("github")
    > myapp <- oauth_app("github", "ClientID", "ClientSecret")
    > github_token <- oauth2.0_token(oauth_endpoints("github"), myapp)
    > req <- GET("https://api.github.com/rate_limit", config(token = github_token))
    > stop_for_status(req)
    > content(req)
    > BROWSE("https://api.github.com/users/jtleek/repos",authenticate("Access Token","x-oauth-basic","basic"))


Question 2
----------
The sqldf package allows for execution of SQL commands on R data frames. We will use the sqldf package to practice the queries we might send with the dbSendQuery command in RMySQL. Download the American Community Survey data and load it into an R object called

    acs
    
https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv 

Which of the following commands will select only the data for the probability weights pwgtp1 with ages less than 50?

### Answer
sqldf("select pwgtp1 from acs where AGEP < 50")

### Explanation
    
    > library(sqldf)
    > acs <- read.csv("./getdata-data-ss06pid.csv", header=T, sep=",")
    > sqldf("select pwgtp1 from acs where AGEP < 50")


Question 3
----------
Using the same data frame you created in the previous problem, what is the equivalent function to unique(acs$AGEP)

### Answer
sqldf("select distinct AGEP from acs")

### Explanation

    > sqldf("select distinct AGEP from acs")


Question 4
----------
How many characters are in the 10th, 20th, 30th and 100th lines of HTML from this page: 

http://biostat.jhsph.edu/~jleek/contact.html 

(Hint: the nchar() function in R may be helpful)

### Answer
45 31 7 25

### Explanation

    > hurl <- "http://biostat.jhsph.edu/~jleek/contact.html" 
    > con <- url(hurl)
    > htmlCode <- readLines(con)
    > close(con)
    > sapply(htmlCode[c(10, 20, 30, 100)], nchar)
    <meta name="Distribution" content="Global" />               <script type="text/javascript"> 
                                               45                                            31 
                                            })();                 \t\t\t\t<ul class="sidemenu"> 
                                                7                                            25 


Question 5
----------
Read this data set into R and report the sum of the numbers in the fourth column. 

https://d396qusza40orc.cloudfront.net/getdata%2Fwksst8110.for 

Original source of the data: http://www.cpc.ncep.noaa.gov/data/indices/wksst8110.for 

(Hint this is a fixed width file format)

### Answer
32426.7

### Explanation

    > data <- read.csv("./getdata-wksst8110.for", header = TRUE)
    > file_name <- "./getdata-wksst8110.for"
    > df <- read.fwf(file=file_name,widths=c(-1,9,-5,4,4,-5,4,4,-5,4,4,-5,4,4), skip=4)
    > sum(df[, 4])
    > [1] 32426.7

