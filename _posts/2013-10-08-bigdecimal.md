---
layout: post
title: BigDecimal, Java
description: >
  Описание класса BigDecimal в Java
categories: blog
tags: >
  java
social_image: bigdecimal/bigdecimal.gif
---

BigDecimal — расширенный класс для различных арифметических операций из стандартной поставки Java.

## Как он устроен?
Объект типа BigDecimal хранит в себе два целочисленных значения:

* Biglnteger мантиссу вещественного числа
* int десятичный порядок числа

Таким образом число 123.45 представляется в виде объектов Biglnteger( 12345 ) и int( 2 ).

## Создание объекта

Класс BigDecimal имеет следующие конструкторы:

* BigDecimal (Biglnteger val) — объект будет хранить большое целое val, порядок равен нулю;
* BigDecimal (String val) и BigDecimal (char[] in) — число задается строкой символов ##val## (последовательностью cимволов ##in##), которая должна содержать запись числа по правилам языка Java, то есть: положительное или отрицательное число определяется '+' ( '\u002B') or '-' ('\u002D'); экспонента определяется знаками 'e' ('\u0065'), или 'E' ('\u0045'), то есть число -1234.5E-4 будет представлен мантиссой -12345 и порядком 5.
* BigDecimal (int number) (аналогично для long, double) — конвертация из числового типа в тип BigDecimal;

##Перевод в другие типы

Определены следующие методы doubleValue(), floatValue(), intValue(), longValue(), toBiginteger(). Комментарии излишни.

## Арифметические операции

* abs() — абсолютное значение объекта this;
* add(BigDecimal x) — операция сложения this + х ;
* compareTo(BigDecimal val) — операция сравнения this и val. Возвращает -1, 0 и 1, если this меньше, равно, или больше val соответственно.
* divide(BigDecimal х, RoundingMode r) — операция деления this / х с округлением по способу r ;
* divide(BigDecimal х, n, RoundingMode r) — операция деления this / х с изменением порядка и округлением по способу r;
* mах(BigDecimal х) — наибольшее из this и х;
* min(BigDecimal x) — наименьшее из this и х;
* movePointLeft(int n) — сдвиг влево на n разрядов;
* movePointRight(int n) — сдвиг вправо на n разрядов;
* multiply(BigDecimal х) — операция умножения this * х ;
* negate() — возвращает объект с обратным знаком;
* scale() — возвращает порядок числа;
* setscale(int n) — устанавливает новый порядок n ;
* setscale(int n, RoundingMode r) — устанавливает новый порядок n и округляет число при необходимости по способу r ;
* signum() — int знак числа, хранящегося в объекте, возвращает -1, 0, 1 для отрицательных, нулевых и положительных значений соответственно;
* subtract(BigDecimal х) — операция вычитания this - х ;
* unscaiedvalue() —возвращает мантиссу числа.

## Типы округлений

RoundingMode имеет следующие значения:

* ROUND_CEILING — округление в сторону большего целого;
* ROUND_DOWN — округление к нулю, к меньшему по модулю целому значению;
* ROUND_FLOOR — округление к меньшему целому;
* ROUND_HALF_DOWN — округление к ближайшему целому, среднее значение округляется к меньшему целому;
* ROUND_HALF_EVEN — округление к ближайшему целому, среднее значение округляется к четному числу;
* ROUND_HALF_UP — округление к ближайшему целому, среднее значение округляется к большему целому;
* ROUND_UNNECESSARY — предполагается, что результат будет целым, и округление не понадобится;
* ROUND_UP — округление от нуля, к большему по модулю целому значению.
