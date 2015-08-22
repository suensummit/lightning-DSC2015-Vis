---
title       : 運動數據視覺化 { 閃電秀 }
subtitle    : Lightning Talk | R <3 Baseball
author      : Summit Suen
job         : Taiwan R User Group
logo        : Taiwan-R-logo.png
github      : {user: suensummit, repo: lightning-DSC2015-Vis}
license     : by-nc-sa
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
--- 

## R 就是要拿來跑統計啊，不然要幹嘛？

![](http://helmetstudio.tk/agar/i/%E6%B3%9B%E8%88%9F%E5%93%A5.png)

--- .title-slide2 #id 

## 

---

## 工欲善其事，必先利其器


```r
# install.packages("Lahman")
library(Lahman)
library(dplyr)
totalRS <- Teams %>% select(yearID, R, G) %>% mutate(AvgRperG = R/G) %>% group_by(yearID) %>% summarise(sum(AvgRperG))
names(totalRS) <- c("yearID", "RUN")
head(totalRS)
```

```
## Source: local data frame [6 x 2]
## 
##   yearID      RUN
## 1   1871 93.12897
## 2   1872 95.21474
## 3   1873 73.15998
## 4   1874 58.55903
## 5   1875 70.08774
## 6   1876 47.01267
```

---

## 工欲善其事，必先利其器


```r
library(ggplot2)
ggplot(data = totalRS, aes(x = yearID, y = RUN)) + stat_smooth() + geom_line()
```

```
## geom_smooth: method="auto" and size of largest group is <1000, so using loess. Use 'method = x' to change the smoothing method.
```

![plot of chunk unnamed-chunk-2](assets/fig/unnamed-chunk-2-1.png) 

---

## 工欲善其事，必先利其器


```r
library(Lahman)
library(dplyr)
head(filter(Pitching, playerID == "wangch01"))
```

```
##   playerID yearID stint teamID lgID  W L  G GS CG SHO SV IPouts   H ER HR
## 1 wangch01   2005     1    NYA   AL  8 5 18 17  0   0  0    349 113 52  9
## 2 wangch01   2006     1    NYA   AL 19 6 34 33  2   1  1    654 233 88 12
## 3 wangch01   2007     1    NYA   AL 19 7 30 30  1   0  0    598 199 82  9
## 4 wangch01   2008     1    NYA   AL  8 2 15 15  1   0  0    285  90 43  4
## 5 wangch01   2009     1    NYA   AL  1 6 12  9  0   0  0    126  66 45  7
## 6 wangch01   2011     1    WAS   NL  4 3 11 11  0   0  0    187  67 28  8
##   BB  SO BAOpp  ERA IBB WP HBP BK BFP GF  R SH SF GIDP
## 1 32  47    NA 4.02   3  3   6  0 486  0 58 NA NA   NA
## 2 52  76    NA 3.63   4  6   2  1 900  1 92 NA NA   NA
## 3 59 104    NA 3.70   1  9   8  1 823  0 84 NA NA   NA
## 4 35  54    NA 4.07   1  0   3  0 402  0 44 NA NA   NA
## 5 19  29    NA 9.64   1  3   2  0 206  2 46 NA NA   NA
## 6 13  25    NA 4.04   0  2   1  0 264  0 35 NA NA   NA
```

---

## 工欲善其事，必先利其器


```r
# install.packages("Sxslt", repos = "http://www.omegahat.org/R", type = "source")
# require(devtools)
# install_github("openWAR", "beanumber")
require(openWAR)
getGameIds(date=as.Date("2015-08-20"))
```

```
## 
## Retrieving data from 2015-08-20 ...
## ...found 11 games
```

```
##  [1] "gid_2015_08_20_arimlb_cinmlb_1" "gid_2015_08_20_atlmlb_chnmlb_1"
##  [3] "gid_2015_08_20_chamlb_anamlb_1" "gid_2015_08_20_clemlb_nyamlb_1"
##  [5] "gid_2015_08_20_kcamlb_bosmlb_1" "gid_2015_08_20_minmlb_balmlb_1"
##  [7] "gid_2015_08_20_phimlb_miamlb_1" "gid_2015_08_20_sfnmlb_pitmlb_1"
##  [9] "gid_2015_08_20_tbamlb_houmlb_1" "gid_2015_08_20_texmlb_detmlb_1"
## [11] "gid_2015_08_20_wasmlb_colmlb_1"
```

---

## 工欲善其事，必先利其器


```r
gd = gameday(gameId="gid_2015_08_20_chamlb_anamlb_1")
```

```
## gid_2015_08_20_chamlb_anamlb_1
```

```r
gd$url
```

```
##                                                                                                        bis_boxscore.xml 
##      "http://gd2.mlb.com/components/game/mlb/year_2015/month_08/day_20/gid_2015_08_20_chamlb_anamlb_1/bis_boxscore.xml" 
##                                                                                                          inning_all.xml 
## "http://gd2.mlb.com/components/game/mlb/year_2015/month_08/day_20/gid_2015_08_20_chamlb_anamlb_1/inning/inning_all.xml" 
##                                                                                                          inning_hit.xml 
## "http://gd2.mlb.com/components/game/mlb/year_2015/month_08/day_20/gid_2015_08_20_chamlb_anamlb_1/inning/inning_hit.xml" 
##                                                                                                                game.xml 
##              "http://gd2.mlb.com/components/game/mlb/year_2015/month_08/day_20/gid_2015_08_20_chamlb_anamlb_1/game.xml" 
##                                                                                                         game_events.xml 
##       "http://gd2.mlb.com/components/game/mlb/year_2015/month_08/day_20/gid_2015_08_20_chamlb_anamlb_1/game_events.xml"
```

---

## 工欲善其事，必先利其器


```r
# now playing
gd$ds$inning
```

```
##  [1] 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 4 4 4 4 4 4 4 5 5 5 5
## [36] 5 5 5 5 5 5 5 5 5 5 5 5 6 6 6 6 6 6 6 7 7 7 7 7 7 7 8 8 8 8 8 8 8 8 8
## [71] 8 9 9 9 9 9 9
```

```r
gd$ds$description
```

```
##  [1] Shane Victorino singles on a sharp line drive to left fielder Melky Cabrera.                                                                                         
##  [2] Kole Calhoun lines out sharply to left fielder Melky Cabrera.                                                                                                        
##  [3] Mike Trout doubles (23) on a sharp line drive to center fielder Adam Eaton.   Shane Victorino scores.                                                                
##  [4] Albert Pujols grounds out, shortstop Alexei Ramirez to first baseman Adam LaRoche.                                                                                   
##  [5] C.  J.   Cron grounds out, third baseman Tyler Saladino to first baseman Adam LaRoche.                                                                               
##  [6] Adam Eaton doubles (20) on a line drive to right fielder Kole Calhoun.                                                                                               
##  [7] Tyler Saladino out on a sacrifice bunt, pitcher Nick Tropeano to first baseman C.  J.   Cron.   Adam Eaton to 3rd.                                                   
##  [8] Jose Abreu grounds out, shortstop Erick Aybar to first baseman C.   Cron.   Adam Eaton scores.                                                                       
##  [9] Melky Cabrera lines out sharply to left fielder Shane Victorino.                                                                                                     
## [10] Erick Aybar lines out softly to right fielder Avisail Garcia.                                                                                                        
## [11] Johnny Giavotella singles on a line drive to right fielder Avisail Garcia.  Johnny Giavotella out at 2nd, right fielder Avisail Garcia to shortstop Alexei Ramirez.  
## [12] Chris Iannetta pops out to second baseman Carlos Sanchez.                                                                                                            
## [13] Avisail Garcia grounds out, third baseman Kaleb Cowart to first baseman C.  J.   Cron.                                                                               
## [14] Adam LaRoche flies out to center fielder Mike Trout.                                                                                                                 
## [15] Alexei Ramirez called out on strikes.                                                                                                                                
## [16] Kaleb Cowart grounds out, second baseman Carlos Sanchez to first baseman Adam LaRoche.                                                                               
## [17] Shane Victorino pops out to second baseman Carlos Sanchez.                                                                                                           
## [18] Kole Calhoun singles on a soft line drive to right fielder Avisail Garcia.                                                                                           
## [19] Mike Trout walks.   Kole Calhoun to 2nd.                                                                                                                             
## [20] Albert Pujols walks.   Kole Calhoun to 3rd.    Mike Trout to 2nd.                                                                                                    
## [21] C.  J.   Cron grounds into a force out, third baseman Tyler Saladino to second baseman Carlos Sanchez.   Albert Pujols out at 2nd.                                   
## [22] Geovany Soto strikes out swinging.                                                                                                                                   
## [23] Carlos Sanchez grounds out sharply to first baseman C.  J.   Cron.                                                                                                   
## [24] Adam Eaton strikes out swinging.                                                                                                                                     
## [25] Erick Aybar grounds out, third baseman Tyler Saladino to first baseman Adam LaRoche.                                                                                 
## [26] Johnny Giavotella grounds out, shortstop Alexei Ramirez to first baseman Adam LaRoche.                                                                               
## [27] Chris Iannetta called out on strikes.                                                                                                                                
## [28] Tyler Saladino singles on a sharp ground ball to left fielder Shane Victorino.                                                                                       
## [29] Jose Abreu grounds out, shortstop Erick Aybar to first baseman C.   Cron.   Tyler Saladino to 2nd.                                                                   
## [30] Melky Cabrera walks.                                                                                                                                                 
## [31] Avisail Garcia grounds into a double play, shortstop Erick Aybar to second baseman Johnny Giavotella to first baseman C.   Cron.   Melky Cabrera out at 2nd.         
## [32] Kaleb Cowart grounds out, third baseman Tyler Saladino to first baseman Adam LaRoche.                                                                                
## [33] Shane Victorino lines out to second baseman Carlos Sanchez.                                                                                                          
## [34] Kole Calhoun singles on a sharp line drive to right fielder Avisail Garcia.                                                                                          
## [35] Mike Trout singles on a ground ball to left fielder Melky Cabrera.   Kole Calhoun to 2nd.                                                                            
## [36] Albert Pujols singles on a sharp line drive to center fielder Adam Eaton.   Kole Calhoun scores.    Mike Trout to 2nd.                                               
## [37] C.  J.   Cron called out on strikes.                                                                                                                                 
## [38] Adam LaRoche singles on a ground ball to right fielder Kole Calhoun.                                                                                                 
## [39] Alexei Ramirez singles on a sharp line drive to left fielder Shane Victorino.   Adam LaRoche to 2nd.                                                                 
## [40] Geovany Soto singles on a bunt ground ball to first baseman C.  J.   Cron.   Adam LaRoche to 3rd.    Alexei Ramirez to 2nd.                                          
## [41] Carlos Sanchez singles on a line drive to center fielder Mike Trout.   Adam LaRoche scores.    Alexei Ramirez to 3rd.    Geovany Soto to 2nd.                        
## [42] Adam Eaton out on a sacrifice fly to left fielder Shane Victorino.   Alexei Ramirez scores.    Geovany Soto to 3rd on the throw.    Carlos Sanchez to 2nd.           
## [43] Tyler Saladino singles on a soft line drive to left fielder Shane Victorino.   Geovany Soto scores.    Carlos Sanchez to 3rd.                                        
## [44] Jose Abreu doubles (23) on a sharp line drive to left fielder Shane Victorino.   Carlos Sanchez scores.    Tyler Saladino scores.                                    
## [45] Melky Cabrera lines out to third baseman Kaleb Cowart.                                                                                                               
## [46] Jose Alvarez intentionally walks Avisail Garcia.                                                                                                                     
## [47] Adam LaRoche strikes out swinging.                                                                                                                                   
## [48] Erick Aybar grounds out sharply, shortstop Alexei Ramirez to first baseman Adam LaRoche.                                                                             
## [49] Johnny Giavotella strikes out swinging.                                                                                                                              
## [50] Chris Iannetta singles on a ground ball to center fielder Adam Eaton.                                                                                                
## [51] Kaleb Cowart pops out to third baseman Tyler Saladino in foul territory.                                                                                             
## [52] Alexei Ramirez lines out to right fielder Kole Calhoun.                                                                                                              
## [53] Geovany Soto called out on strikes.                                                                                                                                  
## [54] Carlos Sanchez grounds out, third baseman Kaleb Cowart to first baseman C.  J.   Cron.                                                                               
## [55] Shane Victorino strikes out swinging.                                                                                                                                
## [56] Kole Calhoun walks.                                                                                                                                                  
## [57] Mike Trout grounds into a double play, shortstop Alexei Ramirez to second baseman Carlos Sanchez to first baseman Adam LaRoche.   Kole Calhoun out at 2nd.           
## [58] Adam Eaton singles on a bunt ground ball to first baseman C.  J.   Cron.                                                                                             
## [59] Tyler Saladino strikes out swinging.                                                                                                                                 
## [60] Jose Abreu strikes out swinging.                                                                                                                                     
## [61] Melky Cabrera flies out to center fielder Mike Trout.                                                                                                                
## [62] Albert Pujols grounds out, shortstop Alexei Ramirez to first baseman Adam LaRoche.                                                                                   
## [63] C.  J.   Cron grounds out softly, shortstop Alexei Ramirez to first baseman Adam LaRoche.                                                                            
## [64] Erick Aybar grounds out, shortstop Alexei Ramirez to first baseman Adam LaRoche.                                                                                     
## [65] Avisail Garcia doubles (15) on a sharp line drive to center fielder Mike Trout.                                                                                      
## [66] Adam LaRoche homers (11) on a line drive to right center field.    Avisail Garcia scores.                                                                            
## [67] Alexei Ramirez walks.                                                                                                                                                
## [68] Geovany Soto doubles (8) on a sharp line drive to center fielder Mike Trout.   Alexei Ramirez to 3rd.                                                                
## [69] Carlos Sanchez called out on strikes.                                                                                                                                
## [70] Trayce Thompson called out on strikes.                                                                                                                               
## [71] Tyler Saladino strikes out swinging.                                                                                                                                 
## [72] Johnny Giavotella grounds out, second baseman Carlos Sanchez to first baseman Adam LaRoche.                                                                          
## [73] Chris Iannetta flies out to right fielder Avisail Garcia.                                                                                                            
## [74] Kaleb Cowart grounds out, shortstop Alexei Ramirez to first baseman Adam LaRoche.                                                                                    
## [75] Jose Abreu grounds out, third baseman Kaleb Cowart to first baseman C.   Cron.                                                                                       
## [76] Melky Cabrera lines out sharply to center fielder Shane Victorino.                                                                                                   
## [77] Avisail Garcia flies out to center fielder Shane Victorino.                                                                                                          
## 90 Levels: Adam Eaton doubles (20) on a line drive to right fielder Kole Calhoun.   ...
```

---

## 工欲善其事，必先利其器


```r
str(gd$ds)
```

```
## Classes 'GameDayPlays', 'tbl_df', 'tbl' and 'data.frame':	77 obs. of  62 variables:
##  $ pitcherId     : num  500779 500779 500779 500779 500779 ...
##  $ batterId      : num  425664 594777 545361 405395 543068 ...
##  $ field_teamId  : chr  "145" "145" "145" "145" ...
##  $ ab_num        : num  5 6 7 8 9 1 2 3 4 13 ...
##  $ inning        : num  1 1 1 1 1 1 1 1 1 2 ...
##  $ half          : Factor w/ 2 levels "bottom","top": 1 1 1 1 1 2 2 2 2 1 ...
##  $ balls         : num  1 1 3 3 0 1 0 1 1 0 ...
##  $ strikes       : num  2 1 1 2 0 2 0 2 2 0 ...
##  $ endOuts       : num  0 1 1 2 3 0 1 2 3 1 ...
##  $ event         : Factor w/ 21 levels "Defensive Sub",..: 18 11 3 8 8 3 16 8 11 11 ...
##  $ actionId      : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ description   : Factor w/ 90 levels "Adam Eaton doubles (20) on a line drive to right fielder Kole Calhoun.  ",..: 81 59 68 9 23 1 85 50 65 40 ...
##  $ stand         : Factor w/ 2 levels "L","R": 2 1 2 2 2 1 2 2 1 2 ...
##  $ throws        : Factor w/ 2 levels "L","R": 1 1 1 1 1 2 2 2 2 1 ...
##  $ runnerMovement: chr  "[425664::1B::Single]" "" "[425664:1B::T:Pickoff Attempt 1B][545361::2B::Pickoff Attempt 1B]" "" ...
##  $ x             : num  95.1 89 175.1 107.8 101.2 ...
##  $ y             : num  144.7 74.8 60.5 157.1 166.5 ...
##  $ game_type     : Factor w/ 1 level "R": 1 1 1 1 1 1 1 1 1 1 ...
##  $ home_team     : Factor w/ 1 level "ana": 1 1 1 1 1 1 1 1 1 1 ...
##  $ home_teamId   : num  108 108 108 108 108 108 108 108 108 108 ...
##  $ home_lg       : Factor w/ 1 level "AL": 1 1 1 1 1 1 1 1 1 1 ...
##  $ away_team     : Factor w/ 1 level "cha": 1 1 1 1 1 1 1 1 1 1 ...
##  $ away_teamId   : num  145 145 145 145 145 145 145 145 145 145 ...
##  $ away_lg       : Factor w/ 1 level "AL": 1 1 1 1 1 1 1 1 1 1 ...
##  $ venueId       : num  1 1 1 1 1 1 1 1 1 1 ...
##  $ stadium       : Factor w/ 1 level "Angel Stadium of Anaheim": 1 1 1 1 1 1 1 1 1 1 ...
##  $ timestamp     : chr  "2015-08-21 02:15:29" "2015-08-21 02:17:28" "2015-08-21 02:19:40" "2015-08-21 02:22:24" ...
##  $ playerId.C    : num  434567 434567 434567 434567 434567 ...
##  $ playerId.1B   : num  425560 425560 425560 425560 425560 ...
##  $ playerId.2B   : num  570560 570560 570560 570560 570560 ...
##  $ playerId.3B   : num  573135 573135 573135 573135 573135 ...
##  $ playerId.SS   : num  493351 493351 493351 493351 493351 ...
##  $ playerId.LF   : num  466320 466320 466320 466320 466320 ...
##  $ playerId.CF   : num  594809 594809 594809 594809 594809 ...
##  $ playerId.RF   : num  541645 541645 541645 541645 541645 ...
##  $ batterPos     : chr  "LF" "RF" "CF" "DH" ...
##  $ batterName    : Factor w/ 30 levels "Abreu","Alvarez, J",..: 30 6 29 19 8 10 23 1 5 3 ...
##  $ pitcherName   : Factor w/ 30 levels "Abreu","Alvarez, J",..: 20 20 20 20 20 28 28 28 28 20 ...
##  $ runsOnPlay    : int  0 0 1 0 0 0 0 1 0 0 ...
##  $ startOuts     : num  0 0 1 1 2 0 0 1 2 0 ...
##  $ runsInInning  : int  1 1 1 1 1 1 1 1 1 0 ...
##  $ runsITD       : num  0 0 0 1 1 0 0 0 1 0 ...
##  $ runsFuture    : num  1 1 1 0 0 1 1 1 0 0 ...
##  $ start1B       : chr  NA "425664" "425664" NA ...
##  $ start2B       : chr  NA NA NA "545361" ...
##  $ start3B       : chr  NA NA NA NA ...
##  $ end1B         : chr  "425664" "425664" NA NA ...
##  $ end2B         : chr  NA NA "545361" "545361" ...
##  $ end3B         : chr  NA NA NA NA ...
##  $ outsInInning  : num  3 3 3 3 3 3 3 3 3 3 ...
##  $ startCode     : num  0 1 1 2 2 0 2 4 0 0 ...
##  $ endCode       : num  1 1 2 2 0 2 4 0 0 0 ...
##  $ fielderId     : num  NA 466320 NA 493351 573135 ...
##  $ gameId        : chr  "gid_2015_08_20_chamlb_anamlb_1" "gid_2015_08_20_chamlb_anamlb_1" "gid_2015_08_20_chamlb_anamlb_1" "gid_2015_08_20_chamlb_anamlb_1" ...
##  $ isPA          : logi  TRUE TRUE TRUE TRUE TRUE TRUE ...
##  $ isAB          : logi  TRUE TRUE TRUE TRUE TRUE TRUE ...
##  $ isHit         : logi  TRUE FALSE TRUE FALSE FALSE TRUE ...
##  $ isBIP         : logi  TRUE TRUE TRUE TRUE TRUE TRUE ...
##  $ our.x         : num  -74.6 -89.7 124.9 -42.9 -59.5 ...
##  $ our.y         : num  135.6 309.9 345.7 104.5 81.2 ...
##  $ r             : num  155 323 368 113 101 ...
##  $ theta         : num  2.07 1.85 1.22 1.96 2.2 ...
```

---

## 工欲善其事，必先利其器


```r
hit <- data.frame(x = cbind(c(gd$ds$x, gd$ds$our.x)), y = cbind(c(gd$ds$y, gd$ds$our.y)), isHit = gd$ds$isHit)
# img <- jpeg::readJPEG("assets/img/670px-Set-up-a-Baseball-Diamond-Step-1.jpg")
# g <- rasterGrob(img, interpolate=TRUE)
ggplot(data = hit, aes(x = x, y = y, color = isHit)) + coord_fixed() + 
#   annotation_custom(g, xmin=-Inf, xmax=Inf, ymin=-Inf, ymax=Inf) + 
  geom_point(size = 6, alpha = 0.4) + geom_density2d(colour = "black", size = 1, alpha = 0.5)
```

![plot of chunk unnamed-chunk-8](assets/fig/unnamed-chunk-8-1.png) 

---

## Recap

### 拿資料

- 靜態資料庫
  - [Lahman Database](http://lahman.r-forge.r-project.org/)

- 即時資料源
  - [openWAR](https://baseballwithr.wordpress.com/2014/03/17/introduction-to-openwar/)

- 隨時爬網頁
  - [httr](https://cran.r-project.org/web/packages/httr/index.html)
  - [XML](https://cran.r-project.org/web/packages/XML/index.html)
  - [RJSONIO](http://www.omegahat.org/RJSONIO/)
  - [RSelenium](http://ropensci.github.io/RSelenium/)
  - [phantomJS](http://phantomjs.org/)

---

## Recap

### 玩資料

- 賽博計量學
  - [pitchRx](http://cpsievert.github.io/pitchRx/#2D_animation)
  - [openWAR](https://baseballwithr.wordpress.com/2014/03/17/introduction-to-openwar/)
  
- 資料視覺化
  - [ggplot2](http://ggplot2.org/)
  - [recharts](http://yihui.name/recharts/)
  - [dygraphs](http://rstudio.github.io/dygraphs/)

---

## 關於視覺化，我想說的是⋯⋯

![](https://t.kfs.io/upload_images/41788/DSC15_LOGO-___large.jpg)
![](assets/img/Taiwan-R-logo.png)

![](http://datasci.tw/images/course/s001.png)
![](http://datasci.tw/images/course/s002.png)
![](http://datasci.tw/images/course/s003.png)
![](http://datasci.tw/images/course/s004.png)
![](http://datasci.tw/images/course/s005.png)
![](http://datasci.tw/images/course/s006.png)
![](http://datasci.tw/images/course/s007.png)
![](http://datasci.tw/images/course/s008.png)
