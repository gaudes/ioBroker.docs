---
translatedFrom: en
translatedWarning: Если вы хотите отредактировать этот документ, удалите поле «translationFrom», в противном случае этот документ будет снова автоматически переведен
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/ru/adapterref/iobroker.modbus/README.md
title: iobroker.modbus
hash: 7C8JiWKGGPkBIOH/OwE5jSPJtbtE9l8IuyAwz3khMYc=
---
![логотип](../../../en/adapterref/iobroker.modbus/admin/modbus.png)

![Количество установок](http://iobroker.live/badges/modbus-stable.svg)
![Версия NPM](http://img.shields.io/npm/v/iobroker.modbus.svg)
![Загрузки](https://img.shields.io/npm/dm/iobroker.modbus.svg)
![NPM](https://nodei.co/npm/iobroker.modbus.png?downloads=true)

# Iobroker.modbus =====================
Внедрение ModBus Slave и Master для ioBroker. Поддерживаются следующие типы:

- Modbus RTU через последовательный интерфейс (мастер)
- Modbus RTU через TCP (мастер)
- Modbus TCP (ведомый, ведущий)

## Настройки
### IP-адрес партнера
IP-адрес партнера Modbus.

### Порт
Порт TCP партнера Modbus, если он настроен как главный (клиент), или собственный порт, если он настроен как подчиненный (сервер).

### Идентификатор устройства
Идентификатор устройства Modbus. Важно, если используется мост TCP / Modbus.

### Тип
Ведомый (Сервер) или Ведущий (Клиент).

### Использовать псевдонимы в качестве адреса
Обычно все регистры могут иметь адрес от 0 до 65535. Используя псевдонимы, вы можете определить поля виртуальных адресов для каждого типа регистров. Обычно:

- дискретные входы от 10001 до 20000
- катушки от 1 до 1000
- входные регистры от 30001 до 40000
- регистры хранения от 40001 до 60000

Каждый псевдоним будет сопоставлен с внутренним адресом, например, 30011 будет сопоставлен с входным регистром 10. и так далее.

### Не выравнивать адреса по слову
Обычно адреса катушек и дискретных входов соответствуют 16 битам. Подобные адреса от 3 до 20 будут выровнены от 0 до 32.
Если эта опция активна, адреса не будут выровнены.

### Раунд Реал в
Сколько цифр после запятой для float и double.

### Задержка опроса
Интервал циклического опроса (актуально только для мастера)

### Время переподключения
Интервал переподключения (актуально только для мастера)

### Время пульса
если импульс используется для катушек, это определяет интервал длительности импульса.

### Максимальная длина запроса на чтение
Максимальная длина команды READ_MULTIPLE_REGISTERS как количество регистров для чтения.

Некоторые системы требуют сначала «запрос на запись» для доставки данных по «запросу на чтение».
Вы можете включить этот режим, установив для параметра «Максимальная длина запроса на чтение» значение 1.

** Примечание: ** Некоторые решения USB Modbus (например, основанные на socat) могут иметь проблемы при работе с модулем serialport npm.

Существует программный шлюз [** Modbus RTU <-> Modbus RTU через TCP **](http://mbus.sourceforge.net/index.html), позволяющий использовать последовательный протокол RTU по протоколу TCP.

Оба решения **RTU через TCP** и **TCP** работают хорошо.

## Типы данных
- uint16be - 16-разрядный без знака (Big Endian): AABB => AABB
- uint16le - 16-разрядный без знака (Little Endian): AABB => BBAA
- int16be - 16-битный (Big Endian) со знаком: AABB => AABB
- int16le - подписано 16 бит (Little Endian): AABB => BBAA
- uint32be - 32-разрядный без знака (Big Endian): AABBCCDD => AABBCCDD
- uint32le - 32-разрядный без знака (Little Endian): AABBCCDD => DDCCBBAA
- uint32sw - 32-разрядный без знака (подстановка слова с прямым порядком байтов): AABBCCDD => CCDDAABB
- uint32sb - 32-разрядный без знака (перестановка байтов с прямым порядком байтов): AABBCCDD => DDCCBBAA
- int32be - 32-разрядный со знаком (Big Endian): AABBCCDD => AABBCCDD
- int32le - 32-разрядный со знаком (Little Endian): ABBCCDD => DDCCBBAA
- int32sw - 32-разрядный со знаком (перестановка слов с прямым порядком байтов): AABBCCDD => CCDDAABB
- int32sb - 32-разрядный со знаком (байт с прямым порядком байтов): AABBCCDD => DDCCBBAA
- uint64be - 64-разрядный без знака (Big Endian): AABBCCDDEEFFGGHH => AABBCCDDEEFFGGHH
- uint64le - 64-разрядная без знака (Little Endian): AABBCCDDEEFFGGHH => HHGGFFEEDDCCBBAA
- uint8be - 8-битный без знака (Big Endian): AA => AA
- uint8le - 8-битный без знака (Little Endian): AA => AA
- int8be - 8-значный со знаком (Big Endian): AA => AA
- int8le - 8 бит со знаком (Little Endian): AA => AA
- floatbe - Float (Big Endian): AABBCCDD => AABBCCDD
- floatle - Float (Little Endian): AABBCCDD => DDCCBBAA
- floatsw - Float (своп из Big Endian Word): AABBCCDD => CCDDAABB
- floatsb - Float (байт с прямым порядком байтов): AABBCCDD => DDCCBBAA
- doublebe - Double (Big Endian): AABBCCDDEEFFGGHH => AABBCCDDEEFFGGHH
- doublele - Double (Little Endian): AABBCCDDEEFFGGHH => HHGGFFEEDDCCBBAA
- строка - строка (нулевой конец): ABCDEF \ 0 => ABCDEF \ 0
- stringle - String (Little Endian, Zero-end): BADCFE \ 0 => ABCDEF \ 0

Следующее описание было скопировано из [Вот](http://www.chipkin.com/how-real-floating-point-and-32-bit-data-is-encoded-in-modbus-rtu-messages/)

Двухточечный протокол Modbus является популярным выбором для связи RTU, если по какой-либо другой причине это основное удобство. Сам протокол контролирует взаимодействие каждого устройства в сети Modbus, как устройство устанавливает известный адрес, как каждое устройство распознает свои сообщения и как основная информация извлекается из данных. По сути, протокол является основой всей сети Modbus.

Однако такое удобство не обходится без каких-либо сложностей, и протокол Modbus RTU Message не является исключением. Сам протокол был разработан на основе устройств с длиной 16-битного регистра. Следовательно, при реализации 32-битных элементов данных требовались особые соображения. Эта реализация остановилась на использовании двух последовательных 16-битных регистров для представления 32 бит данных или, по существу, 4 байтов данных. Именно в этих 4 байтах данных данные с плавающей запятой одинарной точности могут быть закодированы в сообщение Modbus RTU.

### Важность порядка байтов
Сам Modbus не определяет тип данных с плавающей запятой, но широко распространено мнение, что он реализует 32-битные данные с плавающей запятой, используя стандарт IEEE-754. Однако в стандарте IEEE нет четкого определения порядка байтов полезных данных. Поэтому наиболее важным фактором при работе с 32-битными данными является то, что данные адресуются в правильном порядке.

Например, число 123 / 456.00, как определено в стандарте IEEE 754 для 32-разрядных чисел с плавающей запятой одинарной точности, выглядит следующим образом:

![Изображение1](../../../en/adapterref/iobroker.modbus/img/img1.png)

Влияние различных порядков байтов является значительным. Например, упорядочение 4 байтов данных, которые представляют 123456,00 в последовательности «B A D C», известной как «перестановка байтов». Когда интерпретируется как тип данных с плавающей запятой IEEE 744, результат будет совсем другим:

![Image2](../../../en/adapterref/iobroker.modbus/img/img2.png)

Упорядочение тех же байтов в последовательности «C D A B» называется «перестановка слов». Опять же, результаты резко отличаются от первоначального значения 123456,00:

![Image3](../../../en/adapterref/iobroker.modbus/img/img3.png)

Кроме того, и «замена байтов», и «замена слов» по существу полностью изменили бы последовательность байтов, чтобы получить еще один результат:

![image4](../../../en/adapterref/iobroker.modbus/img/img4.png)

Ясно, что при использовании сетевых протоколов, таких как Modbus, необходимо строго следить за тем, как байты памяти упорядочиваются при их передаче, также известной как «порядок следования байтов».

### Определение порядка байтов
Сам протокол Modbus объявлен как протокол "big-Endian" в соответствии со спецификацией протокола приложений Modbus, V1.1.b:

```Modbus uses a “big-Endian” representation for addresses and data items. This means that when a numerical quantity larger than a single byte is transmitted, the most significant byte is sent first.```

Big-Endian является наиболее часто используемым форматом для сетевых протоколов, настолько распространенным, что его также называют «сетевым порядком».

Учитывая, что протокол сообщения Modbus RTU является байтовым порядком байтов, для успешного обмена 32-битным типом данных через сообщение Modbus RTU необходимо учитывать порядок байтов как главного, так и подчиненного устройств. Многие ведущие и ведомые устройства RTU допускают конкретный выбор порядка байтов, особенно в случае программно-имитируемых модулей. Нужно просто убедиться, что оба устройства имеют одинаковый порядок байтов.

Как правило, семейство микропроцессора устройства определяет его порядковый номер. Как правило, стиль с прямым порядком байтов (старший байт сохраняется первым, а затем младший байт) обычно встречается в процессорах, разработанных с процессором Motorola. Стиль с прямым порядком байтов (младший байт сохраняется первым, а затем старший байт) обычно встречается в процессорах, использующих архитектуру Intel. Это вопрос личной точки зрения относительно того, какой стиль считается «задом наперед».

Однако, если порядок байтов и порядок байтов не являются настраиваемым параметром, вам придется определить, как интерпретировать байт. Это можно сделать, запросив известное значение с плавающей точкой от ведомого. Если возвращается невозможное значение, то есть число с двузначным показателем степени или около того, порядок байтов, скорее всего, нуждается в модификации.

### Практическая помощь
Драйверы FieldServer Modbus RTU предлагают несколько перемещений функций, которые обрабатывают 32-разрядные целые и 32-разрядные значения с плавающей запятой. Что еще более важно, эти перемещения функций учитывают все различные формы последовательности байтов. В следующей таблице показано перемещение функции FieldServer, которая копирует два смежных 16-разрядных регистра в 32-разрядное целочисленное значение.

| Функциональное ключевое слово | Режим обмена | Исходные байты | Целевые байты |
|-------------------|--------------------|-----------------|--------------|
| 2.i16-1.i32 | N / A | [a b] [c d] | [a b c d] |
| 2.i16-1.i32-s | байт и перестановка слов | [a b] [c d] | [d c b a] |
| 2.i16-1.i32-sb | обмен байтов | [a b] [c d] | [b a d c] |
| 2.i16-1.i32-sw | обмен словами | [a b] [c d] | [c d a b] |

В следующей таблице показаны функции FieldServer, которые копируют два смежных 16-разрядных регистра в 32-разрядное значение с плавающей запятой:

| Функциональное ключевое слово | Режим обмена | Исходные байты | Целевые байты |
|-------------------|--------------------|-----------------|--------------|
| 2.i16-1.ifloat | N / A | [a b] [c d] | [a b c d] |
| 2.i16-1.ifloat-s | байт и перестановка слов | [a b] [c d] | [d c b a] |
| 2.i16-1.ifloat-sb | обмен байтов | [a b] [c d] | [b a d c] |
| 2.i16-1.ifloat-sw | обмен словами | [a b] [c d] | [c d a b] |

В следующей таблице показаны перемещения функции FieldServer, которые копируют одно 32-разрядное значение с плавающей запятой в два смежных 16-разрядных регистра:

| Функциональное ключевое слово | Режим обмена | Исходные байты | Целевые байты |
|------------------|-------------------|-----------------|----------------|
| 1.float-2.i16 | N / A | [a b] [c d] | [a b] [c d] |
| 1.float-2.i16-s | байт и перестановка слов | [a b] [c d] | [d c] [b a] |
| 1.float-2.i16-sb | перестановка байтов | [a b] [c d] | [b a] [d c] |
| 1.float-2.i16-sw | обмен словами | [a b] [c d] | [c d] [a b] |

Учитывая различные перемещения функции FieldServer, правильная обработка 32-битных данных зависит от выбора правильной. Обратите внимание на следующее поведение этих функций FieldServer при использовании известного десятичного значения с плавающей запятой одинарной точности 123456,00:

| 16-битные значения | Функция Переместить | Результат | Функция Переместить | Результат |
|---------------|-------------------|-----------|-------------------|---------------|
| 0x2000 0x47F1 | 2.i16-1.float | 123456,00 | 1.float-2.i16 | 0x2000 0x47F1 |
| 0xF147 0x0020 | 2.i16-1.float-s | 123456,00 | 1.float-2.i16-s | 0xF147 0X0020 |
| 0x0020 0xF147 | 2.i16-1.float-сб | 123456,00 | 1.float-2.i16-сб | 0x0020 0xF147 |
| 0x47F1 0x2000 | 2.i16-1.float-SW | 123456,00 | 1.float-2.i16-SW | 0x47F1 0x2000 |

Обратите внимание, что различные порядки байтов и слов требуют использования соответствующей функции перемещения FieldServer. После выбора правильного перемещения данных данные могут быть преобразованы в обоих направлениях.

Из многих доступных в Интернете преобразователей и калькуляторов с шестнадцатеричным числом с плавающей запятой лишь немногие фактически разрешают манипулировать порядками байтов и слов. Одна из таких утилит находится по адресу www.61131.com/download.htm, где можно загрузить версии утилит для Linux и Windows. После установки утилита запускается как исполняемый файл с единым диалоговым интерфейсом. Утилита представляет десятичное значение с плавающей точкой 123456,00 следующим образом:

![Image5](../../../en/adapterref/iobroker.modbus/img/img5.png)

Затем можно поменять местами байты и / или слова, чтобы проанализировать, какие потенциальные проблемы могут возникать между главным и подчиненным устройствами Modbus RTU.

## Тестовое задание
В папке * test 'есть несколько программ для проверки связи TCP:

- Ananas32 / 64 является ведомым симулятором (содержит только регистры и входы, без катушек и цифровых входов)
- RMMS мастер симулятор
- mod_RSsim.exe является ведомым симулятором. Возможно, вам потребуется [распространяемый пакет Microsoft Visual C ++ 2008 SP1] (https://www.microsoft.com/en-us/download/details.aspx?id=5582) для его запуска (из-за ошибки SideBySide).

# 2.0.9 (2018-10-11)
* (Bjoern3003) Запись регистров была исправлена

# 2.0.7 (2018-07-02)
* (bluefox) Режим сервера был исправлен

# 2.0.6 (2018-06-26)
* (bluefox) исправлен основной режим rtu-tcp

# 2.0.3 (2018-06-16)
* (bluefox) Исправлено округление чисел

# 2.0.2 (2018-06-12)
* (bluefox) Исправлена ошибка с чтением блоков
* (bluefox) Реализовано чтение блока для дискретных значений

# 2.0.1 (2018-05-06)
* (bluefox) Добавлена поддержка нескольких идентификаторов устройств

# 1.1.1 (2018-04-15)
* (Apollon77) Оптимизация обработки переподключения

# 1.1.0 (2018-01-23)
* (bluefox) Добавлены строки с прямым порядком байтов
* (Apollon77) Обновление библиотеки Serialport

# 1.0.2 (2018-01-20)
* (bluefox) Исправлено чтение катушек

# 0.5.4 (2017-09-27)
* (Apollon77) Несколько исправлений

# 0.5.0 (2017-02-11)
* (bluefox) Создание всех состояний друг за другом

# 0.4.10 (2017-02-10)
* (Apollon77) Не воссоздайте все точки данных при запуске адаптера
* (ykuendig) Несколько исправлений оптимизации и формулировок

# 0.4.9 (2016-12-20)
* (bluefox) исправление серийного RTU

# 0.4.8 (2016-12-15)
* (Apollon77) обновление библиотеки serialport для совместимости с узлом 6.x

# 0.4.7 (2016-11-27)
* (bluefox) Использовать старую версию jsmodbus

# 0.4.6 (2016-11-08)
* (bluefox) обратная совместимость с 0.3.x

# 0.4.5 (2016-10-25)
* (bluefox) лучшая обработка буфера на tcp и serial

# 0.4.4 (2016-10-21)
* (bluefox) Исправлена запись в регистры хранения.

# 0.4.1 (2016-10-19)
* (bluefox) Поддержка RTU ModBus через последовательный порт и через TCP (только подчиненный)

# 0.3.11 (2016-08-18)
* (Apollon77) Исправлено неверное количество байтов в цикле

# 0.3.10 (2016-02-01)
* (bluefox) исправляет потерянные настройки истории.

# 0.3.9 (2015-11-09)
* (bluefox) Всегда используйте write_multiple_registers при записи удерживающих регистров.

# 0.3.7 (2015-11-02)
* (bluefox) добавляет специальный режим чтения / записи, если «Максимальная длина запроса на чтение» равна 1.

# 0.3.6 (2015-11-01)
* (bluefox) добавить циклическую запись для хранения регистров (исправлено)

# 0.3.5 (2015-10-31)
* (bluefox) добавить циклическую запись для хранения регистров

# 0.3.4 (2015-10-28)
* (Bluefox) добавить двойные и исправить uint64

# 0.3.3 (2015-10-27)
* (bluefox) исправление регистров хранения

# 0.3.2 (2015-10-27)
* (bluefox) исправление импорта из текстового файла

# 0.3.1 (2015-10-26)
* (bluefox) исправлена ошибка с длиной блока чтения (мастер)
* (bluefox) поддержка блоков чтения и максимальной длины запроса на чтение (master)
* (bluefox) может определять поля при импорте

# 0.3.0 (2015-10-24)
* (bluefox) добавить настройки раунда
* (bluefox) добавить идентификатор устройства
* (bluefox) slave поддерживает числа с плавающей запятой, целые числа и строки

# 0.2.6 (2015-10-22)
* (bluefox) добавляет различные типы для inputRegisters и для хранения регистров ТОЛЬКО ДЛЯ MASTER

# 0.2.5 (2015-10-20)
* (bluefox) исправляет имена объектов, если используются псевдонимы

# 0.2.4 (2015-10-19)
* (bluefox) исправить ошибку добавить новые значения

# 0.2.3 (2015-10-15)
* (bluefox) исправить ошибку с мастером

# 0.2.2 (2015-10-14)
* (bluefox) навесное оборудование
* (bluefox) изменить модель адресации

# 0.0.1
* (bluefox) начальная фиксация

## Changelog