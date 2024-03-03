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
- Самый большой трафик идёт из Южной Азии, там будет находится дата-центр в Индии, чтобы обеспечить основные страны, Индия, Пакистан.
- Также в России необходим дата центр, который будет работать, как на саму Россию, так и на страны СНГ, Европы.
- В США и Бразилии будет расположен 3 и 4-ый дата-центр для обеспечения Северную и Южную Америку.

### DNS балансировка
- Будет использовать Latency-based DNS (AMAZON S53), в результате чего пользователю будет подбираться ближайший дата-центр, будем использовать эту технологию для глобальной балансировки.
### BGP Anycast
- Внутри регионов будем использовать BGP Anycast (будем выдавать один ip для нескольких дата-центров, отправлять пользователя к ближайшему дата-центру, cdn серверу)
- Будем использовать CDN сервера для отдачи статики (видео, информации о пользователе). Для этого будем кэшировать ролики в дата-центрах и дальше рассылать по cdn серверам, также будем предоставлять провайдерам кэши для ускорения контента(ISP), чтобы снять нагрузку с cdn серверов.
  - Кеш сервер будет работать так, отдавать пользователю контент и в моменты минимальной нагрузки загружать с cdn серверов новый контент.