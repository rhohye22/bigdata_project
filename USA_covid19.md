
---
title: "USA covid-19 1/15"
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


## 주(state)별 확진자 data frame 로드하기  



### github에서 csv파일 불러오는 방법
* cvs파일 데이터가 출력되는 페이지의 링크를 복사
* 'us_state_url'이란 객체에 링크를 삽입




![screenshot](https://github.com/rhohye22/study_R/raw/main/image/csv_down.png)
 
 
```{r}
us_state_url <- 'https://github.com/nytimes/covid-19-data/raw/master/us-states.csv'
```

## us_covid 라는 이름을 가진 확진자 현황 데이터 프레임 생성.
```{r}
us_covid <- read.csv(file = us_state_url)
```


### 데이터프레임의 변수와 일자를 확인 


* cases: The total number of cases of Covid-19, including both confirmed and probable. 
  * 사례: 확인된 사례와 가능한 사례 모두를 포함하여 코로나19의 총 사례 수.


```{r message=TRUE}
tail(us_covid)
```

### 데이터 타입을 확인

```{r message=TRUE}
str(us_covid)
```
* 문자열과 정수로 구성되어 있음.


### 2021-01-15 일자의 데이터를 가진 us_covid_0115 데이터프레임 생성

```{r}
us_covid_0115 <- us_covid %>% filter(date == "2021-01-15")
str(us_covid_0115)
```

____


## 지도 데이터 불러오기


* maps 패기지를 불러온다.(지도 데이터가 있는 패키지)
```{r message=FALSE, warning=FALSE}
library(maps)
```


* ggplot2의 map_data 함수를 이용하여 있는 미국 지도 데이터를 로드

```{r}
state <- map_data(map = 'state')
head(state)
```
  * long : 경도 
  * lat : 위도


### geom_polygon() 함수로 지도 그리기

```{r}
ggplot(data = state, mapping = aes(x = long, y = lat, group = group))+
  geom_polygon(fill = 'white', color = 'black')+
  coord_map(projection = 'polyconic')
```


___


## 지도 데이터프레임(state)와 주별 확진자 데이터 프레임(us_covid_0115)을 조인

```{r}
str(us_covid_0115)
```
* **state** 라는 변수명으로 미국의 주를 표기.
* 변수의 데이터들이 **대문자**로 시작하는 것을 확인.

```{r}
str(state)
```
* **region** 라는 변수 명으로 미국의 주를 표기.
* 변수의 데이터들이 **소문자**로 시작하는 것을 확인.


### Join하기 앞서 us_covid_0115$state를 소문자로 변경

```{r}
us_covid_0115$state <- tolower(us_covid_0115$state)
```

* join하여 us_covid_map 데이터 프레임을 생성 

```{r}
us_covid_map <- left_join(state, us_covid_0115, by = c('region' = 'state'))
head(us_covid_map)
```


## us_covid_map 데이터 프레임을 이용하여 지도 그리기

```{r message=TRUE}
ggplot(data = us_covid_map, mapping = aes(x = long, y = lat, group = group))+
  geom_polygon(mapping = aes(fill = cases), color = 'black')+
  scale_fill_continuous(low = 'pink', high = 'darkred')+
  coord_map(projection = 'polyconic')

```






