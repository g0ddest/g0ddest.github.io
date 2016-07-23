---
layout: post
title: Создание мультидоменного SSL сертификата с помощью openssl
description: >
  Описание создания мультидоменного SSL сертификата с помощью openssl
categories: blog
tags: >
  openssl
social_image: openssl/ssl.jpg
---

Для того, чтобы создать мультидоменный запрос на сертификат сначала пишем конфигурционный файл:

~~~
[req]
default_bits = 2048              # Длина ключа в битах.
default_keyfile = private.key    # Имя файла, в который будет записан закрытый ключ.
encrypt_key = yes                # Нам не нужно шифровать закрытый ключ паролем.
default_md = sha512              # Алгоритм хеша.
x509_extensions = v3_req         # Включаем расширение V3.
prompt = no                      # Не нужно запрашивать данные у пользователя, мы всё пропишем здесь.
distinguished_name = req_distinguished_name
req_extensions = v3_req

[req_distinguished_name]
CN = www.example.com
O = Organisation Ltd.     # Компания
L = New York              # Город
C = US                    # Страна
ST = New Jersey           # штат
emailAddress = admin@example.com

[v3_req]
# Список альтернативных имён. Можно указать прямо здесь, но это не
# удобно, особенно если их много, так что мы указываем название секции
# с именами.
subjectAltName = @alt_names

[alt_names]
# Имена. Можно указать хоть сколько, главное чтобы цифры после точки были разными.
DNS.0 = example.com
DNS.1 = www.example.com
~~~

Сохраняем как файл cert.conf. Затем запускаем генерацию файла запроса сертификата CSR:

~~~sh
openssl req -new -config cert.conf -out cert.csr
~~~

В итоге после ввода пароля у нас должно получиться 2 файла: cert.csr и private.key. Первый файл и есть необходимый нам запрос сертификата, второй файл приватный ключ.

Если есть необходимость создать самоподписной сертификат, то с помощью 1 команды

~~~sh
openssl x509 -req -in cert.csr -signkey private.key -out cert.crt
~~~

Если необходимо просмотреть что получилось внутри файла CSR, то можно воспользоваться онлайн-декодером: [http://redkestrel.co.uk/products/decoder/](http://redkestrel.co.uk/products/decoder/) , либо же воспользоваться онлайн ASN.1 декодером: [http://lapo.it/asn1js/](http://lapo.it/asn1js/)
