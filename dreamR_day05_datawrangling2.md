# DreamR day05 Data Wrangling 2 
Sejin Park  
2015년 9월 9일  




```r
library("dplyr")
```

```
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
movie_thiswk = read.csv('weekly_thiswk.csv', stringsAsFactors = FALSE)

movie_lastwk = read.csv('weekly_lastwk.csv', stringsAsFactors = FALSE)

movie_data = read.csv('weekly_boxoffice_data.csv', stringsAsFactors = FALSE)

movie_thiswk
```

```
##    thiswk                             title
## 1       1                          War Room
## 2       2            Straight Outta Compton
## 3       3               A Walk in the Woods
## 4       4 Mission Impossible : Rogue Nation
## 5       5          The Transporter Refueled
## 6       6                         No Escape
## 7       7                        Inside Out
## 8       8           The Man From U.N.C.L.E.
## 9       9        Un Gallo Con Muchos Huevos
## 10     10                        Sinister 2
```

```r
movie_lastwk
```

```
##    lastwk                             title
## 1       1            Straight Outta Compton
## 2       2                          War Room
## 3       3 Mission Impossible : Rogue Nation
## 4       4                         No Escape
## 5       5                        Sinister 2
## 6       6           The Man From U.N.C.L.E.
## 7       7                  Hitman: Agent 47
## 8       8                           Ant-Man
## 9       9                    Jurassic World
## 10     10                          The Gift
```

```r
movie_thiswk %>%
  inner_join(movie_lastwk, by = 'title')
```

```
##   thiswk                             title lastwk
## 1      1                          War Room      2
## 2      2            Straight Outta Compton      1
## 3      4 Mission Impossible : Rogue Nation      3
## 4      6                         No Escape      4
## 5      8           The Man From U.N.C.L.E.      6
## 6     10                        Sinister 2      5
```

```r
movie_thiswk %>%
  left_join(movie_lastwk, by = 'title')
```

```
##    thiswk                             title lastwk
## 1       1                          War Room      2
## 2       2            Straight Outta Compton      1
## 3       3               A Walk in the Woods     NA
## 4       4 Mission Impossible : Rogue Nation      3
## 5       5          The Transporter Refueled     NA
## 6       6                         No Escape      4
## 7       7                        Inside Out     NA
## 8       8           The Man From U.N.C.L.E.      6
## 9       9        Un Gallo Con Muchos Huevos     NA
## 10     10                        Sinister 2      5
```

```r
movie_thiswk %>%
  left_join(movie_data, by = c('title'='titl'))
```

```
##    thiswk                             title
## 1       1                          War Room
## 2       2            Straight Outta Compton
## 3       3               A Walk in the Woods
## 4       4 Mission Impossible : Rogue Nation
## 5       5          The Transporter Refueled
## 6       6                         No Escape
## 7       7                        Inside Out
## 8       8           The Man From U.N.C.L.E.
## 9       9        Un Gallo Con Muchos Huevos
## 10     10                        Sinister 2
##                            distributor weekendGross     cumGross wksOut
## 1                     TriStar Pictures  $12,550,000  $27,860,000      2
## 2                   Universal Pictures  $11,061,000 $149,997,000      4
## 3                 Broad Green Pictures  $10,348,000  $12,245,900      1
## 4                   Paramount Pictures   $9,300,000 $182,537,000      6
## 5                           EuropaCorp   $9,000,000   $9,000,000      1
## 6                The Weinstein Company   $7,032,000  $20,033,100      2
## 7  Walt Disney Studios Motion Pictures   $4,544,000 $349,617,000     12
## 8   Warner Bros. Pictures Distribution   $4,445,000  $40,379,000      4
## 9                      Pantelion Films   $4,420,000   $4,420,000      1
## 10                      Focus Features   $4,252,000  $24,591,900      3
##    theaters
## 1      1526
## 2      3094
## 3      1960
## 4      2849
## 5      3434
## 6      3415
## 7      2967
## 8      2102
## 9       395
## 10     2651
```

```r
movie_rank = movie_thiswk %>%
  left_join(movie_lastwk, by = 'title')

movie_rank$lastwk
```

```
##  [1]  2  1 NA  3 NA  4 NA  6 NA  5
```

```r
is.na(movie_rank$lastwk) 
```

```
##  [1] FALSE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE
```

```r
complete.cases(movie_rank)
```

```
##  [1]  TRUE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE
```

```r
movie_rank %>%
  filter(is.na(movie_rank$lastwk))
```

