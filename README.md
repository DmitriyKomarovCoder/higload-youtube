# Проектирование высоконагруженных приложений

## 1. Тема и целевая аудитория

### Тема "Проектирование сервиса по типу Youtube"

### Функционал MVP:
- загрузка видео
- просмотр видео
- авторизация
- оценка видео (лайки)
- подписка на канал
- комментарии к видео
- Поиск видео/канала
- Рекомендации видео

### Целевая аудитория:
- [2.70 b аудитория в месяц](https://www.globalmediainsight.com/blog/youtube-users-statistics/)
- [95.5 m аудитория в Росси в месяц (по статистике с mediascope)](https://mediascope.net/data/#internet)
- [Пользователи по странам](https://www.statista.com/topics/2019/youtube/#topicOverview)

| Страна    |  Кол-во пользователей (миллинов)  |
  |-----------|:---------------------------------:|
| Индия     |                462                |
| США       |                239                |
| Бразилия  |                144                |
| Индонезия |                139                |
| Мексика   |               83.1                |
| Япония    |               78.6                |
| Пакистан  |               71.7                |
| Германия  |               67.8                |
| Вьетнам   |                63                 |
| Филиппины |               58.1                |
    
- 122 m пользователей в день
- средний возраст 25-34
- Среднее время нахождение на ютубе пользователем 19 минут

| Пол       | процентное соотношение |
|-----------|:----------------------:|
| Мужчины   |          54.4          |
| Женщины   |          45.6          |


### DAU по регионам:

- На основе mau рассчитаем dau для регионов, предполагая, что среднее соотношение между DAU/MAU составляет ~15% [[1](https://userpilot.com/blog/dau-mau-ratio/)]. 
- Возьмем из каждого региона самые активные страны.
```DAU_MAU = 15%```
#### Россия:
    MAU_RUSSIA = 95.5 млн
    DAU_RUSSIA = MAU_RUSSIA * DAU_MAU = 14.3 MAU
#### Южной Азии
    MAU_INDIA = 462 млн
    MAU_PAKISTAN = 71.7 млн
    DAU_SOUTH_ASIA = (MAU_INDIA + MAU_PAKISTAN) * DAU_MAU = 80 MAU
#### Северная Америка
    DAU_USA = 239 млн
    DAU_MEXICO = 83.1 млн
    DAU_NORTH_AMERICA = (DAU_USA + DAU_MEXICO) * DAU_MAU = 48.3 MAU
#### Южная Америка
    DAU_BRAZIL = 144 млн
    DAU_SOUTH_AMERICA = DAU_BRAZIL * DAU_MAU = 21.6 MAU
## 2. Расчет нагрузки

## 2.1 Продуктовые метрики

| Действие пользователя                 | Кол-во (млн) |
|---------------------------------------|:------------:|
| DAU_AUTH_RUS                          |     42.9     |
| DAU_AUTH_SOUTH_ASIA                   |     240      |
| DAU_AUTH_NORTH_AMERICA                |    144.9     |
| DAU_AUTH_SOUTH_AMERICA                |     64.8     |
| DAU_AUTH_WORLD                        |    492.6     |
| -                                     |      -       |
| MAU_VIEWS_RUSSIA                      |     336      |
| MAU_VIEWS_SOUTH_ASIA                  |     1420     |
| MAU_VIEWS_NORTH_AMERICA               |     374      |
| DAU_VIEWS_SOUTH_AMERICA               |     217      |
| -                                     |      -       |
| MAU_COMMENT_RUSSIA                    |    0.644     |
| MAU_COMMENT_SOUTH_ASIA                |    2.687     |
| MAU_COMMENT_NORTH_AMERICA             |    0.717     |
| MAU_COMMENT_SOUTH_AMERICA             |    0.520     |
| -                                     |      -       |
| MAU_LIKE_RUSSIA                       |    15.272    |
| MAU_LIKE_SOUTH_ASIA                   |    64.545    |
| MAU_LIKE_NORTH_AMERICA                |      17      |
| MAU_LIKE_SOUTH_AMERICA                |    9.863     |
| -                                     |      -       |
| MAU_SUBSCRIBES_RUSSIA                 |     16.8     |
| MAU_SUBSCRIBES_SOUTH_ASIA             |      71      |
| MAU_SUBSCRIBES_NORTH_AMERICA          |     18.7     |
| MAU_SUBSCRIBES_SOUTH_AMERICA          |    10.85     |
| -                                     |      -       |
| MAU_PAGE_SUB_RUSSIA                   |     3.36     |
| MAU_PAGE_SUB_SOUTH_ASIA               |     14.2     |
| MAU_PAGE_SUB_NORTH_AMERICA            |     3.74     |
| MAU_PAGE_SUB_SOUTH_AMERICA            |     2.71     |
| -                                     |      -       |
| MAU_PAGE_MAIN_RUSSIA                  |     3861     |
| MAU_PAGE_SUB_SOUTH_ASIA               |     2160     |
| MAU_PAGE_SUB_NORTH_AMERICA            |    13041     |
| MAU_PAGE_SUB_SOUTH_AMERICA            |     5832     |
| -                                     |      -       |
| DAU_SEARCH_RUS                        |    304.5     |
| DAU_SEARCH_SOUTH_ASIA                 |    1704.5    |
| DAU_SEARCH_NORTH_AMERICA              |    1030.2    |
| DAU_SEARCH_SOUTH_AMERICA              |    460.8     |
| -                                     |      -       |
| MAU_VIDEO_DAY_DOWNLOAD_RUS            |     9.6      |
| MAU_VIDEO_DAY_DOWNLOAD_SOUTH_ASIA     |    53.944    |
| MAU_VIDEO_DAY_DOWNLOAD_NORTH_AMERICA  |    32.898    |
| MAU_VIDEO_DAY_DOWNLOAD_SOUTH_AMERICA  |    14.621    |

### 2.1.1 Расчет продуктовых метрик
- DAU 14.3 миллионов
- MAU 95.5 миллионов

### Заходов на сервис (Авторизация) DAU_AUTH:
- Средняя мобильная сессия просмотра на YouTube составляет более 40 минут [[2](https://www.omnicoreagency.com/youtube-statistics/)], что указывает на то, что пользователи могут заходить на YouTube несколько раз в день, особенно на мобильных устройствах.
- Сделаем предположение, что это число равно 
```COUNT_AUTH_DAY = 3```,
```MONTH = 30```.
#### Россия:
- ```DAU_AUTH_RUS = DAU_RUSSIA * COUNT_AUTH_DAY = 42.9 млн```
- ```MAU_AUTH_RUS = DAU_AUTH_RUS * MONTH = 1.287 млрд```
#### Южная Азия:
- ```DAU_AUTH_SOUTH_ASIA = DAU_SOUTH_ASIA * COUNT_AUTH_DAY = 240 млн```
- ```MAU_AUTH_SOUTH_ASIA = DAU_AUTH_SOUTH_ASIA * MONTH = 7.2 млрд```
#### Северная Америка:
- ```DAU_AUTH_NORTH_AMERICA = DAU_NORTH_AMERICA * COUNT_AUTH_DAY = 144.9 млн```
- ```MAU_AUTH_NORTH_AMERICA = DAU_AUTH_NORTH_AMERICA * MONTH = 4.347 млрд```
#### Южная Америка:
- ```DAU_AUTH_SOUTH_AMERICA = DAU_SOUTH_AMERICA * COUNT_AUTH_DAY = 64.8 млн```
- ```MAU_AUTH_SOUTH_AMERICA = DAU_AUTH_SOUTH_AMERICA * MONTH = 1.944 млрд```

### Просмотров видео DAU_VIEWS:
- Опираясь на статистику [[3](https://www.globalmediainsight.com/blog/youtube-users-statistics/)]
#### Россия:
    MAU_VIEWS_RUSSIA = 336 млн
#### Южная Азия
    MAU_INDIA = 1.420 млн
    MAU_PAKISTAN ~ (слишком мала)
    MAU_VIEWS_SOUTH_ASIA = MAU_INDIA  = 1.420 млн
#### Северная Америка
    MAU_USA = 336 млн
    MAU_MEXICO ~ (слишком мала)
    MAU_CANADA = 38 млн
    MAU_VIEWS_NORTH_AMERICA = MAU_USA + MAU_CANADA = 374 млн
#### Южная Америка
    MAU_BRAZIL = 217 млн
    MAU_SOUTH_AMERICA = DAU_BRAZIL = 217 млн

### Комментарии:
- Т.к. я не нашёл точной статистики, возьму несколько случайных роликов на ютубе и посчитаю на какое кол-во просмотров приходится 1 комментарий:
 ``` -    VIEWS/COMMENT = 156_000 / 1_120 = 139
 VIEWS/COMMENT = 140_445_492 / 172_281 = 821
 VIEWS/COMMENT = (5_914_354/ 14_390) = 411
 VIEWS/COMMENT = (1.5 млн / 1_190) = 1260
 VIEWS/COMMENT = (2.2 млн / 8_755) = 251
 VIEWS/COMMENT = (2.2 млн / 8_755) = 428
 VIEWS/COMMENT = (2.2 млн / 3617) = 580
 VIEWS/COMMENT = (27 млн / 96945) = 278
  ```
  Приблизительно, получаем на 521 просмотр 1 комментарий
  ```COMMENT_ON_VIEWS = 521 ```
#### Россия:
    MAU_COMMENT_RUSSIA = MAU_VIEWS_RUSSIA / COMMENT_ON_VIEWS = 644_913
#### Южная Азия
    MAU_COMMENT_SOUTH_ASIA = MAU_VIEWS_SOUTH_ASIA / COMMENT_ON_VIEWS = 2_687_140 
#### Северная Америка
    MAU_COMMENT_NORTH_AMERICA = MAU_VIEWS_NORTH_AMERICA / COMMENT_ON_VIEWS = 717_850
#### Южная Америка
    MAU_COMMENT_SOUTH_AMERICA = MAU_VIEWS_SOUTH_AMERICA / COMMENT_ON_VIEWS = 520_153
### Лайки:
- Проделаю аналогичные вычисления как в пункте с комментариями (ролики аналогичные):
 ``` -    VIEWS/COMMENT = 156_000 / 1_120 = 139
 VIEWS/LIKE = 140_445_492 / 172_281 = 30
 VIEWS/LIKE = (5_914_354/ 574_000) = 10
 VIEWS/LIKE = (1.5 млн / 1_190) = 21
 VIEWS/LIKE = (2.2 млн / 69_000) = 31
 VIEWS/LIKE = (2.1 млн / 89_000) = 23
 VIEWS/LIKE = (27 млн / 96945) = 27
 VIEWS/LIKE = (1.2 млн / 74000) = 16

  ```
Приблизительно, получаем на 22 просмотра 1 лайк
```LIKE_ON_VIEWS = 521 ```
#### Россия:
    MAU_LIKE_RUSSIA = MAU_VIEWS_RUSSIA / LIKE_ON_VIEWS = 15_272_727
#### Южная Азия
    MAU_LIKE_SOUTH_ASIA = MAU_VIEWS_SOUTH_ASIA / LIKE_ON_VIEWS = 64_545_454
#### Северная Америка
    MAU_LIKE_NORTH_AMERICA = MAU_VIEWS_NORTH_AMERICA / LIKE_ON_VIEWS = 17 млн
#### Южная Америка
    MAU_LIKE_SOUTH_AMERICA = MAU_VIEWS_SOUTH_AMERICA / LIKE_ON_VIEWS = 9_863_636

### Подписки:
- Предположим, что на каждые 100 просмотров приходится 5 подписок => 5% зрителей остаются на канале.
```SUBSCRIBE_IN_VIEWS = 5%```
#### Россия:
    MAU_SUBSCRIBES_RUSSIA = MAU_VIEWS_RUSSIA * SUBSCRIBE_IN_VIEWS = 16.8 млн
#### Южная Азия
    MAU_SUBSCRIBES_SOUTH_ASIA = MAU_VIEWS_SOUTH_ASIA * SUBSCRIBE_IN_VIEWS = 71 млн
#### Северная Америка
    MAU_SUBSCRIBES_NORTH_AMERICA = MAU_VIEWS_NORTH_AMERICA * SUBSCRIBE_IN_VIEWS = 18.7 млн
#### Южная Америка
    MAU_SUBSCRIBES_SOUTH_AMERICA = MAU_VIEWS_SOUTH_AMERICA * SUBSCRIBE_IN_VIEWS = 10.85 млн

### Лента подписок:
- Предположим, что на 100 просмотров приходится 1 просмотр ленты подписок
```PAGE_SUB = 1%```
#### Россия:
    MAU_PAGE_SUB_RUSSIA = MAU_VIEWS_RUSSIA * PAGE_SUB = 3.36 млн
#### Южная Азия
    MAU_PAGE_SUB_SOUTH_ASIA = MAU_VIEWS_SOUTH_ASIA * PAGE_SUB = 14.2 млн
#### Северная Америка
    MAU_PAGE_SUB_NORTH_AMERICA = MAU_VIEWS_NORTH_AMERICA * PAGE_SUB = 3.74 млн
#### Южная Америка
    MAU_PAGE_SUB_SOUTH_AMERICA = MAU_VIEWS_SOUTH_AMERICA * PAGE_SUB = 2.71 млн

### Главная страница (Рекомендации):
- При открытии приложение/сайта пользователь всегда попадает на главную страницу, не считая случаев, когда он переходит по ссылке на видео, поэтому можно предположить, что каждый заход на сайт это загрузка страницы рекомендаций:
- Предположим, что пользователь заходит на сайт 3 раза в день:
```DAY_ACTIV = 3```
#### Россия:
    MAU_PAGE_MAIN_RUSSIA = DAU_AUTH_RUS * DAY_ACTIV * MOUNTH = 3.861 млрд
#### Южная Азия
    MAU_PAGE_MAIN_SOUTH_ASIA = DAU_AUTH_SOUTH_ASIA  * DAY_ACTIV * MOUNTH = 21.6 млрд
#### Северная Америка
    MAU_PAGE_MAIN_NORTH_AMERICA = DAU_AUTH_NORTH_AMERICA * DAY_ACTIV * MOUNTH = 13.041 млрд
#### Южная Америка
    MAU_PAGE_MAIN_SOUTH_AMERICA = DAU_AUTH_SOUTH_AMERICA * DAY_ACTIV * MOUNTH = 5.832 млрд

### Поиск:
- В день происходит 3.5 млрд запросов [[4](https://www.globalmediainsight.com/blog/youtube-users-statistics/#searchstat:~:text=Every%20day%2C%20more%20than%203.5%20billion%20searches%20are%20made%20on%20YouTube.)] ```DAU_SEARCH_WORLD = 1.5 млрд```
- Разобьём эти запросы по регионам:
- Рассчитаю суммарное DAU 
- ```DAU_TOTAL = DAU_RUSSIA + DAU_SOUTH_ASIA + DAU_NORTH_AMERICA + DAU_SOUTH_AMERICA = 164.2 млн```
- Доля DAU:
``` 
Россия: (14.3 млн / 164.2 млн) * 100% = 8.7%
Южная Азия: (80 млн / 164.2 млн) * 100% = 48.7%
Северная Америка: (48.3 млн / 164.2 млн) * 100% = 29.4%
Южная Америка: (21.6 млн / 164.2 млн) * 100% = 13.2%
```
- Рассчитаю DAU_SEARCH для конкретных регионов:
```
DAU_SEARCH_RUS =  8.7% * DAU_SEARCH_WORLD = 304.5 млн
DAU_SEARCH_SOUTH_ASIA = 48.7% * DAU_SEARCH_WORLD = 1704.5 млн
DAU_SEARCH_NORTH_AMERICA = 29.4% * DAU_SEARCH_WORLD = 1030.2 млн
DAU_SEARCH_SOUTH_AMERICA = 13.2% * DAU_SEARCH_WORLD = 460.8 млн
 ```
### Загрузка видео:
- Каждый день на ютуб загружается 720_000 часов видео [[5](https://bloggingwizard.com/youtube-statistics/#:~:text=That%E2%80%99s%2030%2C000%20hours%20of%20video%20uploaded%20every%20hour%2C%20720%2C000%20uploaded%20every%20day%2C%205.04%20million%20uploaded%20every%20week%2C%2021.9%20million%20uploaded%20every%20month%20and%20262.8%20million%20uploaded%20every%20year.)] ```TOTAL_TIME_DAY_HOURS = 720_000 часов```
- Зная, что ролик в среднем длится 11.7 минут [[6](https://www.globalmediainsight.com/blog/youtube-users-statistics/#:~:text=According%20to%20Statista%2C%20the%20average%20length%20of%20a%20YouTube%20video%20is%20around%2011.7%20minutes.)] ```AVERAGE_TIME_VIDEO = 11.7 минут```
- Кол-во роликов ```COUNT_VIDEO_DAY_WORLD = TOTAL_TIME_DAY_HOURS * 60 / AVERAGE_TIME_VIDEO = 3_692_307```
- Рассчитаю для регионов основывась на полученных данных по процентному соотношению dau:
```
DAU_VIDEO_DAY_DOWNLOAD_RUS = COUNT_VIDEO_DAY_WORLD * 8.7% = 321_230
MAU_VIDEO_DAY_DOWNLOAD_RUS = DAU_VIDEO_DAY_DOWNLOAD_RUS * MOUNTH = 9_636_900

DAU_VIDEO_DAY_DOWNLOAD_SOUTH_ASIA = COUNT_VIDEO_DAY_WORLD * 48.7% = 1_798_153
MAU_VIDEO_DAY_DOWNLOAD_SOUTH_ASIA = DAU_VIDEO_DAY_DOWNLOAD_SOUTH_ASIA * MOUNTH = 53_944_590

DAU_VIDEO_DAY_DOWNLOAD_NORTH_AMERICA = COUNT_VIDEO_DAY_WORLD * 29.4% = 1_096_615
MAU_VIDEO_DAY_DOWNLOAD_NORTH_AMERICA = COUNT_VIDEO_DAY_WORLD * MOUNTH = 32_898_450

DAU_VIDEO_DAY_DOWNLOAD_SOUTH_AMERICA = COUNT_VIDEO_DAY_WORLD * 13.2% = 487_384
MAU_VIDEO_DAY_DOWNLOAD_SOUTH_AMERICA = DAU_VIDEO_DAY_DOWNLOAD_SOUTH_AMERICA* MOUNTH =  14_621_520
```
