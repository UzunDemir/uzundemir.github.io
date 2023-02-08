---
layout: post
title:  "Как быстро поменять окружение в conda"
---

Выполняя практическое задание по созданию API для обученной модели, я столкнулся с одной проблемой.

Описание:

Для активации окружений GitBush на команду *conda activate* не реагировала и выдавала 

*bash: conda: command not found*

![Image alt]({{ site.baseurl }}//images/generator.jfif)

Не знаю с чем это связано, пути может не указаны, времени нет разбираться... После 2-х часов различных манипуляций, среди которых были:

[это](http://pythonr.blogspot.com/2016/09/conda-anaconda-python.html), [вот это](https://stackoverflow.com/questions/35246386/conda-command-not-found), еще [вот это](https://medium.com/nuances-of-programming/8-%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%B2%D1%8B%D1%85-%D0%BA%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4-%D0%B4%D0%BB%D1%8F-%D1%83%D0%BF%D1%80%D0%B0%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F-%D1%81%D1%80%D0%B5%D0%B4%D0%B0%D0%BC%D0%B8-conda-a916bf1f3d05), и уже кое что, чего не помню, GitBush по прежнему не реагировал на condu. 

В среде Anaconda Power Promt команда *conda activate (deactivate)* отрабатывали свое и активировали (деактивировали) окружение по умолчанию 'base'. Команда *conda env list* вызывала список окружений. Но команда *source activate loan_service* и многие другие испробованные варианты не позволяли сменить окружение на нужное.

Решил проблему так:

Заходим в Anaconda Navigator и в Applications меняем галочку с Base на нужное окружение. После этого заходим на Anaconda Power Promt и активируем *conda activate*

![Image alt]({{ site.baseurl }}//images/conda1.png)

После этого *conda activate loan_service* переключила нужное окружение. 

![Image alt]({{ site.baseurl }}//images/conda3.png)

GitBush тоже, кстати после этого стал реагировать!

![Image alt]({{ site.baseurl }}//images/conda2.png)
