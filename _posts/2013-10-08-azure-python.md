---
layout: post
title: Подключение к SQL Azure из Python-а (Ubuntu)
description: >
  Описание подключения к SQL Azure из Python-а (Ubuntu)
categories: blog
tags: >
  python azure odbc
social_image: azurepy/azure.png
---

Сначала напишем тестовый скрипт (в Windows такой код без проблем выполняется при условии установленного pyodbc), назовем его test.py.

~~~python
import pyodbc

cnxn = pyodbc.connect(
'DRIVER={SQL Server};
SERVER=db.database.windows.net
;DATABASE=dbname;
UID=user;
PWD=123'
)
cursor = cnxn.cursor()

cursor.execute("select * from users")
row = cursor.fetchone()
if row:
print row
~~~

Данные для подключения только юзаем свои :)

Само собой у нас ничего не запустится, если у нас не стоит pyodbc. Ставим!
Проще всего поставить его через easy_install. Если его тоже нет, то не беда — ставим пакет python-setuptools:

~~~sh
sudo apt-get install python-setuptools
~~~

И теперь ставим pyodbc:

~~~sh
sudo easy_install pyodbc
~~~

Вам понадобятся следующие пакеты:

~~~sh
sudo apt-get install g++
sudo apt-get install unixodbc-dev
sudo apt-get install python-dev
~~~

Теперь можно устанавливать pyodbc:

~~~sh
sudo easy_install pyodbc
~~~

Еще для работы нам нужен FreeTDS:

~~~sh
sudo apt-get install freetds-common freetds-bin tdsodbc
~~~

Формат строки соединения такой:

~~~sh
TDSVER=8.0 tsql -H XXXXXXXX.database.windows.net -U Username -D DatabaseName -p 1433 -P Password
~~~

Добавляем в конфиг FreeTDS наше соединение:

~~~sh
sudo vim /etc/freetds/freetds.conf
~~~

Добавляем туда следующие строки:

~~~sh
[SERVERNAME]
host = xxx.xxx.xxx.xxx
port = 1433
tds version = 8.0
~~~

Пробуем:

~~~sh
tsql -S SERVERNAME -U USERNAME
Password: PASSWORD
~~~

И получаем такую ошибку:

~~~sh
locale is "en_US.UTF-8"
Msg 40531 (severity 11, state 1) from [SERVERNAME] Line 1:
"Server name cannot be determined. It must appear as the first segment of the server's dns name (servername.database.windows.net). Some libraries do not send the server name, in which case the server name must be included as part of the user name (username@servername). In addition, if both formats are used, the server names must match."
Error 20002 (severity 9):
Adaptive Server connection failed
There was a problem connecting to the server
~~~

А, да. Юзера надо именовать как USERNAME@SERVERNAME
Запускаем test.py. Получаем классный еррор:

~~~sh
pyodbc.Error: ('IM002', '[IM002] [unixODBC][Driver Manager]Data source name not found, and no default driver specified (0) (SQLDriverConnect)')
~~~

Смотрим список драйверов:

~~~sh
cat /etc/odbc.ini
~~~

Хм, пустой. Добавляем нужную запись:
~~~sh
vim /etc/odbc.ini
~~~

~~~sh
[ODBC Data Sources]

SQL Server = Microsoft SQL Server
[SQL Server]
Driver = FreeTDS
Description = FreeTDS Driver for Linux
Trace = No
Servername = SERVERNAME
Database = DATABASE
Port = 1433

[Default]
Driver = /usr/lib/x86_64-linux-gnu/odbc/libtdsodbc.so</pre>
А в /etc/odbcinst.ini описываем
<pre class="brush:shell">[FreeTDS]
Driver=/usr/lib/x86_64-linux-gnu/odbc/libtdsodbc.so</pre>
SERVERNAME и DATABASE вставьте свои. И вместо "SQL Server" можно использовать свое название. Ну и драйвер может лежать тут: /usr/lib/odbc/libtdsodbc.so
Пробуем подключиться
<pre class="brush:shell">isql -v "Sql Server" USERNAME@SERVERNAME PASSWORD
~~~

Успех! Меняем в test.py название на драйвера на FreeTDS.
