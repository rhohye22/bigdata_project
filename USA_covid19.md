---
title: "USA covid-19 12/29"
output:
  html_document: default
  pdf_document: default
---

# 미국의 코로나19 발생 현황 시각화  
## 12/29일 발생 현황  
### [출처 : 미국 코로나 현황 csv](https://github.com/nytimes/covid-19-data)  

## 필요한 패키지 로드
```{r message=FALSE}
library(tidyverse)
#interactive graph 그리는 패키지
library(plotly)
library(ggiraphExtra)
#시계열 그래프를 위한 패키지
library(xts)
library(dygraphs)
library(knitr)
```

```{r message=TRUE}
search()
```


## 주(state)별 확진자 data frame 로드하기  
* github에서 csv파일 불러오는 방법

![screenshot](https://github.com/rhohye22/study_R/raw/main/image/csv_down.png)

* cvs파일 데이터가 출력되는 페이지의 링크를 복사


`us_state_url <- 'https://github.com/nytimes/covid-19-data/raw/master/us-states.csv'`
 

* 'us_state_url'이란 객체에 링크를 삽입


`us_covid <- read.cvs(file = us_state_url)`

### us_covid 라는 이름을 가진 확진자 현황 데이터 프레임 생성.