```
##   thiswk                      title lastwk
## 1      3        A Walk in the Woods     NA
## 2      5   The Transporter Refueled     NA
## 3      7                 Inside Out     NA
## 4      9 Un Gallo Con Muchos Huevos     NA
```

```r
movie_rank %>%
  filter(complete.cases(movie_rank))
```

```
##   thiswk                             title lastwk
## 1      1                          War Room      2
## 2      2            Straight Outta Compton      1
## 3      4 Mission Impossible : Rogue Nation      3
## 4      6                         No Escape      4
## 5      8           The Man From U.N.C.L.E.      6
## 6     10                        Sinister 2      5
```


```r
mean(c(1,2,3,4,5))
```

```
## [1] 3
```

```r
mean(c(1,2,3,4,5,NA))
```

```
## [1] NA
```



```r
numbers = c(1:4, NA, 5:10)
mean(numbers)
```

```
## [1] NA
```

```r
# 해결책 1 
mean(numbers, na.rm=TRUE)
```

```
## [1] 5.5
```

```r
# 해결책 2
numbers_nona = ifelse(is.na(numbers)==TRUE, 0, numbers)

mean(numbers_nona)
```

```
## [1] 5
```

```r
ifelse(is.na(numbers)==TRUE, 0, numbers)
```

```
##  [1]  1  2  3  4  0  5  6  7  8  9 10
```

```r
movie_rank %>% 
  mutate(
    lastwk_na = ifelse(is.na(lastwk) == TRUE, 0, lastwk)
  )
```

```
##    thiswk                             title lastwk lastwk_na
## 1       1                          War Room      2         2
## 2       2            Straight Outta Compton      1         1
## 3       3               A Walk in the Woods     NA         0
## 4       4 Mission Impossible : Rogue Nation      3         3
## 5       5          The Transporter Refueled     NA         0
## 6       6                         No Escape      4         4
## 7       7                        Inside Out     NA         0
## 8       8           The Man From U.N.C.L.E.      6         6
## 9       9        Un Gallo Con Muchos Huevos     NA         0
## 10     10                        Sinister 2      5         5
```

```r
#install.packages('stringr')
library('stringr')

str_replace('꿈꾸는 데이터 디자이너','꿈꾸는','나는')
```

```
## [1] "나는 데이터 디자이너"
```

```r
str_replace('꿈꾸는 꿈꾸는 데이터 디자이너','꿈꾸는','나는')
```

```
## [1] "나는 꿈꾸는 데이터 디자이너"
```

```r
str_replace_all('꿈꾸는 꿈꾸는 데이터 디자이너','꿈꾸는','나는')
```

```
## [1] "나는 나는 데이터 디자이너"
```

str_detect(원본문자열, 찾으려고 하는 패턴/단어)


```r
str_detect(movie_data$distributor,'Pictures')
```

```
##  [1]  TRUE  TRUE  TRUE  TRUE FALSE FALSE  TRUE  TRUE FALSE FALSE  TRUE
## [12]  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE FALSE FALSE  TRUE
## [23]  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE  TRUE  TRUE FALSE FALSE
## [34]  TRUE FALSE  TRUE
```

```r
movie_data %>%
  filter(str_detect(distributor, 'Pictures'))
```

```
##                                  titl                         distributor
## 1                            War Room                    TriStar Pictures
## 2              Straight Outta Compton                  Universal Pictures
## 3                 A Walk in the Woods                Broad Green Pictures
## 4   Mission Impossible : Rogue Nation                  Paramount Pictures
## 5                          Inside Out Walt Disney Studios Motion Pictures
## 6             The Man From U.N.C.L.E.  Warner Bros. Pictures Distribution
## 7                             Minions                  Universal Pictures
## 8                             Ant-Man Walt Disney Studios Motion Pictures
## 9                      Jurassic World                  Universal Pictures
## 10                         Trainwreck                  Universal Pictures
## 11                           Vacation  Warner Bros. Pictures Distribution
## 12                              Aloha             Sony Pictures Releasing
## 13                We Are Your Friends  Warner Bros. Pictures Distribution
## 14                   Mistress America            Fox Searchlight Pictures
## 15                            Grandma              Sony Pictures Classics
## 16                  Learning to Drive                Broad Green Pictures
## 17        The Diary of a Teenage Girl              Sony Pictures Classics
## 18 Steve Jobs: The Man in the Machine                   Magnolia Pictures
## 19                     Irrational Man              Sony Pictures Classics
##    weekendGross     cumGross wksOut theaters
## 1   $12,550,000  $27,860,000      2     1526
## 2   $11,061,000 $149,997,000      4     3094
## 3   $10,348,000  $12,245,900      1     1960
## 4    $9,300,000 $182,537,000      6     2849
## 5    $4,544,000 $349,617,000     12     2967
## 6    $4,445,000  $40,379,000      4     2102
## 7    $3,911,810 $329,783,000      9     1927
## 8    $3,779,000 $174,082,000      8     2967
## 9    $3,416,920 $647,461,000     13     1571
## 10   $1,838,200 $107,518,000      8      808
## 11   $1,450,000  $56,957,000      6     1045
## 12     $850,000     $850,000      1      135
## 13     $811,000   $3,331,000      2     2333
## 14     $745,000   $1,819,700      4      512
## 15     $612,497   $1,184,510      3       52
## 16     $445,352     $713,301      3       70
## 17     $200,846   $1,284,190      5      255
## 18     $187,000     $187,000      1       68
## 19      $68,067   $3,824,400      8       54
```

