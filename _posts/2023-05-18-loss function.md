---
layout: post
title:  "Функции потерь в Python — простая реализация"
---
![water_loss_spin_compressed-2](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/676855f4-9ea9-44d9-9cdd-ce032c523a82)

Функции потерь Python являются важной частью моделей машинного обучения. Эти функции показывают, насколько сильно предсказанный моделью результат отличается от фактического.

Существует несколько способов вычислить эту разницу. В этом материале мы рассмотрим некоторые из наиболее распространенных функций потерь.

В этом уроке будут рассмотрены следующие четыре функции потерь.

* Среднеквадратическая ошибка
* Среднеквадратическая ошибка
* Средняя абсолютная ошибка
* Кросс-энтропийные потери

Из этих четырех функций потерь первые три применяются к модели классификации.

## Реализация функций потерь в Python

### 1. Среднеквадратическая ошибка (MSE)

Среднеквадратичная ошибка (MSE) рассчитывается как среднее значение квадратов разностей между прогнозируемыми и фактически наблюдаемыми значениями. Математически это можно выразить следующим образом:

Реализация MSE на языке Python выглядит следующим образом:

``` python
import numpy as np
def mean_squared_error(act, pred):

   diff = pred - act
   differences_squared = diff ** 2
   mean_diff = differences_squared.mean()
   
   return mean_diff

act = np.array([1.1,2,1.7])
pred = np.array([1,1.7,1.5])

print(mean_squared_error(act,pred)) 
```
Выход :

``` python
0.04666666666666667
```
Вы также можете использовать mean_squared_error из sklearn для расчета MSE. Вот как работает функция:

``` python
from sklearn.metrics import mean_squared_error
act = np.array([1.1,2,1.7])
pred = np.array([1,1.7,1.5])
mean_squared_error(act, pred)
```
Выход :
``` python
0.04666666666666667
```
### 2. Среднеквадратическая ошибка (RMSE)
Среднеквадратическая ошибка (RMSE) рассчитывается как квадратный корень из среднеквадратичной ошибки. Математически мы можем представить это следующим образом:

Реализация Python для RMSE выглядит следующим образом:
``` python
import numpy as np
def root_mean_squared_error(act, pred):

   diff = pred - act
   differences_squared = diff ** 2
   mean_diff = differences_squared.mean()
   rmse_val = np.sqrt(mean_diff)
   return rmse_val

act = np.array([1.1,2,1.7])
pred = np.array([1,1.7,1.5])

print(root_mean_squared_error(act,pred))
```
Выход :
``` python
0.21602468994692867
```
Вы также можете использовать mean_squared_error из sklearn для расчета RMSE. Давайте посмотрим, как реализовать RMSE, используя ту же функцию:
``` python
from sklearn.metrics import mean_squared_error
act = np.array([1.1,2,1.7])
pred = np.array([1,1.7,1.5])
mean_squared_error(act, pred, squared = False) #Если установлено значение False, функция возвращает значение RMSE.
```
Выход :
``` python
0.21602468994692867
```
Если для параметра squared установлено значение True, функция возвращает значение MSE. Если установлено значение False, функция возвращает значение RMSE.

