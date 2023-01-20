---
layout: post
title:  "Как избавиться от PerformanceWarning?"
---
Выполняя работу по оценке модели и подготовке к передаче заказчику в модуле появилась предупреждение.

Как все выглядело:

![Image alt]({{ site.baseurl }}//images/screenshot 20_01_23_2.png)


Что это означает и как от этого избавится?

Предупреждение возникает при возможном влиянии на производительность.

*[Источник](https://github.com/pandas-dev/pandas/blob/v1.5.3/pandas/errors/__init__.py#L31-L34)*


Устраняется это вот таким методом, который описан [здесь.](https://stackoverflow.com/questions/51521526/python-pandas-how-to-supress-performancewarning)

