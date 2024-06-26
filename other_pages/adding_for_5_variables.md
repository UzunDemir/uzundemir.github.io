# 5.9 Итоги пройденных тем. Проверьте себя
Итак, время практики. 

Ваша цель — закрепить пройденный материал о переменных, типах данных и булевых значениях.

Перед вами будет стоять шесть задач.

Выполняя их, старайтесь соблюдать следующие критерии:

Вывод программы должен быть в том виде, который описан в задаче.
Переменные должны иметь информативные названия.
Поехали?

[Скачайте Jupyter Notebook.](https://github.com/UzunDemir/uzundemir.github.io/blob/master/other_files/%D0%9A%D0%BE%D0%BF%D0%B8%D1%8F%205.8_%D0%9F%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%BE%D0%B5_%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5.ipynb) 

В нём продублированы все шесть задач. Выполните их прямо в этом файле.

Чтобы свериться с правильными ответами, нажимайте на кнопку «Продолжить».

Первая задача: используя переменные с текстовыми значениями, выведите на печать осмысленную фразу. Сделайте это с помощью конкатенации.

Переменные

inf = "греться "

noun = "змеи "

obj = "солнце "

prep = "на "

verb = "любят "

Решение умещается в одну строку. Получилось её составить?

Решение может быть таким:

print(noun + verb + inf + prep + obj)

Важно, чтобы слова были перечислены через +, а не через запятую. Именно так реализуется конкатенация, склеивание строк. Это было условием задачи.

А вот порядок переменных у вас может отличаться, в русском всё-таки нет жёсткого правила об этом. Главное, чтобы переменная prep (предлог «на») была перед переменной obj (существительным «солнце») ;)

print(noun + verb + inf + prep + obj)

