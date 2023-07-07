# Проектная работа: Продажа квартир в Санкт-Петербурге — анализ рынка недвижимости

## Описание проекта

В нашем распоряжении данные сервиса Яндекс.Недвижимость — архив объявлений о продаже квартир в Санкт-Петербурге и соседних населённых пунктов за несколько лет. Нужно научиться определять рыночную стоимость объектов недвижимости. 

Наша задача — установить параметры. Это позволит построить автоматизированную систему: она отследит аномалии и мошенническую деятельность. 

По каждой квартире на продажу доступны два вида данных. Первые вписаны пользователем, вторые — получены автоматически на основе картографических данных. Например, расстояние до центра, аэропорта, ближайшего парка и водоёма. 

## План по выполнению проекта
### Шаг 1. 
- Откроем файл с данными и изучим общую информацию
Путь к файлу: `/datasets/real_estate_data.csv`
Датасеты приложены в локальном хранилище

- Загрузим данные из файла в датафрейм.
- Изучим общую информацию о полученном датафрейме.
- Построим общую гистограмму для всех числовых столбцов таблицы. Например, для датафрейма data это можно сделать командой data.hist(figsize=(15, 20)).

### Шаг 2. Предобработка данных
- Находим и изучим пропущенные значения в столбцах:
- Определим, в каких столбцах есть пропуски.
- Заполним пропущенные значения там, где это возможно. Например, если продавец не указал число балконов, то, скорее всего, в его квартире их нет. Такие пропуски правильно заменить на 0. Если логичную замену предложить невозможно, то оставьте эти значения пустыми. Пропуски — тоже важный сигнал, который нужно учитывать.
- В ячейке с типом markdown укажем причины, которые могли привести к пропускам в данных.
- Рассмотрим типы данных в каждом столбце:
- Находим столбцы, в которых нужно изменить тип данных.
- Преобразуем тип данных в выбранных столбцах.
- В ячейке с типом markdown поясним, почему нужно изменить тип данных.
- Изучим уникальные значения в столбце с названиями и устраните неявные дубликаты. Например, «поселок Рябово» и «поселок городского типа Рябово», «поселок Тельмана» и «посёлок Тельмана» — это обозначения одних и тех же населённых пунктов. Вы можете заменить названия в существующем столбце или создать новый с названиями без дубликатов.
- Находим и устраним редкие и выбивающиеся значения. Например, в столбце ceiling_height может быть указана высота потолков 25 м и 32 м. Логично предположить, что на самом деле это вещественные значения: 2.5 м и 3.2 м. Попробуйте обработать аномалии в этом и других столбцах.
 
Если природа аномалии понятна и данные действительно искажены, то восстановим корректное значение.
В противном случае удалим редкие и выбивающиеся значения.
В ячейке с типом markdown опишем, какие особенности в данных мы обнаружили.

### Шаг 3. Добавим в таблицу новые столбцы со следующими параметрами:
- цена одного квадратного метра;
- день недели публикации объявления (0 — понедельник, 1 — вторник и так далее);
- месяц публикации объявления;
- год публикации объявления;
- тип этажа квартиры (значения — «первый», «последний», «другой»);
- расстояние до центра города в километрах (переведем из М в КМ и округлим до целых значений).
- 
### Шаг 4. Проведите исследовательский анализ данных:
Изучим следующие параметры объектов:
- общая площадь;
- жилая площадь;
- площадь кухни;
- цена объекта;
- количество комнат;
- высота потолков;
- этаж квартиры;
- тип этажа квартиры («первый», «последний», «другой»);
- общее количество этажей в доме;
- расстояние до центра города в метрах;
- расстояние до ближайшего аэропорта;
- расстояние до ближайшего парка;
- день и месяц публикации объявления.

Построим отдельные гистограммы для каждого из этих параметров. 
Опишем все ваши наблюдения по параметрам в ячейке с типом markdown.

Изучим, как быстро продавались квартиры (столбец days_exposition). 
Этот параметр показывает, сколько дней было размещено каждое объявление. 
 
#### Постройте гистограмму. Посчитайте среднее и медиану.
- В ячейке типа markdown указываем, сколько времени обычно занимает продажа. Какие продажи можно считать быстрыми, а какие — необычно долгими?
- Какие факторы больше всего влияют на общую (полную) стоимость объекта?
- Изучим, зависит ли цена от:
    - общей площади;
    - жилой площади;
    - площади кухни;
    - количества комнат;
    - этажа, на котором расположена квартира (первый, последний, другой);
    - даты размещения (день недели, месяц, год).