```r
str_detect(movie_data$distributor, '^Sony')
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [12] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE
## [23] FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE
## [34] FALSE FALSE  TRUE
```

```r
movie_data %>%
  filter(str_detect(distributor,'^Sony'))
```

```
##                          titl             distributor weekendGross
## 1                       Aloha Sony Pictures Releasing     $850,000
## 2                     Grandma  Sony Pictures Classics     $612,497
## 3 The Diary of a Teenage Girl  Sony Pictures Classics     $200,846
## 4              Irrational Man  Sony Pictures Classics      $68,067
##     cumGross wksOut theaters
## 1   $850,000      1      135
## 2 $1,184,510      3       52
## 3 $1,284,190      5      255
## 4 $3,824,400      8       54
```


## Date


```r
#현재시각
time = Sys.time()

#요일 
weekdays(time)
```

```
## [1] "Wednesday"
```

```r
# 월
months(time)
```

```
## [1] "September"
```

```r
# 분기 
quarters(time)
```

```
## [1] "Q3"
```

다양한 포맷에 대해서는 ?strftime 을 통해 살펴볼 수 있다. 


```r
strftime(time, "%m/%d")
```

```
## [1] "09/09"
```

```r
strftime(time, "%Y-%m-%d")
```

```
## [1] "2015-09-09"
```

```r
strftime(time, "%y-%m-%d")
```

```
## [1] "15-09-09"
```

```r
strftime(time, "%Y")
```

```
## [1] "2015"
```

```r
#일요일을 0으로 두었을 때 요일 표기 
strftime(time, "%w")
```

```
## [1] "3"
```

## chr to Date


```r
str_time = '20150909'
date_time = as.Date(str_time, format = '%Y%m%d')

date_time + 30
```

```
## [1] "2015-10-09"
```


## aapl


```r
#install.packages('gcookbook')
library(gcookbook)
aapl %>%
  mutate(year = strftime(date, '%Y')) %>%
  group_by(year) %>%
  summarise(price = mean(adj_price)) %>%
  print(n=30)
```

```
## Source: local data frame [29 x 2]
## 
##    year      price
## 1  1984   2.897647
## 2  1985   2.294423
## 3  1986   3.683462
## 4  1987   8.875094
## 5  1988   9.492500
## 6  1989   9.637692
## 7  1990   8.797692
## 8  1991  12.390385
## 9  1992  13.087170
## 10 1993   9.775769
## 11 1994   8.340192
## 12 1995  10.013654
## 13 1996   6.211731
## 14 1997   4.493077
## 15 1998   7.581321
## 16 1999  14.565000
## 17 2000  22.487115
## 18 2001  10.072308
## 19 2002   9.521731
## 20 2003   9.179423
## 21 2004  17.715283
## 22 2005  46.812500
## 23 2006  70.731731
## 24 2007 127.844615
## 25 2008 141.267692
## 26 2009 146.320000
## 27 2010 258.890577
## 28 2011 361.765192
## 29 2012 575.487250
```

```r
aapl %>%
  mutate(weekday = weekdays(date))  %>%
  group_by(weekday) %>%
  summarise(price = mean(adj_price))
```

```
## Source: local data frame [3 x 2]
## 
##    weekday     price
## 1   Friday  63.44906
## 2   Monday 334.02000
## 3 Thursday  63.46796
```
