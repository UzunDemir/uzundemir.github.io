---
layout: post
title:  "Визуализации алгоритмов сортировки"
---

Алгоритмы сортировки упорядочивают элементы по определённому критерию. В день появления первых компьютерных вычислений появился и повод для споров между исследователями. Причиной разногласий послужил поиск эффективных способов решения не только для общих задач, но и для сложных проблем. К таким задачам и проблемам также относится тема сортировки элементов по определённому критерию.

Визуализация алгоритмов сортировки служит для того, чтобы понять, как работают одни из самых известных алгоритмов сортировки.

Пирамидальная сортировка (Heapsort)
Heapsort — алгоритм, в основе которого лежит сравнение.

Алгоритм пирамидальной сортировки можно рассматривать как улучшенную версию алгоритма сортировки выбором: он делит входные данные на отсортированную и несортированную области, а затем последовательно уменьшает несортированную область, извлекая самый большой элемент и перемещая его в сортированную область. Улучшение состоит в том, что используется бинарная куча, а не алгоритм линейного поиска, чтобы найти наибольшее значение.

Пирамидальная сортировка является одним из методов, быстродействие которых оценивается как O(n log n).

![Image alt]({{ site.baseurl }}//images/Heap-Sort.gif)

Больше визуализаций алгоритмов сортировок - [здесь.](https://tproger.ru/digest/sorting-algorithms-visualized/)