- Построим графики, которые покажут зависимость цены от указанных выше параметров. Для подготовки данных перед визуализацией мы используем сводные таблицы.
- Посчитаем среднюю цену одного квадратного метра в 10 населённых пунктах с наибольшим числом объявлений. Выделим населённые пункты с самой высокой и низкой стоимостью квадратного метра. Эти данные можно найти по имени в столбце `locality_name`.
- Ранее мы посчитали расстояние до центра в километрах. Теперь выделим квартиры в Санкт-Петербурге с помощью столбца `locality_name` и вычислим среднюю цену каждого километра. Опишем, как стоимость объектов зависит от расстояния до центра города.

### Шаг 5. Напишите общий вывод
- Опишем полученные результаты и зафиксируйте основной вывод проведённого исследования.

#### Оформление
- Выполним задание в Jupyter Notebook. Заполним программный код в ячейках типа code, текстовые пояснения — в ячейках типа markdown. Применим форматирование и заголовки.

#### Описание данных
- `airports_nearest` — расстояние до ближайшего аэропорта в метрах (м)
- `balcony` — число балконов
- `ceiling_height` — высота потолков (м)
- `cityCenters_nearest` — расстояние до центра города (м)
- `days_exposition` — сколько дней было размещено объявление (от публикации до снятия)
- `first_day_exposition` — дата публикации
- `floor` — этаж
- `floors_total` — всего этажей в доме
- `is_apartment` — апартаменты (булев тип)
- `kitchen_area` — площадь кухни в квадратных метрах (м²)
- `last_price` — цена на момент снятия с публикации
- `living_area` — жилая площадь в квадратных метрах (м²)
- `locality_name` — название населённого пункта
- `open_plan` — свободная планировка (булев тип)
- `parks_around3000` — число парков в радиусе 3 км
- `parks_nearest` — расстояние до ближайшего парка (м)
- `ponds_around3000` — число водоёмов в радиусе 3 км
- `ponds_nearest` — расстояние до ближайшего водоёма (м)
- `rooms` — число комнат
- `studio` — квартира-студия (булев тип)
- `total_area` — общая площадь квартиры в квадратных метрах (м²)
- `total_images` — число фотографий квартиры в объявлении

# Итог

В объявлении о продажах мы в первую очередь обратимся дома со следующими параметрами, поскольку их очень много в объявлении: площадью около 100 м^2, с жилой площадью 0-70 м^2, кухонной площади 0-25 м^2, однокомнатной и двухкомнатной, высотой потолка 2,7 метра, с этажом ниже 5, ближе 10 км к центру города, к аэропорту - 20 км, к парку - 0,5 км. 

Обычно публикуются чаще всего во вторник, в августе. Обычно длится 100 дней в объявлении до тех пор, пока не найдется покупатель. В среднем длится 193 дней продажи в объявлении, но было зафиксировано быструю продажу - 2 дней, а самой медленной - 560 дней.

Хитом продажи можно называть тех дома с площадью 15-150 м^2 и со стоимостью 0-20 млн рублей, 2-4 комнат.

По графику завимость стоимость квартиры с годом публикации можно заметить, что стоимость колеблется, но с каждым годом средняя стоимость медленно растет

![img.png](image/img.png)

Здесь можно заметить, что реже всего продают в 2014. В 2017 есть объявление о продажах с большой стоимостью

![img_1.png](image/img_1.png)

Здесь можно заметить, что реже всего проводит публикация в июне и в апреле. Стоимость колеблется плавно, но нельзя отрицать, что есть выбросы. Если не учитывать эти выбросы, а только те точки, которые вплотно стоят друг друга, то график можно рассмотреть как синусиодальный с большим периодом

А частые - декабрь, но при этом имеет сильное стандартное отклонение, скорее всего это тот самый дом, который является самым дорогим

![img_2.png](image/img_2.png)

Здесь можно заметить, что ближе к центру города, тем стоимость одной площади м^2 дороже.

Здесь график получается плавно убывающим, подобие гиперболы.

Однако здесь необходимо обратить внимание на графике после 8 км, т.к. тут заметно, что цена меняется. Это возможный центр города.
Известно, что самый дорогой по стоимости жилья район в Санкт-Петербурге риелторы назвали "Золотой треугольник". Дома "Золотого треугольника" находятся в пределах Невского проспекта, набережной реки Фонтанки и Дворцовой набережной.

![img_3.png](image/img_3.png)