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
Стандартное отклонение (RMSD) или среднеквадратичная ошибка (RMSE) - это часто используемая мера разницы между значением (выборочным или общим), предсказанным моделью или оценщиком, и наблюдаемым значением. и наблюдаемыми значениями, или квадратный корень из разницы между ними по второму моменту выборки, или среднеквадратичное значение этих разниц. Эти отклонения называются остатками при расчете по выборке данных, используемых для оценки, и ошибками (или ошибками предсказания) при расчете вне выборки. RMSD используется как единая мера предсказательной силы, включающая величину ошибки предсказания в разных точках данных. RMSD зависит от масштаба и поэтому сравнивает точность ошибок предсказания различных моделей для данного набора данных, а не между наборами данных.

RMSD всегда неотрицателен, при этом значение нуля (что редко достигается на практике) указывает на идеальное согласие с данными. В целом, низкий RMSD лучше, чем высокий RMSD. Однако эта мера зависит от используемой числовой шкалы, что делает невозможным сравнение между различными типами данных.

RMSD - это квадратный корень из среднего квадрата ошибок. Влияние каждой ошибки на RMSD пропорционально размеру квадрата ошибки, поэтому большие ошибки оказывают непропорционально большое влияние на RMSD. Поэтому RMSD чувствителен к выбросам.

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

### 3. Средняя абсолютная ошибка (MAE)

Средняя абсолютная ошибка (MAE) рассчитывается как среднее значение абсолютной разницы между прогнозами и фактическими наблюдениями. Математически мы можем представить это следующим образом:

Реализация Python для MAE выглядит следующим образом:
``` python
import numpy as np
def mean_absolute_error(act, pred):
    diff = pred - act
    abs_diff = np.absolute(diff)
    mean_diff = abs_diff.mean()
    return mean_diff

act = np.array([1.1,2,1.7])
pred = np.array([1,1.7,1.5])
mean_absolute_error(act,pred)
```
Выход :
``` python
0.20000000000000004
```
Вы также можете использовать mean_absolute_error из sklearn для расчета MAE.
``` python
from sklearn.metrics import mean_absolute_error
act = np.array([1.1,2,1.7])
pred = np.array([1,1.7,1.5])
mean_absolute_error(act, pred)
```
Выход :
``` python
0.20000000000000004
```
### 4. Функция кросс-энтропийной потери в Python

Перекрестная энтропийная потеря также известна как отрицательная логарифмическая вероятность. Это чаще всего используется для задач классификации. Проблема классификации — это проблема, в которой вы классифицируете пример как принадлежащий к одному из более чем двух классов.

Давайте посмотрим, как вычислить ошибку в случае проблемы бинарной классификации.

Давайте рассмотрим проблему классификации, когда модель пытается провести классификацию между собакой и кошкой.

Код Python для поиска ошибки приведен ниже.
``` python
from sklearn.metrics import log_loss
log_loss(["Dog", "Cat", "Cat", "Dog"],[[.1, .9], [.9, .1], [.8, .2], [.35, .65]])
```
Выход :
``` python
0.21616187468057912
```
Мы используем метод log_loss из sklearn.

Первый аргумент в вызове функции — это список правильных меток классов для каждого входа. Второй аргумент — это список вероятностей, предсказанных моделью.

Вероятности представлены в следующем формате:
``` python
[P(dog), P(cat)]
````
### Заключение

Это руководство было посвящено функциям потерь в Python. Мы рассмотрели различные функции потерь как для задач регрессии, так и для задач классификации. Надеюсь, вам было весело учиться вместе с нами!