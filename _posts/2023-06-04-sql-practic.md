---
layout: post
title:  "SQL - my practics work"
---

### Цель практической работы

В качестве практической работы для закрепления навыков предлагаем вам потренироваться на достаточно популярной базе данных «Компьютерная фирма».

Схема БД состоит из четырёх таблиц:
```
Product (maker, model, type).
PC (code, model, speed, ram, hd, cd, price).
Laptop (code, model, speed, ram, hd, screen, price).
Printer (code, model, color, type, price).
```
Таблица Product представляет производителя (maker), номер модели (model) и тип (PC — ПК, Laptop — портативный компьютер или Printer — принтер). Предполагается, что в этой таблице номера моделей уникальны для всех производителей и типов продуктов. В таблице PC для каждого номера модели, обозначающего ПК, указаны скорость процессора — speed (МГц), общий объём оперативной памяти — ram (Мб), размер диска — hd (Гб), скорость считывающего устройства — cd (например, '4х'), цена — price. Таблица Laptop аналогична таблице РС за исключением того, что вместо скорости CD-привода содержит размер экрана — screen (в дюймах). В таблице Printer для каждой модели принтера указывается, является ли он цветным — color ('y', если цветной), тип принтера — type (лазерный — Laser, струйный — Jet или матричный — Matrix) и цена — price. 
![image](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/24ec4872-8af5-4728-867a-c59fb8524a9d)

Что входит в работу
Итак, перед вами будут стоять следующие задачи:

* создать все указанные таблицы;
* наполнить их данными (как минимум по 100 значений);
* выполнить задачи, которые подробно описаны ниже.

*Подсказка к пункту 2. Код для создания и заполнения таблиц можно создать с помощью Python: модуль random поможет сгенерировать случайные значения, чтобы заполнить таблицу большим объёмом данных.*

Пример кода для таблицы PC
```python
import random

with open('create_and_fill_PC.sql', 'w') as file:
    file.write(
        'CREATE TABLE PC (code INTEGER, model INTEGER, speed INTEGER, ram INTEGER, '
        'hd NUMERIC(10,1), cd VARCHAR(50), price NUMERIC(10,4));\n'
    )
    file.write('INSERT INTO PC\n')
    file.write('VALUES\n') 

    values = []
    for code, i in enumerate(range(1121, 1225), start=1):
        values.append(
            f'({code}, {i}, {random.randrange(500, 901, 100)},'
            f' {random.randrange(32, 129, 32)}, {random.randrange(5, 21, 5)},'
            f' \'{random.randrange(12, 53, 4)}x\', {random.randrange(350, 1001, 50)})'
        )

    # Чтобы в конце не стояла запятая и код корректно сработал
    file.write(',\n'.join(values) + ';\n')
 ```
    
### Задача

1. Напишите запросы к базе данных.

    * Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price.

    * Найдите среднюю скорость ПК.

    * Найдите производителя, продающего ПК, но не ноутбуки.

2. Загрязните специально датасет (вставьте новые значения с уникальным кодом, но всеми остальными дублирующими полями).

3. Напишите оконную функцию, которая поможет вам обнаружить эти строки-редиски.

4. Обновите название колонки в таблице printer с color на color_type и поменяйте тип поля.

5. В последнем пункте выполните слияние двух запросов из таблиц PC и Laptop, выбрав только те значения, у которых цена больше 500, а ram = 64.

Так как задание получается достаточно серьезное и интересное, то я решил записать всю работу.

#### 1. Установка PostgreSQL

