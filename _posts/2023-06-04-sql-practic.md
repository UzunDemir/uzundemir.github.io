---
layout: post
title:  "SQL - my practics work"
---

![data2](https://github.com/UzunDemir/uzundemir.github.io/assets/94790150/83798a80-7579-4936-b50c-006ba0043188)


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
DROP SCHEMA public CASCADE; -- удаляем все таблицы удалив схему
```
```sql
CREATE SCHEMA public; -- создаем новую схему 
```
```sql
-- создаем нужные таблицы со столбцами 
CREATE TABLE product (
  maker VARCHAR(50),
  model VARCHAR(50),
  type VARCHAR(50)    
);
CREATE TABLE pc (
  code INTEGER,
  model INTEGER,
  speed INTEGER,
  ram INTEGER,
  hdd NUMERIC(4,1),
  cd VARCHAR(10),
  price NUMERIC(10,2)
);

CREATE TABLE laptop (
  code INTEGER,
  model INTEGER,
  speed INTEGER,
  ram INTEGER,
  hdd NUMERIC(4,1),
  price NUMERIC(10,2),
  screen Integer
);

CREATE TABLE printer (
  code INTEGER,
  model INTEGER,
  color VARCHAR(10),
  type VARCHAR(50),
  price NUMERIC(10,2)  
);
```
```sql
-- генерируем данные и заполняем таблицы
DO $$
-- декларируем переменные
-------------------------product
DECLARE A TEXT; -- maker
declare B INT; --model
-------------------------pc, laptop, printer
declare code_ int;
declare model_ int;
declare speed_ int;
declare ram_ int;
declare hdd_ NUMERIC(10, 1);
declare cd_ text;
declare price_ NUMERIC(10, 2);
declare type_ text;
declare color_ text;
-------------------------
BEGIN
    FOR i IN 1..100 LOOP -- 100 значений
        IF random() < 0.04 then   A := 'Alta';
        ELSIF random() < 0.08 then   A := 'Amdahl'; B := random()*10000; code_ := random()*10+1; model_ := 1200 + random()*100; speed_ := random()*10*100; ram_ := random()*10*32; hdd_ := code_+ 8; cd_ := concat(code_*2+12, 'x'); price_ := B+354; type_ := 'HP (Hewlett-Packard)'; color_ := 'b';
        ELSIF random() < 0.12 then   A := 'Avalon'; B := random()*10000; code_ := random()*10+1; model_ := 1200 + random()*100; speed_ := random()*10*100; ram_ := random()*10*32; hdd_ := code_+ 8; cd_ := concat(code_*2+12, 'x'); price_ := B+354; type_ := 'Canon'; color_ := 'b';
        ELSIF random() < 0.16 then   A := 'Bull'; B := random()*10000; code_ := random()*10+1; model_ := 1200 + random()*100; speed_ := random()*10*100; ram_ := random()*10*32; hdd_ := code_+ 8; cd_ := concat(code_*2+12, 'x'); price_ := B+354; type_ := 'Canon'; color_ := 'w';
        ELSIF random() < 0.20 then   A := 'CPP'; B := random()*10000; code_ := random()*10+1; model_ := 1200 + random()*100; speed_ := random()*10*100; ram_ := random()*10*32; hdd_ := code_+ 8; cd_ := concat(code_*2+12, 'x'); price_ := B+354; type_ := 'Epson'; color_ := 'b';
        ELSIF random() < 0.24 then   A := 'Compaq/DEC'; B := random()*10000; code_ := random()*10+1; model_ := 1200 + random()*100; speed_ := random()*10*100; ram_ := random()*10*32; hdd_ := code_+ 8; cd_ := concat(code_*2+12, 'x'); price_ := B+354;
        ELSIF random() < 0.28 then   A := 'Cray'; B := random()*10000; code_ := random()*10+1; model_ := 1200 + random()*100; speed_ := random()*10*100; ram_ := random()*10*32; hdd_ := code_+ 8; cd_ := concat(code_*2+12, 'x'); price_ := B+354; type_ := 'Epson'; color_ := 'b'; 
        ELSIF random() < 0.32 then   A := 'CSPI'; B := random()*10000; code_ := random()*10+1; model_ := 1200 + random()*100; speed_ := random()*10*100; ram_ := random()*10*32; hdd_ := code_+ 8; cd_ := concat(code_*2+12, 'x'); price_ := B+354; type_ := 'Brother'; color_ := 'w';
        ELSIF random() < 0.36 then   A := 'Data General'; B := random()*10000; code_ := random()*10+1; model_ := 1200 + random()*100; speed_ := random()*10*100; ram_ := random()*10*32; hdd_ := code_+ 8; cd_ := concat(code_*2+12, 'x'); price_ := B+354; type_ := 'Xerox'; color_ := 'b';
        ELSIF random() < 0.40 then   A := 'Dell'; B := random()*10000; code_ := random()*10+1; model_ := 1200 + random()*100; speed_ := random()*10*100; ram_ := random()*10*32; hdd_ := code_+ 8; cd_ := concat(code_*2+12, 'x'); price_ := B+354; type_ := 'Brother'; color_ := 'w';
        ELSIF random() < 0.44 then   A := 'Fujitsu'; B := random()*10000; code_ := random()*10+1; model_ := 1200 + random()*100; speed_ := random()*10*100; ram_ := random()*10*32; hdd_ := code_+ 8; cd_ := concat(code_*2+12, 'x'); price_ := B+354; type_ := 'Lexmark'; color_ := 'b';
        ELSIF random() < 0.48 then   A := 'Hewlett-Packard'; B := random()*10000;
        elsif random() < 0.52 then   A := 'Hitachi'; B := random()*10000; type_ := 'Xerox'; color_ := 'b';
        ELSIF random() < 0.56 then   A := 'IBM'; B := random()*10000;
        ELSIF random() < 0.60 then   A := 'Intel'; B := random()*10000; type_ := 'Samsung'; color_ := 'w';
        ELSIF random() < 0.64 then   A := 'NEC'; B := random()*10000;
        ELSIF random() < 0.68 then   A := 'Parsytec'; B := random()*10000; type_ := 'Lexmark'; color_ := 'b';
        ELSIF random() < 0.72 then   A := 'Quadrics'; B := random()*10000;
        ELSIF random() < 0.76 then   A := 'Sequent'; B := random()*10000; type_ := 'Ricoh'; color_ := 'w';
        ELSIF random() < 0.80 then   A := 'Siemens-Nixdorf'; B := random()*10000;
        ELSIF random() < 0.84 then   A := 'SGI'; B := random()*10000; type_ := 'Kyocera'; color_ := 'b';
        ELSIF random() < 0.88 then   A := 'Sun'; B := random()*10000;
        ELSIF random() < 0.92 then   A := 'SCS'; B := random()*10000;
        ELSIF random() < 0.99 then   A := 'Unisys'; B := random()*10000; type_ := 'Dell'; color_ := 'w';
       --
       
        END IF;

        
        RAISE NOTICE 'Adding record with model %, %, %, %, %, %, %, %, %', A, B, code_, model_, speed_, ram_, hdd_, cd_, price_;
        INSERT INTO Product (maker, model, type) VALUES (A, B, 'PC'); -- product
        INSERT INTO PC (code, model, speed, ram, hdd, cd, price) VALUES (code_, model_, speed_, ram_, hdd_, cd_, price_); -- pc
        INSERT INTO Laptop (code, model, speed, ram, hdd, price, screen) VALUES (code_* 10, model_+ 1234, speed_+ 1, ram_+12, hdd_, price_/2, hdd_+10); -- laptop
        INSERT INTO printer (code, model, color, type, price) VALUES (code_* 3, model_+ 34, color_, type_, price_/2); -- printer
    END LOOP;
END $$;
```
```sql
ALTER TABLE product ALTER COLUMN model TYPE integer USING model::integer; -- изменить тип столбца
```

```sql
-- выводит уникальные значения, которые не встречаются в printer
SELECT DISTINCT model
FROM laptop
WHERE model not IN (SELECT model FROM printer);
```
#### Задача
1. Напишите запросы к базе данных.
* Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price.
```sql
SELECT model, price
FROM Printer
WHERE price = (SELECT MAX(price) FROM Printer);
```
* Найдите среднюю скорость ПК.
```sql
SELECT AVG(speed) FROM PC;
```
* Найдите производителя, продающего ПК, но не ноутбуки.
```sql
SELECT model
FROM product
WHERE model NOT IN (SELECT model FROM laptop);
```
2. Загрязните специально датасет (вставьте новые значения с уникальным кодом, но всеми остальными дублирующими полями).
```sql
INSERT INTO PC (code, model, speed, ram, hdd, cd, price)
SELECT random()*10000, model, speed, ram, hdd, cd, price
FROM PC
ORDER BY random()
LIMIT 10;
```
3. Напишите оконную функцию, которая поможет вам обнаружить эти строки-редиски.
```sql
DELETE FROM pc
WHERE (model, speed, ram, hdd, cd, price) IN (
  SELECT model, speed, ram, hdd, cd, price
  FROM pc
  GROUP BY model, speed, ram, hdd, cd, price
  HAVING COUNT(*) > 1
);
```
4. Обновите название колонки в таблице printer с color на color_type и поменяйте тип поля.
```sql
-- Изменяем название колонки на color_type и меняем тип поля на varchar(25)
ALTER TABLE printer RENAME COLUMN color TO color_type
ALTER TABLE printer ALTER COLUMN color_type TYPE varchar(25);
```
5. В последнем пункте выполните слияние двух запросов из таблиц PC и Laptop, выбрав только те значения, у которых цена больше 500, а ram = 64.

```sql
-- Объединяем результаты двух запросов с помощью оператора UNION
SELECT ram, price, 'PC' as type
FROM PC
WHERE price > 500 AND ram = 64
union 
SELECT ram, price, 'Laptop' as type
FROM Laptop
WHERE price > 500 AND ram = 64;
```
