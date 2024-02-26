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

| Действие пользователя   | Кол-во (млн) |
|-------------------------|:------------:|
| DAU_AUTH_RUS            |     42.9     |
| DAU_AUTH_SOUTH_ASIA     |     240      |
| DAU_AUTH_NORTH_AMERICA  |    144.9     |
| DAU_AUTH_SOUTH_AMERICA  |     64.8     |
| DAU_AUTH_WORLD          |    492.6     |
| -                       |      -       |
| MAU_VIEWS_RUSSIA        |     336      |
| MAU_VIEWS_SOUTH_ASIA    |    1.420     |
| MAU_VIEWS_NORTH_AMERICA |     374      |
| DAU_VIEWS_SOUTH_AMERICA |     217      |


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
  -    VIEWS/COMMENT = 140_445_492 / 172_281 = 821
  -    VIEWS/COMMENT = (5_914_354/ 14_390) = 411
  -    VIEWS/COMMENT = (1.5 млн / 1_190) = 1260
  -    VIEWS/COMMENT = (2.2 млн / 8_755) = 251
  -    VIEWS/COMMENT = (2.2 млн / 8_755) = 428
  -    VIEWS/COMMENT = (2.2 млн / 3617) = 580
  -    VIEWS/COMMENT = (27 млн / 96945) = 278
  ```
  Приблизительно получаем на 521 просмотр на 1 комментарий
#### Россия:
    MAU_COMMENT_RUSSIA = 42.9 
#### Южная Азия
    MAU_VIEWS_INDIA = 1.420 млн
    MAU_PAKISTAN ~ (слишком мала)
    MAU_SOUTH_ASIA = MAU_INDIA  = 1.420 млн
#### Северная Америка
    MAU_USA = 336 млн
    MAU_MEXICO ~ (слишком мала)
    MAU_CANADA = 38 млн
    MAU_NORTH_AMERICA = MAU_USA + MAU_CANADA = 374 млн
#### Южная Америка
    MAU_BRAZIL = 217 млн
    MAU_SOUTH_AMERICA = DAU_BRAZIL = 217 млн