В процессе установки, я руководствовался этим [видео.](https://www.youtube.com/watch?v=gl91tlwqA9Y) 

[Установка и базовая настройка PostgreSQL в Windows 10](https://winitpro.ru/index.php/2019/10/25/ustanovka-nastrojka-postgresql-v-windows/)

Я установил с [официального источника](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads) актуальную на тот момент версию 15 для Windows.

#### 2. Доступ к PostgreSQL по сети, правила файерволла

Чтобы разрешить сетевой доступ к вашему экземпляру PostgreSQL с других компьютеров, вам нужно создать правила в файерволе. Вы можете создать правило через командную строку или PowerShell.

Запустите командную строку от имени администратора. Введите команду:
```
netsh advfirewall firewall add rule name="Postgre Port" dir=in action=allow protocol=TCP localport=5432
```
* Где rule name – имя правила
* Localport – разрешенный порт

После применения команды в брандмауэре Windows появится новое разрешающее правило для порта Postgres.
![image](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/dd53e9f3-289e-48ff-a583-baead6e41d83)

#### 3. Утилиты управления PostgreSQL через командную строку

Рассмотрим управление и основные операции, которые можно выполнять с PostgreSQL через командную строку с помощью нескольких утилит. Основные инструменты управления PostgreSQL находятся в папке bin, потому все команды будем выполнять из данного каталога.

Перед запуском СУБД, смените кодировку для нормального отображения в русской Windows 10. В командной строке выполните: `chcp 1251`

* Запустите командную строку.
* Перейдите в каталог bin выполнив команду: `CD C:\Program Files\PostgreSQL\11\bin`

![image](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/a75369aa-0433-484b-ac82-35a3375f8821)

Основные команды PostgreSQL:

* Проверка установленной версии СУБД: `psql –V`

* Для создания новой базы данных воспользуйтесь утилитой createdb: `createdb -U postgres testdb` (где postgres суперпользователь, testdb новая база данных)Введите пароль суперпользователя.

![image](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/5c441853-fe10-40e1-b055-4ae6ce4f8e3a)

* Проверить список активных баз: `Psql -U postgres –l` (пароль)

![image](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/9d212e7a-8500-4e93-b201-7a9febc084cb)

* С помощью инструмента createuser cоздадим нового пользователя: `createuser –U postgres operator` (где operator -имя нового пользователя)

![image](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/02436243-c3c2-4954-8117-e53d55941012)

* Предоставим пользователю привилегии суперпользователя (на практике этого делать не надо). 
* Запустите интерактивную командную оболочку управления PostgreSQL (shell): `psql –U postgres` . 
* С помощью SQL команды `ALTER ROLE` предоставим нужные права нашему пользователю: `ALTER ROLE operator SUPERUSER CREATEROLE CREATEDB;` . 
* Мы предоставили пользователю права суперпользователя, права на создание ролей и баз данных.
* Для выводы списка пользователей и ролей в СУБД выполните команду: `\du`

![image](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/cd3a99c6-608c-43e4-bfec-bc51c75ee225)

#### 4. PgAdmin: Визуальный редактор для PostgresSQL

Редактор PgAdmin служит для упрощения управления базой данных PostgresSQL в понятном визуальном режиме.

* Для запуска редактора запустите PgAdmin 4 в меню Пуск
* Для доступа нужно ввести пароль суперпользователя postgres
* В панели Servers вы можете раскрыть список активных БД.

![image](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/db1e901a-11f5-429b-95c4-70a9063e5ac0)

* В панели управления возможно быстро создать нового пользователя и группу, предоставить ему права. Для этого Откройте меню Object -> Create -> Create Login/Group.

![image](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/1576c966-fc26-405b-aacc-d05cbff555ac)

* Для создания новой базы данных достаточно выбрать: Database в меню Object -> Create. В новом поле указать имя базы и владельца.

По умолчанию все созданные базы хранятся в каталоге base по пути `C:\Program Files\PostgreSQL\11\data\base.`

Для каждой `БД` существует подкаталог внутри `PGDATA/base`, названный по `OID` базы данных в `pg_database`. Этот подкаталог по умолчанию является местом хранения файлов базы данных; в частности, там хранятся её системные каталоги. Каждая таблица и индекс хранятся в отдельном файле.

Для резервного копирования и восстановления лучше использовать инструмент `Backup` в панели инструментов `Tools`. Для автоматизации бэкапа `PostgreSQL` из командной строки используйте утилиту `pg_dump.exe`.

#### Создание таблиц в Dbeaver 

Нужно создать таблицы:

```
Product (maker, model, type).
PC (code, model, speed, ram, hd, cd, price).
Laptop (code, model, speed, ram, hd, screen, price).
Printer (code, model, color, type, price).
```
```sql
DROP SCHEMA public CASCADE;
```