print(noun + verb + inf + prep + obj

Представьте, что вы проехали на такси 15,5 километра. 

Поездка обошлась вам в 520 рублей. 

Давайте напишем программу, которая будет выводить стоимость километра поездки в зависимости от стоимости поездки и пройденного расстояния.

Что нужно сделать

Сохраните эти данные в две переменные.
Посчитайте, сколько стоил километр вашей поездки. Запишите значение в отдельную переменную.
Выведите стоимость километра поездки на экран.

На экране вы должны были увидеть результат: 33.54838709677419.

Давайте разберём код по порядку.

Шаг 1. Присваиваем новым переменным значения дистанции и стоимости поездки. Не забываем давать переменным информативные названия:

# Переменные

distance = 15.5

price = 520

Шаг 2. Чтобы рассчитать стоимость на километр, делим полную цену на всю дистанцию. Результат записываем в новую переменную km:

# Расчёт стоимости на километр

km = price / distance

Шаг 3. Печатаем результат нашего расчёта, то есть значение переменной km:

# Вывод на экран

print(km)

Дано несколько переменных:

# Переменные для проверки

age = 1006

town = 'Торжок'

beautiful = True

Проверьте, относятся ли они к типу данных «строка». Выведите для каждой переменной результат проверки на экран в таком формате:

Переменная age является строкой False

Переменная town является строкой True

Переменная beautiful является строкой False

Верно!
Отлично. Сверим решения?

print('Переменная age является строкой', isinstance(age, str))

print('Переменная town является строкой', isinstance(town, str))

print('Переменная beautiful является строкой', isinstance(beautiful, str))

Код с проверкой и выводом ответа на экран мог занять всего три строки. Получилось?

Вы на экваторе, осталось ещё три задачи.

Посмотрите вокруг. 

Наверное, вы сидите в помещении (если нет, представьте любое помещение, желательно вам не знакомое, чтобы было интереснее).

Как вы думаете, его площадь больше 15 квадратных метров или меньше? 

Напишите программу, которая будет сравнивать площадь этого помещения с 15 квадратными метрами:

Ширину и длину программа должна запрашивать на ввод с экрана. 
В итоге программа должна выводить на экран вопрос «Ваша комната больше 15 квадратных метров?». 
Затем сама же отвечать на него True или False.

Что было важно учесть в этой задаче:

Запрашивая значения с клавиатуры с помощью input(), программа подсказывает, какое именно значение нужно ввести.
Программа должна приводить введённые с клавиатуры значения к числу. Можно сначала записать в переменную, а потом преобразовать тип. Или наоборот.
Названия переменных осмысленные и не повторяют «зарезервированные» Python’ом имена.
А ещё лучше не называть переменную len, потому что такое название уже есть у функции, с которой мы знакомились в видео.
Наше решение такое:

# Начинаем с ввода данных с экрана, которые сохраняем в переменные

width = input('Введите ширину комнаты ')

length = input('Введите длину комнаты ')

# Вычисляем площадь

area = float(width) * float(length)

# Выводим на экран вопрос

print('Ваша комната больше 15 квадратных метров?')

# Сравниваем полученную площадь с 15 квадратными метрами

print(area > 15)

Итак, пятая задача. Напишите программу, которая запрашивает на ввод какое-то слово или фразу, а потом выводит на печать длину этой фразы.

Решение в три строки (без учёта комментариев) будет таким: 

# Запрашиваем фразу

sentence = input('Введите слово или фразу, а мы выведем её длину ')

# Считаем длину

length = len(sentence)

# Выводим длину фразы на экран

print(length)

А в одну — таким: 

print(len(input('Введите слово или фразу, а мы выведем её длину ')))

Важно, чтобы: 

Запрашивая значения с клавиатуры с помощью input(), программа подсказывала, чего именно ждёт.

Не было переменной по имени len. Лучше не называть так переменную, потому что такое название уже есть у функции, с которой мы знакомились в видео.

И последнее задание. 

Ниже вы видите пять выражений.

Чем можно заменить «?», чтобы программа вывела на экран True?

Важно: менять можно только вопросительный знак.

Перед началом вспомним приоритеты операторов в Python.

В нашем случае первыми будут выполняться арифметические операции (умножение * и целочисленное деление //), а также функции (len(), type() и так далее). 
Затем все логические операции (равно ==, не равно !=, меньше <).
И в конце логическое НЕ (not), логическое И (and) и логическое ИЛИ (or) (в соответствующем порядке).
Операторы на одном уровне приоритета выполняются слева направо.

Теперь разберём выражение

len("Привет") < 14 ? type("Привет") == float
Сначала мы считываем длину строки «Привет» (6). И определяем её тип (str). А затем проверяем, меньше ли наша длина, чем 14 (получаем True). И также проверяем, равен ли наш тип типу float (получаем False). 

Соответственно, нам нужно поставить некоторый знак между True и False, чтобы получить True.

Этим знаком будет OR, потому что именно он возвращает True, если хотя бы одно из значений (операндов) является True.

not ((int(3.5) != 3) ? (3 < 5))

Подсказка: в этой задаче 4 правильных ответа, отметьте их все.

Для этого выражения логика рассуждения может быть такой:

В скобках мы должны получить False, чтобы конечный результат был True (Not превращает правду в ложь, а ложь — в правду).

Подойдут следующие операторы:

AND (False and True — это False)

== 
>
>=

True * 5 ? 5

Здесь важно вспомнить, что арифметическая операция умножения будет выполнена первой, до любой логической операции. Умножая True на 5, получаем 5. И тогда мы должны поставить какой-то знак между 5 и 5, чтобы ответом было True. И это будет, конечно, равно ==, >= или <=.

float(35) ? 77.0

float(35) — это 35.0.
Чтобы получить True, нам надо использовать операторы сравнения, которые делали бы выражение верным, а именно:

35.0 не равно 77.0 (!=);

35.0 меньше 77.0 (<);

35.0 меньше или равно 77.0 (<=).

77 // 2 == 38 and ? False

Давайте разберём по порядку. По приоритету сначала выполняем операции целочисленного деления и проверки на равенство:

77 // 2 — это будет 38 (дробную часть просто отбрасываем).
38 == 38 — получаем True.
Выходит: True and ? False.

Чтобы в результате оператора AND получить True, нужно с обеих сторон иметь True. Превратить False в True можно с помощью логического отрицания NOT. 

Это и есть верный ответ.

[Ноутбук с ответами](https://github.com/UzunDemir/uzundemir.github.io/blob/master/5_8_%D0%A0%D0%B5%D1%88%D0%B5%D0%BD%D0%B8%D0%B5_%D0%BF%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%BE%D0%B3%D0%BE_%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D1%8F.ipynb)
