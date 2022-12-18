Зчитати WEB сторінку з сайту IMDB.com зі 100 фільмами 2017 року виходу за посиланням
«http://www.imdb.com/search/title?count=100&release_date=2017,2017&title_type=feature».
```r
library("rvest")
Url<-("http://www.imdb.com/search/title?count=100&release_date=2017,2017&title_type=feature")
movies<-read_html(Url)
movies
```
```r
{html_document}
<html xmlns:og="http://ogp.me/ns#" xmlns:fb="http://www.facebook.com/2008/fbml">
[1] <head>\n<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">\n<script type="text/javascript">var ue_t0 ...
[2] <body id="styleguide-v2" class="fixed">\n            <img height="1" width="1" style="display:none;visibility:hidden; ...
```
Зчитати rank_data
```r
rank_data_html <- html_nodes(movies, '.text-primary')
rank_data_html
```
```r
{xml_nodeset (100)}
 [1] <span class="lister-item-index unbold text-primary">1.</span>
 [2] <span class="lister-item-index unbold text-primary">2.</span>
 ...
```
Перетворити зчитані html rank_data дані в текст та числові значення
```r
rank_data<-html_text(rank_data_html)
rank_data<-as.numeric(rank_data)
head(rank_data)
```
```r
[1] 1 2 3 4 5 6
```
Зчитати title_data та перетворити зчитані html дані в текст
```r
title_data_html <-html_nodes(movies,".lister-item-header a")
title_data<-html_text(title_data_html)
head(title_data)
```
```r
[1] "Той, хто біжить по лезу 2049" "Воно"                         "Найвеличніший шоумен"        
[4] "Пастка"                       "Метелик"                      "Вбивство священного оленя" 
```
Зчитати runtime_data та перетворити зчитані html дані в текст
```r
runtime_data_html<-html_nodes(movies,".text-muted .runtime")
runtime_data<-html_text(runtime_data_html)
head(runtime_data)
```
```r
[1] "164 min" "135 min" "105 min" "104 min" "133 min" "121 min"
```
Перетворити runtime_data  з тексту в числові значення
```r
runtime_data <- substr(runtime_data, 1, 3)
runtime_data <- as.numeric(runtime_data)
head(runtime_data)
```
```r
[1] 164 135 105 104 133 121
```
Створити data.frame «movies» з наступними даними:
номер фільму (rank_data), назва фільму (title_data), тривалість (runtime_data).
```r
movies<-data.frame(Rank= rank_data, Titile=title_data, Time = runtime_data,stringsAsFactors = FALSE)
```
1. Виведіть перші 6 назв фільмів дата фрейму.
```r
head(movies, n=6)
```
```r
  Rank                       Titile Time
1    1 Той, хто біжить по лезу 2049  164
2    2                         Воно  135
3    3         Найвеличніший шоумен  105
4    4                       Пастка  104
5    5                      Метелик  133
6    6    Вбивство священного оленя  121
```
2. Виведіть всі назви фільмів с тривалістю більше 120 хв.
```r
movies120<-subset(movies,Time>120)
movies120
```
```r
 Rank                                   Titile Time
1     1             Той, хто біжить по лезу 2049  164
2     2                                     Воно  135
5     5                                  Метелик  133
6     6                Вбивство священного оленя  121
...
```
3. Скільки фільмів мають тривалість менше 100 хв.
```r
nrow (subset(movies, Time<100))
```
```r
[1] 13
```
