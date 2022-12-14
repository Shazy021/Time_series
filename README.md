# Прогнозирование заказов такси

<h1>Содержание<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#1.-Подготовка" data-toc-modified-id="1.-Подготовка-1">1. Подготовка</a></span><ul class="toc-item"><li><span><a href="#Вывод" data-toc-modified-id="Вывод-1.1">Вывод</a></span></li></ul></li><li><span><a href="#2.-Анализ" data-toc-modified-id="2.-Анализ-2">2. Анализ</a></span><ul class="toc-item"><li><span><a href="#2.1-Общая-картина" data-toc-modified-id="2.1-Общая-картина-2.1">2.1 Общая картина</a></span></li><li><span><a href="#2.2-Декомпозиция" data-toc-modified-id="2.2-Декомпозиция-2.2">2.2 Декомпозиция</a></span></li><li><span><a href="#Вывод" data-toc-modified-id="Вывод-2.3">Вывод</a></span></li></ul></li><li><span><a href="#3.-Подготовка-данных" data-toc-modified-id="3.-Подготовка-данных-3">3. Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Вывод" data-toc-modified-id="Вывод-3.1">Вывод</a></span></li></ul></li><li><span><a href="#4.-Обучение" data-toc-modified-id="4.-Обучение-4">4. Обучение</a></span></li><li><span><a href="#5.-Тестирование" data-toc-modified-id="5.-Тестирование-5">5. Тестирование</a></span><ul class="toc-item"><li><span><a href="#5.1-LinearRegression" data-toc-modified-id="5.1-LinearRegression-5.1">5.1 LinearRegression</a></span></li><li><span><a href="#5.2-RandomForestRegressor" data-toc-modified-id="5.2-RandomForestRegressor-5.2">5.2 RandomForestRegressor</a></span></li><li><span><a href="#5.3-LGBMRegressor" data-toc-modified-id="5.3-LGBMRegressor-5.3">5.3 LGBMRegressor</a></span></li><li><span><a href="#5.4-CatBoostRegressor" data-toc-modified-id="5.4-CatBoostRegressor-5.4">5.4 CatBoostRegressor</a></span></li></ul></li><li><span><a href="#6.-Результаты" data-toc-modified-id="6.-Результаты-6">6. Результаты</a></span></li><li><span><a href="#Общий-вывод" data-toc-modified-id="Общий-вывод-7">Общий вывод</a></span></li>

## Описание проекта
Компания собрала исторические данные о заказах такси в аэропортах. Чтобы привлекать больше водителей в период пиковой нагрузки, нужно спрогнозировать количество заказов такси на следующий час. Постройте модель для такого предсказания.

## Задача
Значение метрики RMSE на тестовой выборке должно быть не больше 48.

1. Загрузить данные и выполнить их ресемплирование по одному часу.
2. Проанализировать данные.
3. Обучить разные модели с различными гиперпараметрами. Сделать тестовую выборку размером 10% от исходных данных.
4. Проверить данные на тестовой выборке и сделать выводы.

## Вывод
* На первом этапе произведена загрузка данных и их подготовка для обучения моделей;
* За время наблюдений наметился определенный тренд на общее увеличение заказов такси в течение часа, которое скорее всего вызвано работой общественного транспорта. На графиках явно видна суточная сезонность. Ночью количество заказов стремится к нулю, в то время как вечерний час пик - момент самого сильного спроса на услуги такси.
* На третьем этапе к датасету были добавлены дополнительные признаки, а именно день недели, час "отстающие значения" и скользящее среднее.
* На четвертом и пятом этапах проведено тестирование всех моделей, каждой из них удалось достичь требуемого показателя метрики RMSE. Путем проверки обученных моделей на тестовых выборках, пришли к выводу, что модель LGBMRegressor дает лучшие показатели.
* На шестом этапе провели сравнительный анализ графиков в двух масштабах, показал, что модель хуже всего справляется с предсказаниями на высоких пиках и на провалах, но в целом, угадывает направление движения.

|                     |	LinearRegression|	RandomForest|	LightGBM|	CatBoost|
|---------------------|-----------------|-------------|---------|---------|
RMSE(train)|	25.781551|	10.119625|	16.913556|	10.084195|
RMSE(test)|	45.687386|	43.827003|	41.805253|	40.085333|
Время обучения (секунды)|	4.581428|	49.451477|	12.482001|	25.378567|
Время предсказания (секунды)|	0.003998|	0.041001|	0.001997|	0.006|
