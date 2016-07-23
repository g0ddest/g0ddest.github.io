---
layout: post
title: Управление PCsensor FS1_P USB foot switch из C#
description: >
  Управление PCsensor FS1_P USB foot switch из C#
categories: blog
tags: >
  C#
social_image: footswitch/pedal.jpg
---

{%
	include media-image.html
	url="footswitch/pedal.jpg"
	caption="Вот так выглядит PCsensor FS1_P"
	noborder=""
%}

Недавно стала задача управлять софтом, написанным на C# аналоговой педалью. Выбрана была модель PCsensor FS1_P (VID: 0x0C45 PID:0 x7403), однако к ней кроме поставляемой софтины не было никакой спецификации.

Для работы с USB была выбрана библиотека [LibUsbDotNet](http://libusbdotnet.sourceforge.net/V2/Index.html) (требует установки отдельного device filter).

После ряда экспериментов была выявлена длина возвращаемого сигнала и сам сигнал:

~~~
15000000
20000000
~~~

на нажатие педали и на возвращение в первоначальное состояние

~~~
20000000
10000000
~~~

Была написана небольшая библиотека, возвращающая 2 события: onPress и onRelease на нажатие и на возвращение педали в первоначальное состояние соответственно.

Пример использования:

~~~cs
var footswtich = new FootswitchListener();

try
{
     footswtich.StartListen();

     footswtich.Press += () => Console.WriteLine("Pressed button");
     footswtich.Release += () => Console.WriteLine("Release button");

catch (Exception fe)
{
     Console.WriteLine(fe.Message);
}
~~~

Просмотреть [код на github](https://github.com/g0ddest/footswitch)
