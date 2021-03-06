﻿## ✨ Лаба 6. Суперачивка. Построение неперсонализированной рекомендательной системы для фильмов

##### [![New Professions Lab — Big Data 10](images/npl7.svg)](https://github.com/newprolab/content_bigdata10)

### Дедлайн

⏰ Четверг, 23 мая 2018 года, 23:59.

### Задача

По данным о рейтингах фильмов из MovieLens рекомендовать топ-10 фильмов по разным критериям.

#### Обработка данных на вход

Имеются следующие входные данные:
* Та же таблица `users * films` с рейтингами, что и в [Лабе 6](lab06.md).
* Параметр `k` для расчета поправленного среднего рейтинга. Параметр берётся из [Личного кабинета](http://lk.newprolab.com/lab/laba06s).
* Процент доверия для расчёта границ доверительного интервала. Параметр берётся из [Личного кабинета](http://lk.newprolab.com/lab/laba06s).

#### Обработка данных на выход

Для каждого фильма необходимо посчитать:
1. Количество человек `n`, поставивших рейтинг фильму .
2. Средний рейтинг фильма (`сумма рейтингов фильма / количество человек, оценивших фильм`) 

![tex1](images/lab06s_eq1.svg)

3. Количество человек `m`, оценивших фильм положительно. Оценки 4 и выше  считаются положительными. 
4. Доля людей, оценивших фильм положительно (`пункт 3 / пункт 1` или `m / n`).
5. Глобальное среднее по всему датасету. `Сумма всех оценок по всем фильмам /Количество всех оценок по всем фильмам`.

![tex2](images/lab06s_eq2.svg)

6. Оценку, поправленную на нехватку данных:

![tex3](images/lab06s_eq3.svg)

Мы искусственно добавляем `k` глобальных средних (\mu из пункта 5) каждому фильму.

7. Нижнюю и верхнюю границы доверительного интервала оценки (Wilson score interval) из лекции с заданным уровнем доверия. 

![tex4](images/lab06s_eq4.svg)

где  `n` —  количество рейтингов (пункт 1), `p` - доля людей, оценивших фильм положительно (пункт 4).
   
| **уровень доверия, %** | 99.9  | 99    | 95    | 90    |
| ---------------------- | ----- | ----- | ----- | ----- |
| **z**                  | 3.291 | 2.576 | 1.960 | 1.645 |

Рекомендовать топ-10 фильмов (**если рейтинги совпадают, то сортировать по алфавиту названий фильмов от A до Z**):

1. По откликам (пункт 1) — поле `“top10_rates”`.
2. По среднему рейтингу (пункт 2) — поле `“top10_average”`.
3. По среднему рейтингу с регуляризацией `k` (пункт 6) — поле `“top10_rating”`.
4. По нижней границе доверительного интервала Wilson (пункт 7) — поле `“top10_lower”`.


В поле `“top10_rates”` нужно указать `id` 10 фильмов, упорядоченных по убыванию числа количества человек, посмотревших фильм.

В поле `“top10_average”` нужно указать `id` 10 фильмов, упорядоченных по среднему рейтингу.

В поле `“top10_rating”` нужно указать `id` 10 фильмов, упорядоченных по среднему рейтингу с регуляризацией.

В поле `“top10_lower”` нужно указать id `10` фильмов, упорядоченных по нижней границе доверительного интервала.

Выходной формат — json. Пример решения:

```
{  
   "top10_rates": [  
      13456,
      12378,
      78213,
      ...
   ],
   "top10_average": [  
      13456,
      12378,
      78213,
      ...
   ],
   "top10_rating": [  
      13456,
      12378,
      78213,
      ...
   ],
   "top10_lower": [  
      13456,
      12378,
      78213,
      ...
   ]
}
```

### Проверка

Файл необходимо положить в свою домашнюю директорию под названием: `lab06s.json`.

Проверка осуществляется [автоматическим скриптом](http://lk.newprolab.com/lab/laba06s) из Личного кабинета.
