# Проектирование высоконагруженных приложений

## Содержание
* ### [Тема и целевая аудитория](#1-тема-и-целевая-аудитория)
* ### [Расчет нагрузки](#2-расчет-нагрузки)
* ### [Глобальная балансировка нагрузки](#3-глобальная-балансировка-нагрузки)
* ### [Локальная балансировка нагрузки](#4-локальная-балансировка-нагрузки)
* ### [Логическая схема БД](#5-логическая-схема-бд)
* ### [Физическая схема БД](#6-физическая-схема-бд)
* ### [Алгоритмы](#7-алгоритмы)
* ### [Технологии](#8-технологии)
* ### [Схема проекта](#9-схема-проекта)
* ### [Обеспечение надёжности](#10-обеспечение-надёжности)
* ### [Расчет ресурсов](#11-расчет-ресурсов)
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
    
- средний возраст 25-34
- Средняя мобильная сессия просмотра на YouTube составляет более 40 минут

| Пол       | процентное соотношение |
|-----------|:----------------------:|
| Мужчины   |          54.4          |
| Женщины   |          45.6          |


### DAU по регионам:

- На основе mau рассчитаем dau для регионов, предполагая, что среднее соотношение между DAU/MAU составляет ~56% [[1](https://mediascope.net/data/#internet)]. 
- Возьмем из каждого региона самые активные страны.
```DAU_MAU = 56%```
#### Россия:
    MAU_RUSSIA = 95.5 млн
    DAU_RUSSIA = MAU_RUSSIA * DAU_MAU = 54.2 MAU
#### Южная Азия
    MAU_INDIA = 462 млн
    MAU_PAKISTAN = 71.7 млн
    DAU_SOUTH_ASIA = (MAU_INDIA + MAU_PAKISTAN) * DAU_MAU = 298.8 MAU
#### Северная Америка
    DAU_USA = 239 млн
    DAU_MEXICO = 83.1 млн
    DAU_NORTH_AMERICA = (DAU_USA + DAU_MEXICO) * DAU_MAU = 180.4 MAU
#### Южная Америка
    DAU_BRAZIL = 144 млн
    DAU_SOUTH_AMERICA = DAU_BRAZIL * DAU_MAU = 80.6 MAU
## 2. Расчет нагрузки

## 2.1 Продуктовые метрики

| Действие пользователя                | Кол-во (млн) |
|--------------------------------------|:------------:|
| DAU_AUTH_RUS                         |     54.2     |
| DAU_AUTH_SOUTH_ASIA                  |    298.8     |
| DAU_AUTH_NORTH_AMERICA               |    180.4     |
| DAU_AUTH_SOUTH_AMERICA               |     80.6     |
| DAU_AUTH_WORLD                       |     614      |
| -                                    |      -       |
| MAU_VIEWS_RUSSIA                     |     336      |
| MAU_VIEWS_SOUTH_ASIA                 |     1420     |
| MAU_VIEWS_NORTH_AMERICA              |     374      |
| MAU_VIEWS_SOUTH_AMERICA              |     217      |
| -                                    |      -       |
| MAU_COMMENT_RUSSIA                   |    0.644     |
| MAU_COMMENT_SOUTH_ASIA               |    2.687     |
| MAU_COMMENT_NORTH_AMERICA            |    0.717     |
| MAU_COMMENT_SOUTH_AMERICA            |    0.520     |
| -                                    |      -       |
| MAU_LIKE_RUSSIA                      |    15.272    |
| MAU_LIKE_SOUTH_ASIA                  |    64.545    |
| MAU_LIKE_NORTH_AMERICA               |      17      |
| MAU_LIKE_SOUTH_AMERICA               |    9.863     |
| -                                    |      -       |
| MAU_SUBSCRIBES_RUSSIA                |     16.8     |
| MAU_SUBSCRIBES_SOUTH_ASIA            |      71      |
| MAU_SUBSCRIBES_NORTH_AMERICA         |     18.7     |
| MAU_SUBSCRIBES_SOUTH_AMERICA         |    10.85     |
| -                                    |      -       |
| MAU_PAGE_SUB_RUSSIA                  |     3.36     |
| MAU_PAGE_SUB_SOUTH_ASIA              |     14.2     |
| MAU_PAGE_SUB_NORTH_AMERICA           |     3.74     |
| MAU_PAGE_SUB_SOUTH_AMERICA           |     2.71     |
| -                                    |      -       |
| MAU_PAGE_MAIN_RUSSIA                 |     3861     |
| MAU_PAGE_MAIN_SOUTH_ASIA             |    21600     |
| MAU_PAGE_MAIN_NORTH_AMERICA          |    13041     |
| MAU_PAGE_MAIN_SOUTH_AMERICA          |     5832     |
| -                                    |      -       |
| DAU_SEARCH_RUS                       |    304.5     |
| DAU_SEARCH_SOUTH_ASIA                |    1704.5    |
| DAU_SEARCH_NORTH_AMERICA             |    1030.2    |
| DAU_SEARCH_SOUTH_AMERICA             |    460.8     |
| -                                    |      -       |
| MAU_VIDEO_DAY_DOWNLOAD_RUS           |     9.6      |
| MAU_VIDEO_DAY_DOWNLOAD_SOUTH_ASIA    |    53.944    |
| MAU_VIDEO_DAY_DOWNLOAD_NORTH_AMERICA |    32.898    |
| MAU_VIDEO_DAY_DOWNLOAD_SOUTH_AMERICA |    14.621    |

### 2.1.1 Расчет продуктовых метрик

### Заходов на сервис (Авторизация):
- Средняя мобильная сессия просмотра на YouTube составляет более 40 минут [[2](https://www.omnicoreagency.com/youtube-statistics/)], что указывает на то, что пользователи могут заходить на YouTube несколько раз в день, особенно на мобильных устройствах.
- Сделаем предположение, что это число равно 
```COUNT_AUTH_DAY = 0.5```,
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
```LIKE_ON_VIEWS = 22 ```
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
- Предположим, что пользователь заходит на сайт 4 раза в день:
```DAY_ACTIV = 4```
#### Россия:
    MAU_PAGE_MAIN_RUSSIA = DAU_RUSSIA * DAY_ACTIV * MOUNTH = 6.504 млрд
#### Южная Азия
    MAU_PAGE_MAIN_SOUTH_ASIA = DAU_SOUTH_ASIA   * DAY_ACTIV * MOUNTH = 35.8 млрд
#### Северная Америка
    MAU_PAGE_MAIN_NORTH_AMERICA = DAU_NORTH_AMERICA * DAY_ACTIV * MOUNTH = 21.648 млрд
#### Южная Америка
    MAU_PAGE_MAIN_SOUTH_AMERICA = DAU_SOUTH_AMERICA * DAY_ACTIV * MOUNTH = 9.672 млрд

### Поиск:
- В день происходит 3.5 млрд запросов [[4](https://www.globalmediainsight.com/blog/youtube-users-statistics/#searchstat:~:text=Every%20day%2C%20more%20than%203.5%20billion%20searches%20are%20made%20on%20YouTube.)] ```DAU_SEARCH_WORLD = 3.5 млрд```
- Разобьём эти запросы по регионам:
- Рассчитаю суммарное DAU 
- ```DAU_TOTAL = DAU_RUSSIA + DAU_SOUTH_ASIA + DAU_NORTH_AMERICA + DAU_SOUTH_AMERICA = 614 млн```
- Доля DAU:
``` 
Россия: (DAU_RUSSIA / DAU_TOTAL) * 100% = 8.7%
Южная Азия: (DAU_SOUTH_ASIA / DAU_TOTAL) * 100% = 48.7%
Северная Америка: (DAU_NORTH_AMERICA / DAU_TOTAL) * 100% = 29.4%
Южная Америка: (DAU_SOUTH_AMERICA / DAU_TOTAL) * 100% = 13.2%
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
- Рассчитаю для регионов основываясь на полученных данных по процентному соотношению dau:
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

## 2.2 Технические метрики

### 2.2.1 Сводные таблицы технических метрик

| Хранение данных                      | Увелечение ежегодно ПБ |
|--------------------------------------|:----------------------:|
| Видео (Россия)                       |         +725.4         |
| Видео (Южная Азия)                   |         +4056          |
| Видео (Северная Америка)             |         +2474          |
| Видео (Южная Америка)                |         +1099          |
| Пользователь (Россия)                |           2            |      
| Пользователь (Южная Азия)            |           2            |      
| Пользователь (Северная Америка)      |           7            |      
| Пользователь (Южная Америка)         |           3            |      

### 2.2.2 Расчет технических метрик
#### 2.2.2.1 Хранилище

**Видео**:
- Сейчас большинство роликов на ютубе имеют разрешение 1080 p 
-  Будем хранить все варианты разрешений [[7](https://support.google.com/youtube/answer/1722171?hl=en#zippy=%2Cframe-rate%2Cbitrate%2Cvideo-codec-h%2Cvideo-resolution-and-aspect-ratio%2Ccolor-space)]
```
  2160p (4K) - 40 Mbps
  1440p (2K) - 16 Mbps
  1080p - 8 Mbps
  720p - 5 Mbps
  480p - 2.5 Mbps
  360p - 1 Mbps
  
```
- Также ютуб позволяет добавлять превью к видео размером до 2 мб.
- Используя продуктовые данные, рассчитаю статистику по регионам:
```
Россия:
Кол-во загрузок в день: (DAU_VIDEO_DAY_DOWNLOAD_RUS * AVERAGE_TIME_VIDEO * SECOND) * (8 + 5 + 2.5 + 1 + 2) / 8 / 1024 = 509.254 тыс. Гб
Кол-во в год: 509.254 * 365 = 186 ПБ память для хранение роликов. Также нужно хранить резервную копию, будем хранить 2 копии: 558 пб
Также некоторые пользователи загружают ролики в качесте 4k. Т.к. их кол-во намного меньше чем 1080, то увеличим размер хранилище на 30%
Итоговый результат 725.4 ПБ
```
```
Южная Азия
Кол-во загрузок в день: ((DAU_VIDEO_DAY_DOWNLOAD_SOUTH_ASIA * AVERAGE_TIME_VIDEO * SECOND) * (8 + 5 + 2.5 + 1 + 2) / 8 / 1024) = 2 850 661 ГБ
Кол-во загрузок в год: 4056 ПБ
```
```
Северная Америка:
Кол-во загрузок в день: ((DAU_VIDEO_DAY_DOWNLOAD_NORTH_AMERICA * AVERAGE_TIME_VIDEO * SECOND) * (8 + 5 + 2.5 + 1 + 2) / 8 / 1024) = 1 738 493 ГБ
Кол-во загрузок в год: 2474 ПБ
```
```
Южная Америка
Кол-во загрузок в день: ((DAU_VIDEO_DAY_DOWNLOAD_SOUTH_AMERIC * AVERAGE_TIME_VIDEO * SECOND) * (8 + 5 + 2.5 + 1 + 2) / 8 / 1024) = 772 663 ГБ
Кол-во загрузок в год: 1099 ПБ
```

**Данные пользователя**:

- Данные пользователя:
  
  - Фото профиля 4 МБ
  - Баннер канала 6 МБ
  - Логотип канала 1 МБ
```
На одного пользователя в России: MAU_RUSSIA * (4 + 6 + 1) МБ = 1 ПБ
На одного пользователя в Южной Азии: MAU_SOUTH_ASIA * (4 + 6 + 1) МБ = 5.8 ПБ
На одного пользователя в Северной Америке: MAU_NORTH_AMERICA * (4 + 6 + 1) МБ = 3.5 ПБ
На одного пользователя в Южной Америке: MAU_SOUTH_AMERICA * (4 + 6 + 1) МБ = 1.584 ПБ
Все полученные данные умножу на 2, т.к. нужна одна копия на аккаунт
```
- Т.к. пользователи выставляют эти данные один раз, и при замене, старые будут удалятся, то можно считать, что в будущем эти данные не будут сильно расти.

#### 2.2.2.2 Потребление трафика
- Сетевой трафик:
  - Суммарный суточный:
    
    - Загрузка видео:
      - ```
          В России кол-во загрузок в день: 509.254 гигабайт
          В Южной Азии кол-во загрузок в день: 2 850 661 гигабайт
          В Северной Америке кол-во загрузок в день: 1 738 493 ГБ
          В Южной Америке кол-во загрузок в день: 772 663 ГБ
        ```
    - Отдача видео
    - Предположим, что в среднем люди смотрят ролик в 1080p, тогда ```AVERAGE_BITRATE = 8```
      - ```
            В России кол-во загрузок за день: MAU_VIEWS_RUSSIA * AVERAGE_BITRATE * AVERAGE_TIME_VIDEO * SECOND / 8 / 1024 = 230_343_750 / DAYS = 7_678_125 ГБ.
            В Южной Азии кол-во загрузок: MAU_VIEWS_SOUTH_ASIA * AVERAGE_BITRATE * AVERAGE_TIME_VIDEO * SECOND / 8 / 1024 = 32_449_218 ГБ.
            В Северной Америке кол-во загрузок: MAU_VIEWS_NORTH_AMERICA * AVERAGE_BITRATE / 8 / 1024 = 8_546_484 ГБ.
            В Южной Америке кол-во загрузок: MAU_VIEWS_SOUTH_AMERICA * AVERAGE_BITRATE / 8 / 1024 = 4_958_789 ГБ
        ```
    - Страницы (основная, видео, подписки):
      
      - html, css, js - 2 Мб
      - Превью 2 Мб
      - ```
        Россия в день: (MAU_PAGE_SUB_RUSSIA + MAU_VIEWS_RUSSIA + MAU_PAGE_MAIN_RUSSIA) * 4 МБ / 8 / 1024 = 173_906 / 30 = 68_365 ГБ
        Южная азия в день: ((MAU_PAGE_SUB_SOUTH_ASIA + MAU_VIEWS_SOUTH_ASIA + MAU_PAGE_MAIN_SOUTH_ASIA) * 4 МБ / 8 / 1024) / 30 = 374_905 ГБ
        Северная Америка в день: (MAU_PAGE_SUB_NORTH_AMERICA + MAU_VIEWS_NORTH_AMERICA + MAU_PAGE_MAIN_NORTH_AMERICA) * 4 МБ / 8 / 1024 / 30 = 218_403 ГБ
        Южная Америка в день: (MAU_PAGE_SUB_SOUTH_AMERICA + MAU_VIEWS_SOUTH_AMERICA + MAU_PAGE_MAIN_SOUTH_AMERICA) * 4 МБ / 8 / 1024 / 30 = 98_497 ГБ
        ```
  - Пиковое использование трафика:
  - Возьмем пиковые значение, как x2 от средней за день
    - Загрузка видео:
      - ```
          Россия в секунду: 509.254 * 2 / 86400 = 11.7 ГБ/c
          Южная азия в секунду: 65.9 ГБ/c
          Северная Америка в секунду: 40.2 ГБ/c
          Южная Америка в секунду: 17 ГБ/с
        ```
    - Отдача видео:
      - ```
         Россия в секунду: 7_678_125 * 2 / 86400 = 177.7 ГБ/c
         Южная азия в секунду: 751.1 ГБ/c
         Северная Америка в секунду: 197.8 ГБ/c
         Южная Америка в секунду: 114.8 ГБ/c
        ```
    - Страницы (основная, видео, подписки):
      
      - ```
         Россия в секунду: 68_365 * 2 / 86400 = 1.6 ГБ/c
         Южная азия в секунду: 8.7 ГБ/c
         Северная Америка в секунду: 5 ГБ/c
         Южная Америка в секунду: 2.3 ГБ/c
        ``` 

#### 2.2.2.3 RPS по типам запросов
| Тип запроса                                          | RPS                                                 |
|------------------------------------------------------|-----------------------------------------------------|
| Добавление комментария(Россия)                       | `MAU_COMMENT_RUSSIA / 30 / 86400  = 1`              |
| Добавление комментария(Южная Азия)                   | `MAU_COMMENT_SOUTH_ASIA / 30 / 86400  = 1`          |
| Добавление комментария(Северная Америка)             | `MAU_COMMENT_NORTH_AMERICA / 30 / 86400  = 1`       |
| Добавление комментария(Южная Америка)                | `MAU_COMMENT_SOUTH_AMERICA  / 30 / 86400 = 1`       |
| Загрузка видеоролика на канал(Россия)                | `DAU_VIDEO_DAY_DOWNLOAD_RUS / 86400 = 4`            |
| Загрузка видеоролика на канал(Южная Азия)            | `DAU_VIDEO_DAY_DOWNLOAD_SOUTH_ASIA / 86400 = 21`    |
| Загрузка видеоролика на канал(Северная Америка)      | `DAU_VIDEO_DAY_DOWNLOAD_NORTH_AMERICA / 86400 = 13` |
| Загрузка видеоролика на канал(Южная Америка)         | `DAU_VIDEO_DAY_DOWNLOAD_SOUTH_AMERICA / 86400 = 6`  |
| Поиск(Россия)                                        | `DAU_SEARCH_RUS / 86400  = 3.5 тыс`                 |
| Поиск(Южная Азия)                                    | `DAU_SEARCH_SOUTH_ASIA / 86400  = 19.7 тыс`         |
| Поиск(Северная Америка)                              | `DAU_SEARCH_NORTH_AMERICA / 86400  = 11.9 тыс`      |
| Поиск(Южная Америка)                                 | `DAU_SEARCH_SOUTH_AMERICA / 86400 = 5.3 тыс`        |
| Добавление лайка(Россия)                             | `MAU_LIKE_RUSSIA / 30 / 86400 = 6`                  |
| Добавление лайка(Южная Азия)                         | `MAU_LIKE_SOUTH_ASIA / 30 / 86400 = 24`             |
| Добавление лайка(Северная Америка)                   | `MAU_LIKE_NORTH_AMERICA / 30 / 86400 = 7`           |
| Добавление лайка(Южная Америка)                      | `MAU_LIKE_SOUTH_AMERICA / 30 / 86400 = 3`           |
| Проверка авторизации(Россия)                         | `DAU_AUTH_RUS / 86400  = 496`                       |
| Проверка авторизации(Южная Азия)                     | `DAU_AUTH_SOUTH_ASIA / 86400  = 2.7 тыс`            |
| Проверка авторизации(Северная Америка)               | `DAU_AUTH_NORTH_AMERICA / 86400 = 1.6 тыс`          |
| Проверка авторизации(Южная Америка)                  | `DAU_AUTH_SOUTH_AMERICA / 86400 = 750`              |
| Подписка на канал(Россия)                            | `MAU_SUBSCRIBES_RUSSIA / 30 / 86400 = 6`            |
| Подписка на канал(Южная Азия)                        | `MAU_SUBSCRIBES_SOUTH_ASIA / 30 / 86400 = 27`       |
| Подписка на канал(Северная Америка)                  | `MAU_SUBSCRIBES_NORTH_AMERICA / 30 / 86400 = 7`     |
| Подписка на канал(Южная Америка)                     | `MAU_SUBSCRIBES_SOUTH_AMERICA / 30 / 86400 = 4`     |

## 3. Глобальная балансировка нагрузки
### Нахождение ЦОДов
- Установим 25 датацентров аналогично Google "https://www.google.com/about/datacenters/locations/", уберём пару датацентров из Америке и Тайваня.
### DNS балансировка
- Будет использовать Latency-based DNS (AMAZON S53), в результате чего пользователю будет подбираться ближайший дата-центр, будем использовать эту технологию для глобальной балансировки.
- Будем использовать механизмы Failover, предоставляемые DNS-провайдерами (AWS Route 53, Cloudflare, NS1), которые будут проверять состояние датацентров и автоматически переключать трафик на другие датацентры в случае сбоя. Чтобы избежать перегрузки одного датацентра потоком пользователей, трафик будет равномерно распределяться между несколькими доступными датацентрами.
### BGP Anycast
- Внутри регионов будем использовать BGP Anycast (будем выдавать один ip для нескольких дата-центров, отправлять пользователя к ближайшему дата-центру, cdn серверу)
- Будем использовать CDN сервера для отдачи статики (видео, информации о пользователе). Для этого будем кэшировать ролики в дата-центрах и дальше рассылать по cdn серверам, также будем предоставлять провайдерам кэши для ускорения контента(ISP)[[8](https://youtu.be/g5v2-H-sabM?t=490)], чтобы снять нагрузку с cdn серверов.
- Кеш сервер будет работать так, отдавать пользователю контент и в моменты минимальной нагрузки загружать с cdn серверов новый контент.
- Кеш можно расположить по всему миру аналогично Google:
![gls7h9xb-thtnrspvjd4ghk7jfo.png](gls7h9xb-thtnrspvjd4ghk7jfo.png)
<!-- - Настроим BGP communities. BGP Communities предоставляют возможность реализовывать сложные сценарии маршрутизации, такие как политика маршрутизации, фильтрация и маршрутизация на основе атрибутов. Это позволяет более тонко настраивать маршрутизацию трафика, что может быть критически важно для обеспечения отказоустойчивости в сложных сетевых средах. С помощью BGP communities настроим схему схема перераспределения трафика, сделаем её равномерной при падении ДЦ. -->


## 4. Локальная балансировка нагрузки
### BGP/RIP балансировка
- В ДЦ будет стоять маршрутизаторы, с помощью BGP маршрутизации будет распределять данные на балансировщик L7.
- Возможные аналоги RIP и OSPF - более простые в настройке, но не такие гибкие как BGP.
- OSPF и RIP - имеют ограничения на масштабируемость.
- BGP позволяет определить, что определенные типы трафика должны направляться через определенный маршрут или группу маршрутов к балансировщику в отличии от RIP и OSPF.
### L7 балансировщик
- Будем использовать Envoy, выбираем его за продвинутую обсервабилити и возможности динамического переконфигурирования. Будет кешировать некоторые запросы, решать проблему "медленного клиента". Будет распределять запросы на сервисы.
- Выделим железа так, чтобы оно при пиковых нагрузках, было загружено максимум на 70%.
- В качесте re-try стратегий будем использовать jitter.
### SSL termination:
- Будем использовать SSL Termination, чтобы снять нагрузку с серверов по расшифровке ssl, это будет делать L7 балансировщик.
- Session cache - будет кешировать сессию.

## 5. Логическая схема БД

```mermaid
---
title: Схема бд
---
erDiagram
    history {
        user_id uuid PK
        video_id uuid PK
        second int
        created date
    }
    like {
        user_id uuid PK
        video_id uuid PK
        grade bool
        created date
        update date
    }
    video {
        id uuid PK
        title text
        author uuid FK
        tags array
        description text
        preview_url text
        created date
        update date
    }
    session {
        user_id uuid
        cookie string
    }
    user {
        id uuid PK
        username text
        login text
        email text
        password_hash string
        gender text
        country text
        avatar_url text
        birthday date
        created date
        update date
    }
    video_statistic {
        id uuid PK
        video_id uuid FK
        comments_count int
        view_count int
        like_count int
        dislike_count int
        video_quality_count int
        created date
        update date
    }
    video_quality {
        video_id uuid PK
        video_url_360p chunks
        video_url_480p chunks
        video_url_720p chunks
        video_url_1080p chunks
        video_url_1440p chunks
        video_url_2160p chunks
        created date
        update date
    }
    comment_like {
        user_id uuid FK
        comment_id uuid FK
        grade bool
        created date
    }
    comment {
        id uuid FK
        video_id uuid PK
        user_id uuid PK
        body text
        parent uuid
        created date
        update date
    }
    subscribe {
        author_id uuid PK
        subsciber_id uuid
        created date
    }
    view {
        user_id uuid PK
        video_id uuid PK
        created_at date
    }
    user ||--o{ history : has
    video ||--o{ history : has
    user ||--o{ comment_like : like
    comment ||--o{ comment_like : like
    user ||--o{ video : creates
    video ||--o{ comment : has
    user ||--o{ comment : comment
    user ||--o{ like : likes
    video ||--o{ like : likes
    user ||--o{ subscribe : subscribes
    user ||--o{ view : views
    user ||--o{ session : has
    video ||--|| video_statistic : has
    video ||--|| video_quality : has
    video ||--o{ view : has
```

### Размер данных:
Возьму кол-во пользователей сумма mau за подсчитанные регионы MAU_WORLD = 940.5 млн * 2 = 1881 млн (чтобы учесть расширение бд) = 1881
#### User:
```
16 (UUID) + 50 (username) + 50 (login) + 50 (email) + 60 (password_hash) + 8 (gender) + 50 (country) + 50 (avatar_url) + 8 (birthday) + 8 (created) + 8 (updated) = 352 байта * 1881млн = 616,6 Гб
```
#### Session:
```
16 (UUID) + 60 (cookie) = 76 байта * 1881 млн = 133,13 Гб
```
#### Subscribe:
```
16 (UUID) + 16 (UUID) + 8 (created) = 39 байта * 1881млн = 68,3 ГБ * 50 (среднее кол-во подписок на каналы) = 3415 Гб
```
#### Video (дополнительная информация):
Сами ролиики буду хранить в s3 рассчет был проведен выше. Возьму за кол-во роликов, цифру рассчитанную выше COUNT_VIDEO_DAY_WORLD = 3_692_307
```
16 (UUID) + 50 (title) + 16 (UUID) + 500 (description) + 60 (preview_url) + 8 (created) + 8 (updated) = 658 байт * COUNT_VIDEO_DAY_WORLD = 2,3 ГБ * 30 = 69 ГБ (в месяц)
```

#### Comment:
SUM_COMMENT = 4 570 056 (месяц)
```
16 (UUID) + 16 (UUID) + 500 (body) + 8 (created) + 8 (update) = 548 байт * SUM_COMMENT = 2.3 ГБ (в месяц)
```
#### Like
SUM_LIKE = 106_681_817 (месяц)
```
16 (UUID) + 16 (UUID) + 1 (bool) + 8 (created) + 8 (updated) = 49 байт * SUM_LIKE =  4,8 ГБ (в месяц)
```
#### Video_statistic:
```
16 (UUID) + 16 (UUID) + 20 (суммарно int) + 8 (created) + 8 (updated) = 68 * COUNT_VIDEO_DAY_WORLD = 251_076_876 * 30 = 7.015 ГБ (в месяц)
```
#### View:
MAU_VIEWS = 2347 млн
```
16 (UUID) + 16 (UUID) = 32 * 2347млн = 70 ГБ (в месяц)
```

## 6. Физическая схема БД


### Выбор хранилища данных:
- Для сессий использую redis(userid, cookie string), т.к. хранилище in-memory и не сильно важно целостность данных, небольшая нагрузка `session`.
- Для видео использую MySQL Vitess, который позволяет из коробки горизонтально масштабироваться `video`, `video_quality`, `video_statistic`, `comment`, `like`, `view`, `subscibe`, `user`.
![alt text](<0 dGqO-_hGaOC5KIqv.webp>)
Ключевыми формирующими аспектами Vitess являются VTTablet и VTGate.
- **VTGate** используется как легкий прокси-сервер, который направляет трафик к соответствующим серверам VTTablet и возвращает консолидированные результаты обратно клиенту. При маршрутизации запросов к соответствующим серверам VTTablet, VTGate учитывает схему шардирования, требуемую задержку и доступность таблиц и их базовых экземпляров MySQL
- **VTTablet**, отвечает за мониторинг локального MySQL и обеспечение его оптимального состояния. Для достижения этой цели VTTablet устраняет ошибочные запросы, применяет ограничения и выполняет периодические проверки работоспособности. 
- Управление репликацией: VTTablet может выполнять команды для управления репликацией, включая инициализацию мастера шарда, планирование переключения родителей, аварийное переключение родителей и переключение таблицы.
- С помощью подхода описанного выше достигается горизонтальное масштабирование.
- Видео файлы и аватарки буду хранить в [Google file system](https://habr.com/ru/articles/73673/) (либо похожие аналоги), в таблице video_quality буду хранить чанки с (размер чанка, имя файла и смещение относительно начала файла). GFS будет отвечать за сохранность данных за счет быстрых слепков, сохранение метаданных на master, и большого кол-во реплик.

### Индексы
- В таблице video:
    - (video_id, created) B-tree
    - (title) GIN  (для поиска)
    - (description) GIN  (для поиска)
- В таблице comments:
    - (video_id, update) B-tree
- В таблице subscibe:
    - (author_id, created) B-tree
- В таблице views:
    - (user_id, video_id) B-tree
- В таблице like:
    - (user_id, video_id) B-tree
- В таблице history:
    - (video_id, created) B-tree
### Денормализация
- Объединю в одну таблицу video + video_quality + video_statistics для шардинга избавлюсь от JOIN
- Объединю для user буду хранить его подписчиков и каналы на которые он подписан для шардинга избавлюсь от JOIN
### Шардирование
- users по id
```mermaid
---
title: шардирование
---
erDiagram
    user {
        id uuid PK
        username text
        verification bool
        login text
        email text
        password_hash string
        gender text
        country text
        avatar_url text
        birthday date
        created date
        update date
    }
```
- subscribe по id

```mermaid
---
title: шардирование subscribe
---
    erDiagram
    subscribe {
        author_id uuid PK
        subsciber_id uuid
        created date
    }
```
- history по id
```mermaid
---
title: шардирование history
---
erDiagram
    history {
        id uuid PK
        user_id uuid FK
        video_id uuid FK
        second int
        created date
    }
```
- comment по id(comment)
```mermaid
---
title: шардирование comment
---
erDiagram
    comment {
        id uuid FK
        video_id uuid PK
        user_id uuid PK
        body text
        parent uuid
        created date
        update date
    }
    comment_like {
        user_id uuid FK
        comment_id uuid FK
        grade bool
        created date
    }
    comment ||--o{ comment_like : like
```
- реакция на ролик шардирование по video_id
```mermaid
---
title: Шардирование view и like
---
erDiagram
    like {
        user_id uuid PK
        video_id uuid PK
        grade bool
        created date
        update date
    }
    view {
        user_id uuid PK
        video_id uuid PK
    }
```

- video по id(video)
```mermaid
---
title: Шардирование video
---
erDiagram
    video {
        id uuid PK
        title text
        author_name text
        author_avatar text
        description text
        preview_url text
        comments_count int
        view_count int
        like_count int
        dislike_count int
        video_quality_count int
        video_url_360p chunks
        video_url_480p chunks
        video_url_720p chunks
        video_url_1080p chunks
        video_url_1440p chunks
        video_url_2160p chunks
        created date
        update date
    }
   ```
### Клиентские библиотеки / интеграции
- vitessdriver https://pkg.go.dev/vitess.io/vitess/go/vt/vitessdriver
- go-redis https://github.com/redis/go-redis

## 7. Алгоритмы

### Рекомендации:
Рекомендательная система будет основана на ALS **матричная факторизации**. Система будет работать онлайн, построение матрицы будет происходит оффлайн

1. Построим матрицу из скрытых параметров пользователя и матрицу из скрытых параметров роликов их произведение и будет оценкой пользователя для данного ролика, за оценку удобно будем брать число Kпросмотренных_секунд/Kдлительнось_секунд. Обучение модели будет состоять в том, чтобы минимизацировать квадратичную ошибку. Нужно учитывать, что ролики бывают очень популярные, пользователи предвзятыми и отражать это различными параметрами. Оптимальное решение будем искать с помощью градиентного спуска

2. При запросе на главную страницу будем подгружать обученную модель и выдавать для конкретного пользователя ролики. Нужно сразу убрать из этой выборки ролики, которые не подходят, т.к. были уже просмотренны, либо из-за детского фильтра, ограничение самого пользователя (черный лист). Для данной задачи будут применятся фильтор блума. Можно уменьшить работу модели используя кеш, в который будем сохранять ролики отданные моделью.

### Обработка видео
- При загрузке ролика будем хранить 6 разных форматов (2160p, 1440p, 1080p, 720p, 480p, 360p). 
- Ролики будут разбиваться на чанки, которые будут хранится в GFS (аналоги)
    - Возможность обрабатывать большие данные, т.к. этим будут заниматься реплики, а мастер будет лишь направлять на них.
- Будем хранить (размер чанка, имя файла и смещение относительно начала файла)
- Ролики будут кодироваться с помощью H.264

### Отправка видео
- Будем использовать технологию адаптивного битрейта. Перед отправкой видео клиенту приходит манифест файл, который описывает доступные сегменты потока и соответствующие им скорости передачи данных. На клиенте будет происходит анализ просмотра прошлых роликов, определение сети передачи, и за счет этого клиент с сервера будет динамически запрашивать нужный ему сегмент потока.
- Буду хранить в history кол-во просмотренных секунд на ролике, чтобы возвращать нужный фрагмент ролика.

## 8. Технологии

| Технология  | Применение  | Обоснование|
|-------------|--------------------------------|-|
| Go          | Backend | Многопоточность, простота, бинарные файлы без зависимостей|
| React          | Frontend | Решение вместе с redux, есть возможность хранить состояние на клиенте. Принятое рынком и программистами решение|
| Kafka          | Брокер сообщений | Обеспечивает сохранность сообщений, многократная повторная обработка. Ориентирован на потоковую обработку событий и обеспечивает оптимизированную обработку больших потоков данных|
| GFS          | Объектное хранилище | Обеспечивает обработку больших объемов данных, высокая производительность и отказоустойчивость|
| MPEG-DASH          | Стриминг | Адаптивность качества потока, поддержка различных кодеков, масштабируемость|
| Redis         | хранение сессий | быстрый доступ к данных, масштабируемость|
| Envoy         | локальная балансировка | более современное решение чем nginx, динамическая конфигурация, поддержка современных протоколов, отказоустойчивость, разделение трафика|
| MySQL Vitess         | основное хранилище | Поддержка шардирования. Vitess использует Go и gRPC для внутренней коммуникации между компонентами, что позволяет легко обрабатывать тысячи клиентов одновременно. Создает очень легкие соединения, что позволяет легко обрабатывать тысячи соединений |
| Prometheus         | сбор метрик | Высокая производительность, интеграция с графаной, поддержка алертов и уведомлений |
| Elasticsearch | сбор и анализ данных | Масштабируемость, мощные инструменты поиска и агрегации, гибкие возможности визуализации ||
| Jaeger | трассировка запросов | Масштабируемость, поддержка различных форматов запросов, интеграция с различными сервисами |
| Flink | потоковая обработка данных | Масштабируемость, обработка различных типов данных в реальном времени |
| Hadoop | пакетная обработка данных | Масштабируемость, обработка различных типов данных в партициях, надежность и устойчивость к ошибкам|
| Spark | пакетная обработка данных  | Масштабируемость, обработка данных в реальном времени и пакетном режимах, поддержка различных типов данных |
| RabbitMQ | Брокер сообщений | Гибкая маршрутизация сообщений, поддержка различных протоколов, гарантированная доставка сообщений, масштабируемость и отказоустойчивость |
## 9. Схема проекта
![SVG Image](hiload_youtube.drawio.svg)
Ссылка на актуальную схему: https://drive.google.com/file/d/1i0CeNobKb54DLINhxr9bRfP_IjhZE6Zf/view?usp=sharing
### Описание схемы:
- **Recommendation service:** 
    - user service собирает статистику с пользователей, и отправляет её в hadoop, 
    - откуда spark берет данные, агрегирует и составляет матрицу факторизации(обученная модель). 
    - Модель будет обучаться на действиях пользователей и видео в оффлайне.
    - Когда пользователей запросит ролики у рекомендательной системы она обратится в cache, который будет иметь некоторый TTL.
    - Если в cache не будет данных, актуальная модель загрузится и выдаст рекомендованные видео, из которых сразу уберём ненужные с помощью фильтров Блума.
- **Upload service:**
    - В upload service приходит json: ```UserId:10
Video ID: 100
Chunks: [a, b, c]
title: ...
description: ... ```
    - Каждый чанк вначале загружается в GFS
    - После этого отправляем в RabbitMQ ```UserId:10
Video ID: 100
chunkOrder: 1  
s3Url: s3.com/10-100-a``` с ссылкой на чанк
    - И отправляем в kafka ```UserId:10
Video ID: 100
num Chunks: 3
title: ...
description: ... ``` чтобы быть уверенными, что все чанки были обработаны
    - processor`s с помощью Round-robin считывают ссылки на чанки, делают запрос в GFS и перекодируют чанк во все разрешения
    - После чего отправляют чанк в очередь кафки, и отправляют RabbitMQ сообщение, что все доставлено.
    - RabbitMQ удалит обработанный чанк из очереди
    - Flink склеивает все чанки с метаинформацией о ролике по User_id и Video_id, что обеспечивает exactly once и корректное завершение обработки
    - После того, как флинк соединил ролики со всеми обработанными чанками, он отправляется в GFS(для хранения роликов)
    - Только после загрузки ролика на GFS он сохраняется в MySql Vitess
    - Также у популярных пользователь будет flag и их ролики сразу будут загружаться в CDN.
- **Subscription Service:**
    - После загрузки информации о ролике в базу они направляются в kafka откуда читаются Flink
    - Flink будет читать и сохранять данные о CDC (Change data capture) ``` user_id:6 -> subscribe {1, 2, 3, 4...} ```
    - Flink при получении информации о новом ролики будет склеивать его с подписчиками и отправлять в кеши, cache (1-100)-(100-200) и т.д. id "условно".
    - Если пользователь очень популярный, то будем записывать ролик в отдельный кеш для популярных пользователей.
    - У всех кешей будет TTL
    - Реализуем проверку, если пользователь не заходил n времени, то его кеш обновлять не будем.
## 10. Обеспечение надёжности
### Отказоустойчивость БД
- Реплицирование и шардирование баз данных
- MySQL Vitess имеет встроенные механизмы для автоматического обнаружения отказов и восстановления системы.
- Несколько геораспределенных ДЦ.
- Raid массивы
- Хранение копий роликов X2
### Отказоустойчивость  балансировка:
- Rate limits ограничение зарпосов на envoy
- произвольное сегментирование в dns
- cdn на стороне провайдера, для уменьшение кол-во запросов в бд
- CQRS синхронное чтение из БД, асинхронное запись через брокер сообщений
### Надежность сервисов
- Максимальное покрытие Unit/Integration/E2E-тестами.
- Graceful shutdown
- Graceful degradation
- Выделим железа так, чтобы оно при пиковых нагрузках, было загружено максимум на 70%.
- Flink для сохранение состояния и согласованность данных
- Flink использует State Backend для хранения состояния операций. State Backend поддерживает распределенное хранение состояния, что означает, что данные могут быть разделены и храниться на нескольких узлах
- Kafka сохранение сообщений на диске, гарантированная доставка.
- кеш на ленту подписок
- re-try запросы с дедлайном
### Observalvability
- Трейсинг запросов для микросервисных походов с помощью Jaeger
- Сбор метрик, логов с сервисов
- Настройка alert для 500 и падении сервисов

## 11. Расчет ресурсов
### CDN:
- Пиковый трафик рафик 1258 ГБ/c
- 1258 ГБ/c / 100 ГБ/c = 13 серверов
- Конфигурация 2U 2x6338/16x32GB/24xNVMe16T/100Gb/s
### Envoy:
- пиковый RPS 46078 * 2 = 92_212 (учтем пиковые нагрузки + ssl терминацию)
- 500 RPS / CPU
- на 1 ядро рекомендовано 512 мегабайт
- 92_212 / 500 = 185 / 64 = 3 серверов
- Конфигурация 1U 2x6338/16x128GB/2xNVMe8T/2x40Gb/s
### Сервисы:
- ~100 RPS/CPU
- 92_212 RPS = 92_212 / 100 = 922,12 CPU / 64 CPU/U = 15 серверов
- Конфигурация 1U 2x6338/2x16GB/2xNVMe8T/2x40Gb/s
### БД:
- ~  Суммарный 14 ПБ (табл)
- (14 * 1024) / (24 * 16) = 38 серверов
- 2 реплики (master, slave, slave) = 114 серверов
- Конфигурация 2U 2620v3/16x32GB/24xNVMe16T/2x25Gb/s
### S3:
- 7 629 ПБ / год
- 7629_000 / (24 * 16) = 20_000 серверов
- Конфигурация 2U 2620v3/16x32GB/24xNVMe16T/2x40Gb/s