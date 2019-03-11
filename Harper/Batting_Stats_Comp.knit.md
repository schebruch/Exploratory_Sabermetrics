
<!-- rnb-text-begin -->

---
title: "R Notebook"
output: html_notebook
---

This is an [R Markdown](http://rmarkdown.rstudio.com) Notebook. When you execute code within the notebook, the results appear beneath the code. 

Try executing this chunk by clicking the *Run* button within the chunk or by placing your cursor inside it and pressing *Ctrl+Shift+Enter*. 



Import required libraries

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxubGlicmFyeShzcWxkZilcbmxpYnJhcnkoZHBseXIpXG5saWJyYXJ5KGdncGxvdDIpXG5saWJyYXJ5KHJlc2hhcGUpXG5cbmBgYCJ9 -->

```r
library(sqldf)
library(dplyr)
library(ggplot2)
library(reshape)

```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->




Get the Batting Data

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuYmF0dGluZ0RhdGEgPC0gcmVhZC5jc3YocGFzdGUoZGF0YVBhdGgsIFwiQmF0dGluZy5jc3ZcIiwgc2VwID0gXCJcIikpXG5cbmBgYCJ9 -->

```r
battingData <- read.csv(paste(dataPath, "Batting.csv", sep = ""))

```

<!-- rnb-source-end -->

<!-- rnb-output-begin eyJkYXRhIjoiY2Fubm90IG9wZW4gZmlsZSAnRGF0YS9iYXNlYmFsbGRhdGFiYW5rLTIwMTkuMi9jb3JlL0JhdHRpbmcuY3N2JzogTm8gc3VjaCBmaWxlIG9yIGRpcmVjdG9yeUVycm9yIGluIGZpbGUoZmlsZSwgXCJydFwiKSA6IGNhbm5vdCBvcGVuIHRoZSBjb25uZWN0aW9uXG4ifQ== -->

```
cannot open file 'Data/baseballdatabank-2019.2/core/Batting.csv': No such file or directoryError in file(file, "rt") : cannot open the connection
```



<!-- rnb-output-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


National, American League Data

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuTkxEYXRhIDwtIHN1YnNldChiYXR0aW5nRGF0YSwgbGdJRCA9PSBcIk5MXCIgJiB5ZWFySUQgPj0gMjAxMiAmIEFCID49IDUwKVxuQUxEYXRhIDwtIHN1YnNldChiYXR0aW5nRGF0YSwgbGdJRCA9PSBcIkFMXCIgJiB5ZWFySUQgPj0gMjAxMiAmIEFCID49IDUwKVxubWxiRGF0YSA8LSBzdWJzZXQoYmF0dGluZ0RhdGEsIHllYXJJRCA+PSAyMDEyICYgQUIgPj0gNTApXG5gYGAifQ== -->

```r
NLData <- subset(battingData, lgID == "NL" & yearID >= 2012 & AB >= 50)
ALData <- subset(battingData, lgID == "AL" & yearID >= 2012 & AB >= 50)
mlbData <- subset(battingData, yearID >= 2012 & AB >= 50)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



Phillies Data

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxucGhpbGxpZXNEYXRhIDwtIHNlbGVjdChmaWx0ZXIoYmF0dGluZ0RhdGEsIHRlYW1JRCA9PSBcIlBISVwiICYgeWVhcklEID49IDIwMTIgJiBBQiA+PSA1MCksIGMoXCJwbGF5ZXJJRFwiLCBcInllYXJJRFwiLCBcIkdcIiwgXCJBQlwiLCBcIlJcIiwgXCJIXCIsIFwiWDJCXCIsIFwiWDNCXCIsIFwiQkJcIixcIlNGXCIsIFwiSFJcIiwgXCJSQklcIikpXG5cblxuXG5gYGAifQ== -->

```r
philliesData <- select(filter(battingData, teamID == "PHI" & yearID >= 2012 & AB >= 50), c("playerID", "yearID", "G", "AB", "R", "H", "X2B", "X3B", "BB","SF", "HR", "RBI"))

```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->




Make Columns Function

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuXG4jbm90ZSBJIGRvbid0IGlubGN1ZGUgSEJQIG9yIElCQiBpbiBPQlAgc2luY2UgaXQgZG9lc24ndCByZWFsbHkgbWVhc3VyZSB0aGUgYmF0dGVyJ3Mgc2tpbGxcbiNYMUIgcmVmZXJzIHRvIHNpbmdsZXMgKEkgY2FuJ3Qgc3RhcnQgYSBjb2wgbmFtZSB3aXRoIGEgMSlcbmZpbGxfcmVsZXZhbnRfY29scyA8LSBmdW5jdGlvbihkZil7XG4gIHRtcCA8LSAoIGRmICU+JSBtdXRhdGUoQkEgPSBIL0FCLCBPQlAgPSAoSCArIEJCKS8oQUIgKyBCQiksU0xHID0gKDIqWDJCICsgMypYM0IgKyA0KkhSICsgQkIgKyAoSCAtIFgyQiAtIFgzQiAtIEhSKSkvQUIsIFgxQiA9IEggLSBIUiAtIFgyQiAtIFgzQiwgT1BTID0gU0xHICsgT0JQKSlcbiAgXG4gIHJldHVybiAodG1wICU+JSBtdXRhdGVfYXQodmFycyhCQSwgQUIsIE9QUywgT0JQLCBTTEcpLCBmdW5zKHJvdW5kKC4sIDMpKSkpXG5cbn1cbmBgYCJ9 -->

```r

#note I don't inlcude HBP or IBB in OBP since it doesn't really measure the batter's skill
#X1B refers to singles (I can't start a col name with a 1)
fill_relevant_cols <- function(df){
  tmp <- ( df %>% mutate(BA = H/AB, OBP = (H + BB)/(AB + BB),SLG = (2*X2B + 3*X3B + 4*HR + BB + (H - X2B - X3B - HR))/AB, X1B = H - HR - X2B - X3B, OPS = SLG + OBP))
  
  return (tmp %>% mutate_at(vars(BA, AB, OPS, OBP, SLG), funs(round(., 3))))

}
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



Make Columns for each data set

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxucGhpbGxpZXNEYXRhIDwtIGZpbGxfcmVsZXZhbnRfY29scyhwaGlsbGllc0RhdGEpXG5cbk5MRGF0YSA8LSBmaWxsX3JlbGV2YW50X2NvbHMoTkxEYXRhKVxuQUxEYXRhIDwtIGZpbGxfcmVsZXZhbnRfY29scyhBTERhdGEpXG5tbGJEYXRhIDwtIGZpbGxfcmVsZXZhbnRfY29scyhtbGJEYXRhKVxuaGFycGVyRGF0YSA8LSBmaWx0ZXIoYmF0dGluZ0RhdGEsIHBsYXllcklEID09IFwiaGFycGVicjAzXCIgJiB5ZWFySUQgPj0gMjAxMilcbmhhcnBlckRhdGEgPC0gZmlsbF9yZWxldmFudF9jb2xzKGhhcnBlckJhdHRpbmcpXG5gYGAifQ== -->

```r
philliesData <- fill_relevant_cols(philliesData)

NLData <- fill_relevant_cols(NLData)
ALData <- fill_relevant_cols(ALData)
mlbData <- fill_relevant_cols(mlbData)
harperData <- filter(battingData, playerID == "harpebr03" & yearID >= 2012)
harperData <- fill_relevant_cols(harperBatting)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



Function to compute average team statistics grouped by yearID

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2V0Q29uZGVuc2VkREZCeVN0YXQgPC0gZnVuY3Rpb24oZGZOYW1lLCBzdGF0VG9Db21wdXRlKXtcbiAgXG4gIHJldHVybihzcWxkZihwYXN0ZTAoXCJzZWxlY3QgeWVhcklELCBhdmcoXCIsc3RhdFRvQ29tcHV0ZSwgXCIpIGFzIFRlYW1fQXZnX1wiLCBzdGF0VG9Db21wdXRlLCBcIiBmcm9tIFwiLGRmTmFtZSwgXCIgZ3JvdXAgYnkgeWVhcklEXCIpKSlcbn1cbmBgYCJ9 -->

```r
getCondensedDFByStat <- function(dfName, statToCompute){
  
  return(sqldf(paste0("select yearID, avg(",statToCompute, ") as Team_Avg_", statToCompute, " from ",dfName, " group by yearID")))
}
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



Compute average statistics over time starting from 2012

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuXG4jZ2V0IHRoZSBjb25kZW5zZWQgYmF0dGluZyBhdmVyYWdlXG5waGlsbGllc0NvbmRlbnNlZEJBIDwtIGdldENvbmRlbnNlZERGQnlTdGF0KFwicGhpbGxpZXNEYXRhXCIsIFwiQkFcIilcbkFMQ29uZGVuc2VkQkEgPC0gZ2V0Q29uZGVuc2VkREZCeVN0YXQoXCJBTERhdGFcIiwgXCJCQVwiKVxuTkxDb25kZW5zZWRCQSA8LSBnZXRDb25kZW5zZWRERkJ5U3RhdChcIk5MRGF0YVwiLCBcIkJBXCIpXG5tbGJDb25kZW5zZWRCQSA8LSBnZXRDb25kZW5zZWRERkJ5U3RhdChcIm1sYkRhdGFcIiwgXCJCQVwiKVxuaGFycGVyQ29uZGVuc2VkQkEgPC0gZ2V0Q29uZGVuc2VkREZCeVN0YXQoXCJoYXJwZXJEYXRhXCIsIFwiQkFcIilcblxuXG4jQ29uZGVuc2VkIE9CUFxucGhpbGxpZXNDb25kZW5zZWRPQlAgPC0gZ2V0Q29uZGVuc2VkREZCeVN0YXQoXCJwaGlsbGllc0RhdGFcIiwgXCJPQlBcIilcbkFMQ29uZGVuc2VkT0JQIDwtIGdldENvbmRlbnNlZERGQnlTdGF0KFwiQUxEYXRhXCIsIFwiT0JQXCIpXG5OTENvbmRlbnNlZE9CUCA8LSBnZXRDb25kZW5zZWRERkJ5U3RhdChcIk5MRGF0YVwiLCBcIk9CUFwiKVxubWxiQ29uZGVuc2VkT0JQIDwtIGdldENvbmRlbnNlZERGQnlTdGF0KFwibWxiRGF0YVwiLCBcIk9CUFwiKVxuaGFycGVyQ29uZGVuc2VkT0JQIDwtIGdldENvbmRlbnNlZERGQnlTdGF0KFwiaGFycGVyRGF0YVwiLCBcIk9CUFwiKVxuXG5cblxuXG4jQ29uZGVuc2VkIFNMR1xucGhpbGxpZXNDb25kZW5zZWRTTEcgPC0gZ2V0Q29uZGVuc2VkREZCeVN0YXQoXCJwaGlsbGllc0RhdGFcIiwgXCJTTEdcIilcbkFMQ29uZGVuc2VkU0xHIDwtIGdldENvbmRlbnNlZERGQnlTdGF0KFwiQUxEYXRhXCIsIFwiU0xHXCIpXG5OTENvbmRlbnNlZFNMRyA8LSBnZXRDb25kZW5zZWRERkJ5U3RhdChcIk5MRGF0YVwiLCBcIlNMR1wiKVxubWxiQ29uZGVuc2VkU0xHIDwtIGdldENvbmRlbnNlZERGQnlTdGF0KFwibWxiRGF0YVwiLCBcIlNMR1wiKVxuaGFycGVyQ29uZGVuc2VkU0xHIDwtIGdldENvbmRlbnNlZERGQnlTdGF0KFwiaGFycGVyRGF0YVwiLCBcIlNMR1wiKVxuXG5cbiNDb25kZW5zZWQgT1BTXG5waGlsbGllc0NvbmRlbnNlZE9QUyA8LSBnZXRDb25kZW5zZWRERkJ5U3RhdChcInBoaWxsaWVzRGF0YVwiLCBcIk9QU1wiKVxuQUxDb25kZW5zZWRPUFMgPC0gZ2V0Q29uZGVuc2VkREZCeVN0YXQoXCJBTERhdGFcIiwgXCJPUFNcIilcbk5MQ29uZGVuc2VkT1BTIDwtIGdldENvbmRlbnNlZERGQnlTdGF0KFwiTkxEYXRhXCIsIFwiT1BTXCIpXG5tbGJDb25kZW5zZWRPUFMgPC0gZ2V0Q29uZGVuc2VkREZCeVN0YXQoXCJtbGJEYXRhXCIsIFwiT1BTXCIpXG5oYXJwZXJDb25kZW5zZWRPUFMgPC0gZ2V0Q29uZGVuc2VkREZCeVN0YXQoXCJoYXJwZXJEYXRhXCIsIFwiT1BTXCIpXG5cblxuXG5gYGAifQ== -->

```r

#get the condensed batting average
philliesCondensedBA <- getCondensedDFByStat("philliesData", "BA")
ALCondensedBA <- getCondensedDFByStat("ALData", "BA")
NLCondensedBA <- getCondensedDFByStat("NLData", "BA")
mlbCondensedBA <- getCondensedDFByStat("mlbData", "BA")
harperCondensedBA <- getCondensedDFByStat("harperData", "BA")


#Condensed OBP
philliesCondensedOBP <- getCondensedDFByStat("philliesData", "OBP")
ALCondensedOBP <- getCondensedDFByStat("ALData", "OBP")
NLCondensedOBP <- getCondensedDFByStat("NLData", "OBP")
mlbCondensedOBP <- getCondensedDFByStat("mlbData", "OBP")
harperCondensedOBP <- getCondensedDFByStat("harperData", "OBP")




#Condensed SLG
philliesCondensedSLG <- getCondensedDFByStat("philliesData", "SLG")
ALCondensedSLG <- getCondensedDFByStat("ALData", "SLG")
NLCondensedSLG <- getCondensedDFByStat("NLData", "SLG")
mlbCondensedSLG <- getCondensedDFByStat("mlbData", "SLG")
harperCondensedSLG <- getCondensedDFByStat("harperData", "SLG")


#Condensed OPS
philliesCondensedOPS <- getCondensedDFByStat("philliesData", "OPS")
ALCondensedOPS <- getCondensedDFByStat("ALData", "OPS")
NLCondensedOPS <- getCondensedDFByStat("NLData", "OPS")
mlbCondensedOPS <- getCondensedDFByStat("mlbData", "OPS")
harperCondensedOPS <- getCondensedDFByStat("harperData", "OPS")

```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



Plot by either BA, OBP, SLG, or OPS

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxucGxvdF9ieV9zdGF0aXN0aWMgPC0gZnVuY3Rpb24oc3RhdCl7XG4gIHBoaWxsaWVzIDwtIGdldChwYXN0ZTAoXCJwaGlsbGllc0NvbmRlbnNlZFwiLCBzdGF0KSlcbiAgTkwgPC0gZ2V0KHBhc3RlMChcIk5MQ29uZGVuc2VkXCIsIHN0YXQpKVxuICBBTCA8LSBnZXQocGFzdGUwKFwiQUxDb25kZW5zZWRcIiwgc3RhdCkpXG4gIG1sYiA8LSBnZXQocGFzdGUwKFwibWxiQ29uZGVuc2VkXCIsIHN0YXQpKVxuICBoYXJwZXIgPC0gIGdldChwYXN0ZTAoXCJoYXJwZXJDb25kZW5zZWRcIiwgc3RhdCkpXG5cbiAgdG1wLmxpc3QgPC0gbGlzdChwaGlsbGllcywgTkwsIEFMLCBtbGIsIGhhcnBlcilcbiAgbmV3REYgPC0gZG8uY2FsbChyYmluZCwgdG1wLmxpc3QpXG4gIG5ld0RGJGxlYWd1ZSA8LSBmYWN0b3IocmVwKGMoXCJwaGlsbGllc1wiLCBcIkFMXCIsIFwiTkxcIiwgXCJNTEJcIiwgXCJIYXJwZXJcIiksIGVhY2ggPSBzYXBwbHkodG1wLmxpc3QsIG5yb3cpKSlcbiAgZyA8LSBnZ3Bsb3QobmV3REYsIGFlcyh4ID0geWVhcklELCB5ID0gZ2V0KHBhc3RlMChcIlRlYW1fQXZnX1wiLCBzdGF0KSksIGNvbG9yID0gbGVhZ3VlKSkgKyB4bGFiKFwiWWVhclwiKSArIHlsYWIocGFzdGUoc3RhdCkpXG4gIGcgPC0gZyArIGdlb21fbGluZSgpICtnZ3RpdGxlKHBhc3RlMChzdGF0LCBcIiBmcm9tIDIwMTIgdG8gMjAxOFwiKSkgXG4gIHJldHVybiAoZylcbn1cbmBgYCJ9 -->

```r
plot_by_statistic <- function(stat){
  phillies <- get(paste0("philliesCondensed", stat))
  NL <- get(paste0("NLCondensed", stat))
  AL <- get(paste0("ALCondensed", stat))
  mlb <- get(paste0("mlbCondensed", stat))
  harper <-  get(paste0("harperCondensed", stat))

  tmp.list <- list(phillies, NL, AL, mlb, harper)
  newDF <- do.call(rbind, tmp.list)
  newDF$league <- factor(rep(c("phillies", "AL", "NL", "MLB", "Harper"), each = sapply(tmp.list, nrow)))
  g <- ggplot(newDF, aes(x = yearID, y = get(paste0("Team_Avg_", stat)), color = league)) + xlab("Year") + ylab(paste(stat))
  g <- g + geom_line() +ggtitle(paste0(stat, " from 2012 to 2018")) 
  return (g)
}
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



plot using the function above all the statistics 

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuXG4jcGxvdCBiYXR0aW5nIGF2ZXJhZ2VcblxucGxvdF9ieV9zdGF0aXN0aWMoXCJCQVwiKVxucGxvdF9ieV9zdGF0aXN0aWMoXCJPQlBcIilcbnBsb3RfYnlfc3RhdGlzdGljKFwiU0xHXCIpXG5wbG90X2J5X3N0YXRpc3RpYyhcIk9QU1wiKVxuXG5gYGAifQ== -->

```r

#plot batting average

plot_by_statistic("BA")
plot_by_statistic("OBP")
plot_by_statistic("SLG")
plot_by_statistic("OPS")

```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



Salaries

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuc2FsYXJpZXMgPC0gcmVhZC5jc3YocGFzdGUwKGRhdGFQYXRoLCBcIlNhbGFyaWVzLmNzdlwiKSlcblxuI2dldCBhdmVyYWdlIHNhbGFyeVxuYXZnU2FsYXJpZXNCeVRlYW0gPC0gc3FsZGYoXCJzZWxlY3QgdGVhbUlELCB5ZWFySUQsIGF2ZyhTYWxhcnkpIGFzICdUZWFtX0F2Z19TYWwnIGZyb20gc2FsYXJpZXMgd2hlcmUgeWVhcklEID49IDIwMTIgZ3JvdXAgYnkgdGVhbUlEXCIpXG5gYGAifQ== -->

```r
salaries <- read.csv(paste0(dataPath, "Salaries.csv"))

#get average salary
avgSalariesByTeam <- sqldf("select teamID, yearID, avg(Salary) as 'Team_Avg_Sal' from salaries where yearID >= 2012 group by teamID")
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->




Add a new chunk by clicking the *Insert Chunk* button on the toolbar or by pressing *Ctrl+Alt+I*.

When you save the notebook, an HTML file containing the code and output will be saved alongside it (click the *Preview* button or press *Ctrl+Shift+K* to preview the HTML file).

The preview shows you a rendered HTML copy of the contents of the editor. Consequently, unlike *Knit*, *Preview* does not run any R code chunks. Instead, the output of the chunk when it was last run in the editor is displayed.

<!-- rnb-text-end -->

