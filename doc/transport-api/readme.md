#API прямого обмена данными с банком



API обмена данными – уровень, который описывает подходы и методы осуществления операций обмена данными, он в свою очередь делится на следующие уровни:

- Уровень аутентификации

- Настройка обмена

- Транспортный уровень



- - -

+ [Порядок взаимодействия на уровне аутентификации](#1)

 + [Аутентификация по логину и паролю](#1.2)

   + [Метод **Logon** (HTTP-метод POST)](#1.2.1)

    + [Метод **LogonOTP** (HTTP-метод POST)](#1.2.2)

 + [Рекомендации для банковского сервиса](#1.3)

+ [Настройка обмена электронными документами](#0)

 + [Получение настроек обмена с банком в автоматическом режиме](#2.2)

   + [Метод **GetSettings** (HTTP-метод POST)](#2.2.1)

+ [Порядок взаимодействия на транспортном уровне](#2)

 + [Формирование и разбор транспортного контейнера](#2.1)

 + [Отправка электронных документов из 1С](#2.3)

   + [Метод **SendPack** (HTTP-метод POST)](#2.3.1)

 + [Получение электронных документов в 1С](#2.4)

   + [Метод **GetPackList** (HTTP-метод GET)](#2.4.1)

    + [Метод **GetPack** (HTTP-метод GET)](#2.4.2)

 + [Процедура синхронизации 1С с банковским сервисом](#2.5)

+ [Обеспечение безопасности данных](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)

 + [Шифрование данных при передаче](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Шифрование-данных-при-передаче)

 + [Криптооперации, выполняемые на стороне Клиента и Банка](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Криптооперации-выполняемые-на-стороне-Клиента-и-Банка)

 + [Защита электронных документов с помощью электронной подписи](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Защита-электронных-документов-с-помощью-электронной-подписи)

+ [Схемы данных](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md)

+ [Классификаторы](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md)

 + [Таблица кодов статусов транспортных контейнеров](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md#-Таблица-кодов-статусов-транспортных-контейнеров)

 + [Таблица кодов видов электронных документов](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md#-Таблица-кодов-видов-электронных-документов)

 + [Таблица кодов статусов электронных документов](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md#-Таблица-кодов-статусов-электронных-документов)

 + [Таблица типов выписок банка](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md#-Таблица-типов-выписок-банка)

 + [Таблица кодов ошибок и их описание, которые может возвращать банковский сервис в «1С:Предприятие 8»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md#-Таблица-кодов-ошибок-и-их-описание-которые-может-возвращать-банковский-сервис-в-1СПредприятие-8)

+ [Описание типов](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md)



- - - -


## <a name="1"></a> Порядок взаимодействия на уровне аутентификации.



Аутентификация Клиента с целью создания сессии на сервисе Банка может проходить по логину и паролю + дополнительный OTP («one time password», опционально) ([Logon](#1.2.1)).


### <a name="1.2"></a> Аутентификация по логину и паролю



Система «1С:Предприятия 8» передает в Банк уникальный идентификатор Клиента в банковской системе (строка может содержать только ANSI-символы в соответствии с [RFC 2616](http://tools.ietf.org/html/rfc2616) для передачи значений в HTTP-заголовке) и строку, содержащую «логин + пароль» (строка «Basic логин:пароль» в формате BASE64 в соответствии с [Basic access authentication](http://tools.ietf.org/html/rfc2617)) (отправка производится HTTP-методом POST - метод [Logon](#1.2.1)).

Особенностью данного процесса является то, что в момент первой аутентификации уникальный идентификатор Клиента неизвестен, тогда следует передавать «0».

В случае использования однофакторной аутентификации на стороне Банка и успешного завершения процесса сервис возвращает идентификатор сессии («маркер») в «1С:Предприятия 8» в синхронном режиме (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)).



![Аутентификация по логину и паролю. Logon](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/Logon.png)



В случае использования двухфакторной аутентификации на стороне Банка (применение OTP – «one time password») и успешного начала процесса аутентификации сервис в синхронном режиме возвращает в «1С:Предприятия 8» идентификатор сессии, пока еще неавторизованной на стороне Банка, и признак, что требуется расширенная аутентификация (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)), а также направляет OTP на мобильный телефон Клиента.



«1С:Предприятии 8» выводит форму с текстом, содержащим идентификатор неавторизованной сессии и, возможно, маску мобильного телефона Клиента (если Банк ее прислал), и предлагает пользователю ввести OTP.



Система «1С:Предприятия 8» передает в Банк уникальный идентификатор Клиента в банковской системе и введенный OTP (отправка производится HTTP-методом POST - метод LogonOTP). В случае успешного завершения процесса банковский сервис возвращает идентификатор авторизованной сессии в «1С:Предприятия 8» в синхронном режиме (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)).

Особенностью данного процесса является то, что в момент первой аутентификации уникальный идентификатор Клиента неизвестен, тогда следует передавать «0».

В дальнейшем, идентификатор авторизованной сессии будет указан в каждом HTTP-запросе для обмена данными на транспортном уровне (в заголовке SID).



Результатом неуспешной аутентификации должна быть ошибка, возвращаемая банковской системой в синхронном режиме (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)).



![Аутентификация по логину и паролю. LogonOTP](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/LogonOTP.png)



## <a name="1.2.1"></a> Метод Logon (HTTP-метод POST) 

Аутентификация по логину и паролю, однофакторная аутентификация



**Заголовки:**

- Host: <Адрес ресурса банка>

- Content-Type: application/xml; charset=utf-8

- CustomerID: <Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен>

- Authorization: Basic <логин:пароль>

- APIVersion: <Версия API обмена данными> 

- AvailableAPIVersion: <Доступная версия API обмена данными>



**Тело запроса:**

- ПУСТО





**Успешный ответ:**

- HTTP/1.1 200 OK

- Content-Type: application/xml; charset=utf-8

- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)>





**Параметры запроса:**



| Параметр      | Тип               | Кратность | Описание                                                         |
|---------------|-------------------|:---------:|------------------------------------------------------------------|
| Host          | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| CustomerID    | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен |
| Authorization | [base64Binary](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#base64Binary)      | [1]       | «логин + пароль» в соответствии с [Basic access authentication](http://tools.ietf.org/html/rfc2617)    |
| APIVersion    | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |
| AvailableAPIVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) | [0-1]       | Максимальная доступная для текущей информационной базы версия API обмена данными                                                                                                                  |



**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |





**Пример запроса** аутентификации по **логину и паролю**:

```http

POST https://dbogate.demobank.ru/Logon HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
CustomerID: 502036
Authorization: Basic NjY5NzcxNDczMTo5MzcyMjkxMzIx
APIVersion: 2.1.1
AvailableAPIVersion: 2.1.1
User-Agent: 1C+Enterprise/8.3
Content-Type: application/xml; charset=utf-8
Content-Length: 0

```

**Пример успешного ответа** на запрос аутентификации по **логину и паролю**:

```xml

HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 145

<?xml version="1.0" encoding="UTF-8"?>
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.1.1">
    <Success>
        <LogonResponse>
			<SID>8867755b6fbb4ae296aa0ac6b179ae88</SID>
        </LogonResponse>
    </Success>
</ResultBank>

```



**Пример успешного ответа** на запрос аутентификации по **логину и паролю + ОТР**:

```xml

HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 176

<?xml version="1.0" encoding="UTF-8"?>
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.1.1">
    <Success>
        <LogonResponse>
            <SID>7767755b6fbb4ae296aa0ac6b179aef9</SID>
            <ExtraAuth>
            	<OTP phoneMask="7916***6465" />
            </ExtraAuth>
        </LogonResponse>
    </Success>
</ResultBank>

```

## <a name="1.2.2"></a> Метод LogonOTP (HTTP-метод POST)

Аутентификация по логину и паролю, двухфакторная аутентификация



**Заголовки:**

- Host: <Адрес ресурса банка>

- Content-Type: application/xml; charset=utf-8

- CustomerID: <Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен>

- SID: <Идентификатор неавторизованной сессии>

- OTP: <Пользовательский OTP>

- APIVersion: <Версия API обмена данными>



**Тело запроса:**

- ПУСТО





**Успешный ответ:**

- HTTP/1.1 200 OK

- Content-Type: application/xml; charset=utf-8

- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)>



**Параметры запроса:**



| Параметр   | Тип               | Кратность | Описание                                                         |
|------------|-------------------|:---------:|------------------------------------------------------------------|
| Host       | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| SID        | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            | [1]       | Идентификатор неавторизованной сессии                            |
| CustomerID | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен |
| OTP        | [int](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#int)               | [1]       | Пользовательский OTP - целые числа 6-7 знаков                    |
| APIVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |



**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |





**Пример запроса** передачи **OTP**:



```http

POST https://dbogate.demobank.ru/LogonOTP HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
SID: 7767755b6fbb4ae296aa0ac6b179aef9
CustomerID: 502036
OTP: 034494
APIVersion: 2.1.1
User-Agent: 1C+Enterprise/8.3
Content-Type: application/xml; charset=utf-8
Content-Length: 0

```



**Пример успешного ответа** на запрос передачи **OTP**:

```xml

HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 145

<?xml version="1.0" encoding="UTF-8"?>
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.1.1">
    <Success>
        <LogonResponse>
			<SID>8867755b6fbb4ae296aa0ac6b179ae88</SID>
        </LogonResponse>
    </Success>
</ResultBank>

```



### <a name="1.3"></a> Рекомендации для банковского сервиса



Время жизни авторизованной сессии на стороне банк.сервиса рекомендуется устанавливать 5 минут, а при получении новых запросов из «1С:Предприятия 8» – автоматически его продлевать.



В случае обнаружения 3-х подряд неверных попыток аутентификации на банковском сервисе в течение 30 секунд с одного IP-адреса рекомендуется отклонять все последующие попытки аутентификации с этого IP-адреса в течение 60 секунд.



В случае обнаружения 10-ти подряд неверных попыток аутентификации с одного IP-адреса в течение 120 секунд банковскому сервису рекомендуется отклонять все последующие попытки аутентификации с этого IP-адреса в течение 240 секунд, а также проинформировать клиента банка о возникшей ситуации альтернативными способами связи (SMS-оповещением и/или письмом по эл.почте).



При отклонении в систему «1С:Предприятие 8» банковский сервис должен в синхронном режиме возвращать соответствующую ошибку (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)).



## <a name="0"></a> Настройка обмена электронными документами

Для начала использования прямого обмена электронными документами из решений  «1С:Предприятие 8» с банковской системой Клиент должен получить параметры обмена данными из Банка – в «1С:Предприятии 8» будут автоматически созданы настройки ЭДО с Банком.


### <a name="2.2"></a> Получение настроек обмена с банком в автоматическом режиме



Получение настроек обмена с банком в 1С выполняется в 3 этапа:

- аутентификация Клиента на стороне Банка (если нет ранее открытой сессии);

- передача в банк расчетного счета Клиента, банк по этому номеру формирует файл настроек и отправляет обратно;

- обработка ответа банка.



## <a name="2.2.1"></a> Метод  GetSettings (HTTP-метод POST)



**Заголовки:**

- Host: <Адрес ресурса банка>

- Content-Type: application/xml; charset=utf-8

- CustomerID: <Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен>

- Account: <Номер расчетного счета Клиента, для зарплатных проектов может отсутствовать>

- Inn: <Инн организации, для которой запрашиваются настройки>

- Bic: <БИК Банка>

- SID: <Идентификатор авторизованной сессии>

- APIVersion: <Версия API обмена данными>

- AvailableAPIVersion: <Доступная версия API обмена данными>



**Тело запроса:**

- ПУСТО





**Успешный ответ:**

- HTTP/1.1 200 OK

- Content-Type: application/xml; charset=utf-8

- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)>





**Параметры запроса:**



| Параметр   | Тип               | Кратность | Описание                                                                                                                                   |
|------------|-------------------|:---------:|--------------------------------------------------------------------------------------------------------------------------------------------|
| Host       | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                                                                                                        |
| CustomerID | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен |
| Account    | [AccNumType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#AccNumType)        | [0-1]       | Номер расчетного счета Клиента, для зарплатных проектов может отсутствовать                                                                                                             |
| Inn    | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (от 10 до 12)        | [1]       | Инн организации, для которой запрашиваются настройки                                                                                                             |
| Bic        | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (9)        | [1]       | БИК Банка                                                                                                                                  |
| SID        | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            | [1]       | Идентификатор авторизованной сессии                                                                                                        |
| APIVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                                                                                                  |
| AvailableAPIVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) | [0-1]       | Максимальная доступная для текущей информационной базы версия API обмена данными                                                                                                                  |

**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |



**Описание:**



При запросе настроек обмена в автоматическом режиме система «1С:Предприятие 8» передает в Банк номер расчетного счета Клиента (отправка производится HTTP-методом POST - метод [GetSettings](#2.2.1)). Банковский сервис по номеру расчетному счета клиента формирует файл настроек (XML-файл, соответствующий [XML-схеме настроек обмена с банком](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Settings) и в синхронном режиме возвращается в «1С:Предприятие 8» (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)).



![Получение настроек обмена с банком в автоматическом режиме. GetSettings](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/GetSettings.png)



Результатом неуспешного запроса настроек обмена будет ошибка, возвращаемая банковской системой также в синхронном режиме (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)).



Особенностью данного процесса является то, что на момент первого запроса настроек обмена с банком в системе «1С:Предприятие 8» неизвестен уникальный идентификатор Клиента в банковской системе. 
В этом случае следует использовать «0» в качестве значения этого реквизита.



**Пример запроса** получения настроек с банком:



```http

POST https://dbogate.demobank.ru/GetSettings HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
CustomerID: 0
SID: 8867755b6fbb4ae296aa0ac6b179ae88
Inn: 761700021132
Bic: 044525888
Account: 40802810200000099888
APIVersion: 2.1.1
AvailableAPIVersion: 2.1.1
User-Agent: 1C+Enterprise/8.3
Content-Type: application/xml; charset=utf-8
Content-Length: 0

```



**Пример успешного ответа** на запрос получения настроек с банком:



```xml

HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 2145

<?xml version="1.0" encoding="UTF-8"?>
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.1.1">
    <Success>
		<GetSettingsResponse creationDate="2015-02-19T11:21:02" formatVersion="2.1.1" id="502036">
			<Data dockind="06">	
        	PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPGVkbzpTZXR0aW5ncyBjcmVhdGlvbkRhdGU9IjIwMTUtMDItMTlUMTE6MjE6MDJaIiBmb3JtYXRWZXJzaW9uPSIyLjAyIiB1c2VyQWdlbnQ9IkJhbmtTZXJ2aWNlIiBpZD0iNTAyMDM2MjcxMjEiIHhzaTpzY2hlbWFMb2NhdGlvbj0iaHR0cDovL2JhbmsuMWMucnUvWE1MU2NoZW1hIDFDLUJhbmtfU2V0dGluZ3MueHNkIiB4bWxuczplZG89Imh0dHA6Ly9iYW5rLjFjLnJ1L1hNTFNjaGVtYSIgeG1sbnM6eHNpPSJodHRwOi8vd3d3LnczLm9yZy8yMDAxL1hNTFNjaGVtYS1pbnN0YW5jZSI+CiAgPGVkbzpTZW5kZXIgYmljPSIwNDQ1MjU5ODUiLz4KICA8ZWRvOlJlY2lwaWVudCBrcHA9IjExMTExMTExMSIgaW5uPSI3NjE3MDAwMjExMzIiIGlkPSI1MDIwMzYyNzEyMSIvPgogIDxlZG86RGF0YT4KICAgIDxlZG86Q3VzdG9tZXJJRD41MDIwMzYyNzEyMTwvZWRvOkN1c3RvbWVySUQ+CiAgICA8ZWRvOkJhbmtTZXJ2ZXJBZGRyZXNzPmh0dHBzOi8vZGJvZ2F0ZS1zdGFnZS5ub21vcy5ydS8xQ0Rib0dhdGUvPC9lZG86QmFua1NlcnZlckFkZHJlc3M+CiAgICA8ZWRvOkZvcm1hdFZlcnNpb24+Mi4wMjwvZWRvOkZvcm1hdFZlcnNpb24+CiAgICA8ZWRvOkVuY29kaW5nPlVURi04PC9lZG86RW5jb2Rpbmc+CiAgICA8ZWRvOkNvbXByZXNzPmZhbHNlPC9lZG86Q29tcHJlc3M+CiAgICA8ZWRvOkxvZ29uPgogICAgICA8ZWRvOkNlcnRpZmljYXRlPgogICAgICAgIDxlZG86RW5jcnlwdGluZ0FsZ29yaXRobT5HT1NUMjgxNDc8L2VkbzpFbmNyeXB0aW5nQWxnb3JpdGhtPgogICAgICA8L2VkbzpDZXJ0aWZpY2F0ZT4KICAgIDwvZWRvOkxvZ29uPgogICAgPGVkbzpDcnlwdG9QYXJhbWV0ZXJzPgogICAgICA8ZWRvOkNTUE5hbWU+U2lnbmFsLUNPTSBFQ0dPU1QgQ3J5cHRvZ3JhcGhpYyBQcm92aWRlcjwvZWRvOkNTUE5hbWU+CiAgICAgIDxlZG86Q1NQVHlwZT4xMjk8L2VkbzpDU1BUeXBlPgogICAgICA8ZWRvOlNpZ25BbGdvcml0aG0+RUNSMzQxMDwvZWRvOlNpZ25BbGdvcml0aG0+CiAgICAgIDxlZG86SGFzaEFsZ29yaXRobT5SVVMtSEFTSDwvZWRvOkhhc2hBbGdvcml0aG0+CiAgICAgIDxlZG86RW5jcnlwdGVkPgogICAgICAgIDxlZG86RW5jcnlwdEFsZ29yaXRobT5HT1NUMjgxNDc8L2VkbzpFbmNyeXB0QWxnb3JpdGhtPgogICAgICA8L2VkbzpFbmNyeXB0ZWQ+CiAgICAgIDxlZG86QmFua1RydXN0ZWRSb290Q2VydGlmaWNhdGUvPgogICAgICA8ZWRvOkJhbmtDZXJ0aWZpY2F0ZT5NSUlFMXpDQ0JIbWdBd0lCQWdJTEFXd0RBV1pnQVE4Q0xTb3dEZ1lLS3dZQkJBR3RXUUVEQWdVQU1JSHVNUXN3CkNRWURWUVFHRXdKU1ZURVZNQk1HQTFVRUNCNE1CQndFUGdSQkJEb0VNZ1F3TVJVd0V3WURWUVFISGd3RUhBUSsKQkVFRU9nUXlCREF4TlRBekJnTlZCQW9lTEFRZUJCQUVIZ0FnQkJFRU1BUTlCRG9BSUFBaUJDUUVHZ0FnQkI0RQpRZ1E2QkVBRVN3UkNCRGdFTlFBaU1WOHdYUVlEVlFRREhsWUVJd1EwQkQ0RVFRUkNCRDRFTWdRMUJFQUVUd1JPCkJFa0VPQVE1QUNBRUpnUTFCRDBFUWdSQUFDQUVIZ1FRQkI0QUlBUVJCREFFUFFRNkFDQUFJZ1FrQkJvQUlBUWUKQkVJRU9nUkFCRXNFUWdRNEJEVUFJakVaTUJjR0NTcUdTSWIzRFFFSkFSWUtjR3RwUUc5bVl5NXlkVEFlRncweApOREV4TWpBd09ESTRNamhhRncweE9URXhNakF3T0RJNE1qaGFNSUh2TVFzd0NRWURWUVFHRXdKU1ZURVZNQk1HCkExVUVDQjRNQkJ3RUhnUWhCQm9FRWdRUU1SVXdFd1lEVlFRSEhnd0VIQVFlQkNFRUdnUVNCQkF4TlRBekJnTlYKQkFvZUxBUWZCQkFFSGdBZ0JCRUVNQVE5QkRvQUlBQWlCQ1FFR2dBZ0JCNEVRZ1E2QkVBRVN3UkNCRGdFTlFBaQpNUlV3RXdZRFZRUUxIZ3dBUkFCQ0FFOEFMUUF4QUVNeFNUQkhCZ05WQkFNZVFBUWZCRUFFTlFRM0JEZ0VOQVExCkJEMEVRZ0FnQkI4RUVBUWVBQ0FFRVFRUUJCMEVHZ0FnQUNJRUpBUWFBQ0FFSGdRaUJCb0VJQVFyQkNJRUdBUVYKQUNJeEdUQVhCZ2txaGtpRzl3MEJDUUVXQ21SaWIwQnZabU11Y25Vd1hqQVdCZ29yQmdFRUFhMVpBUVlDQmdncQpoa2pPUFFNQkJ3TkVBQVJCQkZsRkRVTTRGM2wvWlVNKzMyRG82NVBaZS94eW5pUGd1RjZ2WHhYeDZ0Y1N3TnAwCjlzRXMvb29UUStvdVdiYVdISXZCT1FnTEU5SFZDWmZnUUY2R3JtK2pnZ0h3TUlJQjdEQWRCZ05WSFE0RUZnUVUKMDB3V1pMOCtuQlVOWHNMREUzczEzZHZGZndrd2dnRWtCZ05WSFNNRWdnRWJNSUlCRjRBVVNkOVQ1VDIyRmRNOQo4TUJ0M1puWk9ldGw5bG1oZ2ZTa2dmRXdnZTR4Q3pBSkJnTlZCQVlUQWxKVk1SVXdFd1lEVlFRSUhnd0VIQVErCkJFRUVPZ1F5QkRBeEZUQVRCZ05WQkFjZURBUWNCRDRFUVFRNkJESUVNREUxTURNR0ExVUVDaDRzQkI0RUVBUWUKQUNBRUVRUXdCRDBFT2dBZ0FDSUVKQVFhQUNBRUhnUkNCRG9FUUFSTEJFSUVPQVExQUNJeFh6QmRCZ05WQkFNZQpWZ1FqQkRRRVBnUkJCRUlFUGdReUJEVUVRQVJQQkU0RVNRUTRCRGtBSUFRbUJEVUVQUVJDQkVBQUlBUWVCQkFFCkhnQWdCQkVFTUFROUJEb0FJQUFpQkNRRUdnQWdCQjRFUWdRNkJFQUVTd1JDQkRnRU5RQWlNUmt3RndZSktvWkkKaHZjTkFRa0JGZ3B3YTJsQWIyWmpMbkoxZ2dnQmJBRUJBUThCQWpBTEJnTlZIUThFQkFNQ0EvZ3dnWlVHQTFVZApJQVNCalRDQmlqQ0Jod1lLS3dZQkJBR0NuQVVDQVRCNU1ERUdDQ3NHQVFVRkJ3SUJGaVZvZEhSd09pOHZkM2QzCkxtOW1ZeTV5ZFM5aFltOTFkQzlqWVMxeVpXZHNZVzFsYm5Rdk1FUUdDQ3NHQVFVRkJ3SUNNRGdhTnRIbzhmTGwKN1BzZzVPang4dUR0OXVqdTdlM3U0KzRnNGVEdDZ1N2k4ZXJ1NCs0Zzd1SHg2L1BtNk9MZzdlai9JQ2pCNE8zcQpLVEFPQmdvckJnRUVBYTFaQVFNQ0JRQURTQUF3UlFJZ0R3NEpKL0FmV3E3WHY1aW5lYWJCQVlmUHJiclhGVU1ICjVxbGtseUJDL3ljQ0lRQzJjekI1OWdxeS9EVDRDeFRkR3gvdW9HTHNtT2ZHeGJNUzE1Q0xobGNOYlE9PTwvZWRvOkJhbmtDZXJ0aWZpY2F0ZT4KICAgICAgPGVkbzpDdXN0b21lclNpZ25hdHVyZT4KICAgICAgICA8ZWRvOkdyb3VwU2lnbmF0dXJlcyBudW1iZXJHcm91cD0iMSI+CiAgICAgICAgICA8ZWRvOkNlcnRpZmljYXRlPk1JSUV2RENDQkYyZ0F3SUJBZ0lMQVd3REFWc0RBUThDSCtZd0RnWUtLd1lCQkFHdFdRRURBZ1VBTUlIdU1Rc3cKQ1FZRFZRUUdFd0pTVlRFVk1CTUdBMVVFQ0I0TUJCd0VQZ1JCQkRvRU1nUXdNUlV3RXdZRFZRUUhIZ3dFSEFRKwpCRUVFT2dReUJEQXhOVEF6QmdOVkJBb2VMQVFlQkJBRUhnQWdCQkVFTUFROUJEb0FJQUFpQkNRRUdnQWdCQjRFClFnUTZCRUFFU3dSQ0JEZ0VOUUFpTVY4d1hRWURWUVFESGxZRUl3UTBCRDRFUVFSQ0JENEVNZ1ExQkVBRVR3Uk8KQkVrRU9BUTVBQ0FFSmdRMUJEMEVRZ1JBQUNBRUhnUVFCQjRBSUFRUkJEQUVQUVE2QUNBQUlnUWtCQm9BSUFRZQpCRUlFT2dSQUJFc0VRZ1E0QkRVQUlqRVpNQmNHQ1NxR1NJYjNEUUVKQVJZS2NHdHBRRzltWXk1eWRUQWVGdzB4Ck5ERXdNRFl3TnpFd016SmFGdzB4TlRFeU1UQXdOekV3TXpKYU1JSGFNUXN3Q1FZRFZRUUdFd0pTVlRFVk1CTUcKQTFVRUNCNE1CQndFSGdRaEJCb0VFZ1FRTVNrd0p3WURWUVFLSGlBRUdBUWZBQ0FFSHdRMUJFSUVRQVErQkRJRQpPQVJIQUNBRU9BQWdCQm9FUGpFUE1BMEdBMVVFQ3g0R0FFUUFRZ0JQTVVVd1F3WURWUVFNSGp3RUdBUTlCRFFFCk9BUXlCRGdFTkFSREJEQUVPd1JNQkQwRVN3UTVBQ0FFUHdSQUJEVUVOQVEvQkVBRU9BUTlCRGdFUEFRd0JFSUUKTlFRN0JFd3hNVEF2QmdOVkJBTWVLQVFmQkRVRVFnUkFCRDRFTWdBZ0JCOEVOUVJDQkVBQUlBUWZCRFVFUWdSQQpCRDRFTWdRNEJFY3dYakFXQmdvckJnRUVBYTFaQVFZQ0JnZ3Foa2pPUFFNQkJ3TkVBQVJCQks4eDZDZ09aYmJRCkd2eDVZQm1LbDRWdFo5UkhzbnQzK2wwS0k3eWt6RGFWOGRmVldONjN0eE5xSWJTOUNIQ1lQNU1ZZzZINFdCRWMKR1F4bGMxVU9YVU9qZ2dIcE1JSUI1VEFkQmdOVkhRNEVGZ1FVTHhCZlFUSGFWQ3BJRmtOVzZPa0p4NWNJYi9BdwpnZ0VrQmdOVkhTTUVnZ0ViTUlJQkY0QVVTZDlUNVQyMkZkTTk4TUJ0M1puWk9ldGw5bG1oZ2ZTa2dmRXdnZTR4CkN6QUpCZ05WQkFZVEFsSlZNUlV3RXdZRFZRUUlIZ3dFSEFRK0JFRUVPZ1F5QkRBeEZUQVRCZ05WQkFjZURBUWMKQkQ0RVFRUTZCRElFTURFMU1ETUdBMVVFQ2g0c0JCNEVFQVFlQUNBRUVRUXdCRDBFT2dBZ0FDSUVKQVFhQUNBRQpIZ1JDQkRvRVFBUkxCRUlFT0FRMUFDSXhYekJkQmdOVkJBTWVWZ1FqQkRRRVBnUkJCRUlFUGdReUJEVUVRQVJQCkJFNEVTUVE0QkRrQUlBUW1CRFVFUFFSQ0JFQUFJQVFlQkJBRUhnQWdCQkVFTUFROUJEb0FJQUFpQkNRRUdnQWcKQkI0RVFnUTZCRUFFU3dSQ0JEZ0VOUUFpTVJrd0Z3WUpLb1pJaHZjTkFRa0JGZ3B3YTJsQWIyWmpMbkoxZ2dnQgpiQUVCQVE4QkFqQUxCZ05WSFE4RUJBTUNBL2d3Z1k0R0ExVWRJQVNCaGpDQmd6Q0JnQVlLS3dZQkJBR0NuQVVCCkFUQnlNREVHQ0NzR0FRVUZCd0lCRmlWb2RIUndPaTh2ZDNkM0xtOW1ZeTV5ZFM5aFltOTFkQzlqWVMxeVpXZHMKWVcxbGJuUXZNRDBHQ0NzR0FRVUZCd0lDTURFYUw5SG84ZkxsN1BzZzVPang4dUR0OXVqdTdlM3U0KzRnNGVEdAo2dTdpOGVydTQrNGc3dUh4Ni9QbTZPTGc3ZWovTUE0R0Npc0dBUVFCclZrQkF3SUZBQU5KQURCR0FpRUF3L01MCk9ucWczNDFkOE1IK3NDRlJQOUxtMDMzUDcveDEwckF2TisydytxUUNJUUROQUJnem1yOFNFemtSd1kwKysrOWgKZXRmWGl1MGZSRHNudkk0QlhWRTZZdz09PC9lZG86Q2VydGlmaWNhdGU+CiAgICAgICAgPC9lZG86R3JvdXBTaWduYXR1cmVzPgogICAgICA8L2VkbzpDdXN0b21lclNpZ25hdHVyZT4KICAgIDwvZWRvOkNyeXB0b1BhcmFtZXRlcnM+CiAgICA8ZWRvOkRvY3VtZW50IGRvY0tpbmQ9IjAxIi8+CiAgICA8ZWRvOkRvY3VtZW50IGRvY0tpbmQ9IjAyIi8+CiAgICA8ZWRvOkRvY3VtZW50IGRvY0tpbmQ9IjAzIi8+CiAgICA8ZWRvOkRvY3VtZW50IGRvY0tpbmQ9IjA0Ij4KICAgICAgPGVkbzpTaWduZWQ+CiAgICAgICAgPGVkbzpSdWxlU2lnbmF0dXJlcz4oMCk8L2VkbzpSdWxlU2lnbmF0dXJlcz4KICAgICAgPC9lZG86U2lnbmVkPgogICAgPC9lZG86RG9jdW1lbnQ+CiAgICA8ZWRvOkRvY3VtZW50IGRvY0tpbmQ9IjA1Ij4KICAgICAgPGVkbzpTaWduZWQ+CiAgICAgICAgPGVkbzpSdWxlU2lnbmF0dXJlcz4oMCk8L2VkbzpSdWxlU2lnbmF0dXJlcz4KICAgICAgPC9lZG86U2lnbmVkPgogICAgPC9lZG86RG9jdW1lbnQ+CiAgICA8ZWRvOkRvY3VtZW50IGRvY0tpbmQ9IjA2Ii8+CiAgICA8ZWRvOkRvY3VtZW50IGRvY0tpbmQ9IjEwIj4KICAgICAgPGVkbzpTaWduZWQ+CiAgICAgICAgPGVkbzpSdWxlU2lnbmF0dXJlcz4oMCk8L2VkbzpSdWxlU2lnbmF0dXJlcz4KICAgICAgPC9lZG86U2lnbmVkPgogICAgPC9lZG86RG9jdW1lbnQ+CiAgICA8ZWRvOkRvY3VtZW50IGRvY0tpbmQ9IjE0Ij4KICAgICAgPGVkbzpTaWduZWQ+CiAgICAgICAgPGVkbzpSdWxlU2lnbmF0dXJlcz4oMCk8L2VkbzpSdWxlU2lnbmF0dXJlcz4KICAgICAgPC9lZG86U2lnbmVkPgogICAgPC9lZG86RG9jdW1lbnQ+CiAgICA8ZWRvOkRvY3VtZW50IGRvY0tpbmQ9IjE1Ii8+CiAgPC9lZG86RGF0YT4KPC9lZG86U2V0dGluZ3M+Cg==
   			</Data>
        </GetSettingsResponse>
	</Success>
</ResultBank>

```



## <a name="2"></a> Порядок взаимодействия на транспортном уровне



Данный уровень позволяет отправлять и получать электронные документы между системой «1С:Предприятие 8» Клиента и Банком по согласованным между сторонами обмена настройкам, используя шифрованный канал (протокол TLS 1.0/1.2) (подробнее см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)).



Инициатором сеанса обмена всегда выступает система «1С:Предприятие 8».

Для отправки и получения всех электронных документов используется единый адрес ресурса банка вида: `https://<host>:<port>`.



Для передачи электронных документов между участниками обмена используется «транспортный контейнер» - XML-файл, сформированный по определенному формату ([XML-схема транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)).



Отправителем и получателем транспортного контейнера могут быть как Клиент (Организация), работающий на системе «1С:Предприятие 8», так и Банк (роли участников обмена зависят от конкретной бизнес-операции).



### <a name="2.1"></a> Формирование и разбор транспортного контейнера



Формирование и разбор транспортного контейнер зависит от настроек обмена с конкретным банковским сервисом и может быть представлен в виде схем:



![Формирование и разбор транспортного контейнера](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/shipping_container_write_and_read_rules.png)





### <a name="2.3"></a> Отправка электронных документов из 1С



Отправка электронных документов из 1С выполняется в 3 этапа:

- формирование транспортного контейнера, содержащего электронные документы;

- аутентификация Клиента на стороне Банка (если нет ранее открытой сессии);

- отправка транспортного контейнера в Банк и обработка ответа;


![Отправка электронных документов из 1С в банковский сервис](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/Sending_packages_of_electronic_documents_to_the_bank.png)

При отправке электронных документов из 1С будут последовательно вызваны следующие методы банковского сервиса:
- Аутентификация:
 -	Для аутентификации по логину и паролю, только [Logon](#1.2.1);
 -	Для аутентификации по логину и паролю с двухфакторной авторизацией — [Logon](#1.2.1), а затем — [LogonOTP](#1.2.2).
- Отправка транспортного контейнера с данными электронных документов из 1С — [SendPack](#2.3.1).


## <a name="2.3.1"></a> Метод SendPack (HTTP-метод POST)



**Заголовки:**

- Host: <Адрес ресурса банка>

- Content-Type: application/xml; charset=utf-8

- CustomerID: <Уникальный идентификатор Клиента, содержащий только ANSI-символы>

- SID: <Идентификатор авторизованной сессии>

- APIVersion: <Версия API обмена данными>




**Тело запроса:**

- < XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)>





**Успешный ответ:**

- HTTP/1.1 200 OK
- Content-Type: application/xml; charset=utf-8
- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)>





**Параметры запроса:**



| Параметр   | Тип               | Кратность | Описание                                                         |
|------------|-------------------|:---------:|------------------------------------------------------------------|
| Host       | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| CustomerID | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы |
| SID        | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            | [1]       | Идентификатор авторизованной сессии                              |
| APIVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |
| Packet     | [Packet](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#Packet)            | [1]       | Транспортный контейнер с данными электронных документов                                     |



**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |





**Описание:**



- В «1С:Предприятии 8» формируется транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)) с одним или несколькими электронными документами.

 - Если настройки обмена между Клиентом и Банком предусматривают сжатие данных, то электронные документы перед помещением в транспортный контейнер сжимаются в формате zip-архива, который описан в открытой спецификации, доступной по адресу <http://www.pkware.com/documents/casestudies/APPNOTE.TXT>.

- Далее проходит аутентификация на стороне Банка согласно протоколу, описанному в разделе «[Порядок взаимодействия на уровне аутентификации](#1)».

- Если аутентификация пройдена успешно, транспортный контейнер передается в Банк (отправка на ресурс Банка производится HTTP-методом POST – метод SendPack), в синхронном режиме банковская система возвращает либо ошибку получения, либо уникальный идентификатор на стороне Банка, который будет однозначно соответствовать полученному транспортному контейнеру (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)).

- Банковская система может проводить контроль формата и разбор транспортного контейнера:

 - либо в синхронном режиме (сразу в момент получения);

 - либо в асинхронном режиме (ставит входящие трасп.контейнеры в очередь на разбор).

- Для такого варианта уникальный идентификатор возвращается сразу, после того, как транспортный контейнер успешно был поставлен в очередь на разбор, не дожидаясь самого разбора.

- Далее запускается процесс разбора очереди входящих транспортных контейнеров банковской системой, результатом которого будет ответ о состоянии обработки (XML-файл, соответствующий [XML-схеме извещения о состояния обработки транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusPacketNotice)).

- Подготовленный ответ Банка упаковывается в транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)) и ожидает на стороне Банка очередного запроса на наличие подготовленных к передаче транспортных контейнеров из «1С:Предприятие 8».

- Параллельно с процессом отправки ответа о состоянии обработки транспортного контейнера банковская система выполняет обработку каждого электронного документа, входящего в него.



![Отправка электронных документов из 1С. SendPack](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/SendPack.png)



**Пример отправки** транспортного контейнера:

```http

POST https://dbogate.demobank.ru/SendPack HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
SID: 8867755b6fbb4ae296aa0ac6b179ae88
CustomerID: 502036
APIVersion: 2.1.1
User-Agent: 1C+Enterprise/8.3
Content-Type: application/xml; charset=utf-8
Content-Length: 5239

<?xml version="1.0" encoding="UTF-8"?>
<Packet xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    id="9bad1c35-b7ff-11e4-9a88-0003ffb697db"
    formatVersion="2.1.1"
    creationDate="2016-04-19T11:12:31"
    userAgent="1С - БЭД: 1.3">
	<Sender>
		<Customer id="502036" name="ИП Петрович и Ко" inn="761700021132"/>
	</Sender>
	<Recipient>
		<Bank bic="044525888" name="ДЕМО-БАНК"/>
	</Recipient>
	<Document id="a64225eb-9737-4d80-bd9d-1ffe5fdb63b1" dockind="10" formatVersion="2.02">
		<Data>
77u/PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4NCjxQYXlEb2NSdSB4bWxucz0iaHR0cDovL2JhbmsuMWMucnUvWE1MU2NoZW1hIiB4bWxuczp4cz0iaHR0cDovL3d3dy53My5vcmcvMjAwMS9YTUxTY2hlbWEiIHhtbG5zOnhzaT0iaHR0cDovL3d3dy53My5vcmcvMjAwMS9YTUxTY2hlbWEtaW5zdGFuY2UiIGlkPSJhNjQyMjVlYi05NzM3LTRkODAtYmQ5ZC0xZmZlNWZkYjYzYjEiIGZvcm1hdFZlcnNpb249IjIuMDIiIGNyZWF0aW9uRGF0ZT0iMjAxNS0wMi0xOVQxMToxMTo0MyIgdXNlckFnZW50PSIx0KEgLSDQkdCt0JQ6IDEuMi42Ljg7INCR0LjQsdC70LjQvtGC0LXQutCw0K3Qu9C10LrRgtGA0L7QvdC90YvRhdCU0L7QutGD0LzQtdC90YLQvtCyOiAxLjIuNi44Ij4NCgk8U2VuZGVyIGlkPSI1MDIwMzYyNzEyMSIgbmFtZT0i0JjQnyDQn9C10YLRgNC+0LLQuNGHINC4INCa0L4iIGlubj0iNzYxNzAwMDIxMTMyIiBrcHA9IjExMTExMTExMSIvPg0KCTxSZWNpcGllbnQgYmljPSIwNDQ1MjU5ODUiIG5hbWU9ItCf0JDQniDQkdCQ0J3QmiAmcXVvdDvQpNCaINCe0KLQmtCg0KvQotCY0JUmcXVvdDsiLz4NCgk8RGF0YT4NCgkJPERvY05vPjY8L0RvY05vPg0KCQk8RG9jRGF0ZT4yMDE1LTAyLTE5PC9Eb2NEYXRlPg0KCQk8U3VtPjE8L1N1bT4NCgkJPFBheWVyPg0KCQkJPE5hbWU+0JjQnyDQn9C10YLRgNC+0LLQuNGHINC4INCa0L48L05hbWU+DQoJCQk8SU5OPjc2MTcwMDAyMTEzMjwvSU5OPg0KCQkJPEtQUD4xMTExMTExMTE8L0tQUD4NCgkJCTxBY2NvdW50PjQwODAyODEwMjAwMDAwMDk5MzAyPC9BY2NvdW50Pg0KCQkJPEJhbms+DQoJCQkJPEJJQz4wNDQ1MjU5ODU8L0JJQz4NCgkJCQk8TmFtZT7Qn9CQ0J4g0JHQkNCd0JogItCk0Jog0J7QotCa0KDQq9Ci0JjQlSI8L05hbWU+DQoJCQkJPENpdHk+0JMuINCc0J7QodCa0JLQkDwvQ2l0eT4NCgkJCQk8Q29ycmVzcEFjYz4zMDEwMTgxMDMwMDAwMDAwMDk4NTwvQ29ycmVzcEFjYz4NCgkJCTwvQmFuaz4NCgkJPC9QYXllcj4NCgkJPFBheWVlPg0KCQkJPE5hbWU+0J7QntCeINCk0LjRgNC80LAgItCU0JTQlSI8L05hbWU+DQoJCQk8SU5OPjc3MjQwMDI5MzY8L0lOTj4NCgkJCTxBY2NvdW50PjQwODE3ODEwMzAwMDAwMDAwNzc5PC9BY2NvdW50Pg0KCQkJPEJhbms+DQoJCQkJPEJJQz4wNDQ1ODM4NjE8L0JJQz4NCgkJCQk8TmFtZT7QntCe0J4gItCY0J3QkdCQ0J3QmiI8L05hbWU+DQoJCQkJPENpdHk+0JMuINCc0J7QodCa0JLQkDwvQ2l0eT4NCgkJCQk8Q29ycmVzcEFjYz4zMDEwMTgxMDAwMDAwMDAwMDg2MTwvQ29ycmVzcEFjYz4NCgkJCTwvQmFuaz4NCgkJPC9QYXllZT4NCgkJPFRyYW5zaXRpb25LaW5kPjAxPC9UcmFuc2l0aW9uS2luZD4NCgkJPFByaW9yaXR5PjU8L1ByaW9yaXR5Pg0KCQk8UHVycG9zZT7QsdC10Lcg0J3QlNChPC9QdXJwb3NlPg0KCTwvRGF0YT4NCjwvUGF5RG9jUnU+
		</Data>
		<Signature x509IssuerName="Удостоверяющий Центр Банка" x509SerialNumber="022C03015B03010F022FE2">
		<SignedData>
MIIGbQYJKoZIhvcNAQcCoIIGXjCCBloCAQExEDAOBgorBgEEAa1ZAQIBBQAwCwYJKoZIhvcNAQcBoIIEwDCCBLwwggRdoAMCAQICCwFsAwFbAwEPAh/mMA4GCisGAQQBrVkBAwIFADCB7jELMAkGA1UEBhMCUlUxFTATBgNVBAgeDAQcBD4EQQQ6BDIEMDEVMBMGA1UEBx4MBBwEPgRBBDoEMgQwMTUwMwYDVQQKHiwEHgQQBB4AIAQRBDAEPQQ6ACAAIgQkBBoAIAQeBEIEOgRABEsEQgQ4BDUAIjFfMF0GA1UEAx5WBCMENAQ+BEEEQgQ+BDIENQRABE8ETgRJBDgEOQAgBCYENQQ9BEIEQAAgBB4EEAQeACAEEQQwBD0EOgAgACIEJAQaACAEHgRCBDoEQARLBEIEOAQ1ACIxGTAXBgkqhkiG9w0BCQEWCnBraUBvZmMucnUwHhcNMTQxMDA2MDcxMDMyWhcNMTUxMjEwMDcxMDMyWjCB2jELMAkGA1UEBhMCUlUxFTATBgNVBAgeDAQcBB4EIQQaBBIEEDEpMCcGA1UECh4gBBgEHwAgBB8ENQRCBEAEPgQyBDgERwAgBDgAIAQaBD4xDzANBgNVBAseBgBEAEIATzFFMEMGA1UEDB48BBgEPQQ0BDgEMgQ4BDQEQwQwBDsETAQ9BEsEOQAgBD8EQAQ1BDQEPwRABDgEPQQ4BDwEMARCBDUEOwRMMTEwLwYDVQQDHigEHwQ1BEIEQAQ+BDIAIAQfBDUEQgRAACAEHwQ1BEIEQAQ+BDIEOARHMF4wFgYKKwYBBAGtWQEGAgYIKoZIzj0DAQcDRAAEQQSvMegoDmW20Br8eWAZipeFbWfUR7J7d/pdCiO8pMw2lfHX1Vjet7cTaiG0vQhwmD+TGIOh+FgRHBkMZXNVDl1Do4IB6TCCAeUwHQYDVR0OBBYEFC8QX0Ex2lQqSBZDVujpCceXCG/wMIIBJAYDVR0jBIIBGzCCAReAFEnfU+U9thXTPfDAbd2Z2TnrZfZZoYH0pIHxMIHuMQswCQYDVQQGEwJSVTEVMBMGA1UECB4MBBwEPgRBBDoEMgQwMRUwEwYDVQQHHgwEHAQ+/EhM5EcGNPvvvYXrX14rtH0Q7J7yOAV1ROmMxggFwMIIBbAIBATCB/jCB7jELMAkGA1UEBhMCUlUxRJBDgEOQAgBCYENQQ9BEIEQAAgBB4EEAQeACAEEQQwBD0EOgAgACIEJAQaACAEHgRCBDoEQARLBEIEOAQ1ACIxGTAXBgkqhkiG9w0BCQEWCnBraUBvZmMucnUCCwFsAwFbAwEPAh/mMA4GCisGAQQBrVkBAgEFADAMBgorBgEEAa1ZAQYCBEgwRgIhALJ4SDHfRVBq9egxlJiAC+tGHRNU7vg4AIUA8iS9qFmOAiEA5rdyEyuYZ5H46JjDNVJexcYmgCuDNpiU15rskCKDuVc=
        	</SignedData>
		</Signature>
	</Document>
</Packet>

```



**Пример успешного ответа** на запрос отправки транспортного контейнера:

```xml

HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 145

<?xml version="1.0" encoding="UTF-8"?>
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.1.1">
    <Success>
        <SendPacketResponse>
			<ID>50214584626</ID>
        </SendPacketResponse>
    </Success>
</ResultBank>

```



### <a name="2.4"></a> Получение электронных документов в 1С



Получение электронных документов в 1С выполняется в 3 этапа:

- аутентификация Клиента на стороне Банка (если нет ранее открытой сессии);

- запрос у Банка списка подготовленных к передаче транспортных контейнеров, содержащих электронные документы для Клиента;

- запрос у Банка транспортного контейнера по его уникальному идентификатору и разбор в 1С.


![Получение электронных документов из банковского сервиса в 1С](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/Receive_packages_of_electronic_documents_from_the_bank.png)

При получении электронных документов из банковского сервиса в 1С будут последовательно вызваны следующие методы:
- Аутентификация:
 - Для аутентификации по логину и паролю, только [Logon](#1.2.1);
 - Для аутентификации по логину и паролю с двухфакторной авторизацией — [Logon](#1.2.1), а затем — [LogonOTP](#1.2.2).
- Запрос на получение списка GUID транспортных контейнеров, готовых к отправке клиенту банком, — [GetPackList](#2.4.1).
- В цикле запрос на получение транспортного контейнера по GUID из ранее полученного списка [GetPack](#2.4.1)

Если после получения и разбора всех контейнеров из списка, 1С не находит нужный контейнер, процесс будет начат заново. Это происходит при ожидании получения выписки банка.


## <a name="2.4.1"></a> Метод GetPackList (HTTP-метод GET)



**Заголовки:**

- Host: <Адрес ресурса банка>

- Content-Type: application/xml; charset=utf-8

- CustomerID: <Уникальный идентификатор Клиента, содержащий только ANSI-символы>

- SID: <Идентификатор авторизованной сессии>

- APIVersion: <Версия API обмена данными>



**Адрес запроса:**

- https://<Адрес ресурса банка>/GetPackList?date=<Отметка времени>

(Отметка времени в формате «dd.MM.yyyy HH:mm:ss», где dd – день числом, MM – месяц числом, yyyy – год числом, HH – часы в формате 24, mm – минуты, ss – секунды.)



**Успешный ответ:**

- HTTP/1.1 200 OK
- Content-Type: application/xml; charset=utf-8
- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)>





**Параметры запроса:**



| Параметр   | Тип               | Кратность | Описание                                                         |
|------------|-------------------|:---------:|------------------------------------------------------------------|
| Host       | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| CustomerID | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы |
| SID        | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            | [1]       | Идентификатор авторизованной сессии                              |
| APIVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |



**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |



**Важно:**

- Обратите внимание на параметр ***TimeStampLastPacket*** в ответе, это метка времени, на которую вернули всю актуальную информацию.





**Описание:**



- Проходит аутентификация на стороне Банка согласно протоколу, описанному в разделе «[Порядок взаимодействия на уровне аутентификации](#1)».

- Если аутентификация пройдена успешно, система «1С:Предприятия 8» запрашивает Банк на наличие подготовленных транспортных контейнеров для Клиента (запрос на ресурс Банка производится HTTP-методом GET – метод GetPackList). В синхронном режиме банковская система возвращает список уникальных идентификаторов транспортных контейнеров, готовых к передаче со стороны Банка и отметку времени, равную дате и времени формирования (передаются значения даты и времени сервера Банка) самого последнего транспортного контейнера, входящего в этот список (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)).

 - Если в запросе из «1С:Предприятия 8» о готовых к передаче транспортных контейнерах указать «Отметку времени» (указывается значение даты и времени сервера Банка с точностью до секунды), то именно с этого момента времени будет формироваться список уникальных идентификаторов (в формате GUID), подготовленных к передаче в хронологическом порядке.



   Для того, чтобы получить список GUID всех когда-либо сформированных транспортных контейнеров на банковской стороне, параметр «Отметка времени» в запросе не передается.


**Пример запроса** получения списка уникальных идентификаторов транспортных контейнеров:

```http

GET https://dbogate.demobank.ru/GetPackList?date=16.02.2015%2011:25:32 HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
SID: 8867755b6fbb4ae296aa0ac6b179ae88
CustomerID: 502036
APIVersion: 2.1.1
User-Agent: 1C+Enterprise/8.3

```



**Пример успешного ответа** на запрос получения списка уникальных идентификаторов:

```xml

HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 145


<?xml version="1.0" encoding="UTF-8"?>
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.1.1">
    <Success>
        <GetPacketListResponse TimeStampLastPacket="2015-02-19T11:15:42">
			<PacketID>50214585876</PacketID>
        </GetPacketListResponse>
    </Success>
</ResultBank>

```

Система «1С:Предприятие 8» после получения списка GUID готовых к передаче транспортных контейнеров запрашивает конкретный транспортный контейнер по его GUID (запрос на ресурс Банка производится HTTP-методом GET – метод [GetPack](#2.4.1)). В синхронном режиме банковская система возвращает либо ошибку, либо транспортный контейнер (XML-файл, соответствующий XML-схеме ответа банк.сервиса).

Далее происходит разбор содержимого транспортного контейнера уже в системе «1С:Предприятие 8».





## <a name="2.4.2"></a> Метод GetPack (HTTP-метод GET)



**Заголовки:**

- Host: <Адрес ресурса банка>

- Content-Type: application/xml; charset=utf-8

- CustomerID: <Уникальный идентификатор Клиента, содержащий только ANSI-символы>

- SID: <Идентификатор авторизованной сессии>

- APIVersion: <Версия API обмена данными>





**Адрес запроса:**

- https://<Адрес ресурса банка>/GetPack?id=< GUID транспортного контейнера>



**Тело запроса:**

- ПУСТО





**Успешный ответ:**

- HTTP/1.1 200 OK
- Content-Type: application/xml; charset=utf-8
- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)>



**Параметры запроса:**



| Параметр   | Тип               | Кратность | Описание                                                         |
|------------|-------------------|:---------:|------------------------------------------------------------------|
| Host       | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| CustomerID | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы |
| SID        | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            | [1]       | Идентификатор авторизованной сессии                              |
| APIVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |





**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |



**Пример запроса** получения транспортного контейнера:

```http

GET https://dbogate.demobank.ru/GetPack?id=50214585876 HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
SID: 8867755b6fbb4ae296aa0ac6b179ae88
CustomerID: 502036
APIVersion: 2.1.1
User-Agent: 1C+Enterprise/8.3

```



**Пример успешного ответа** на запрос получения транспортного контейнера:

```xml

HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 2502

<?xml version="1.0" encoding="UTF-8"?>
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.1.1">
    <Success>
		<GetPacketResponse userAgent="Back Office"
        creationDate="2015-02-19T11:15:42"
        formatVersion="2.1.1"
        id="0ef6778b-4a2c-717c-e053-248c410af4aa">
			<Sender>
				<Bank bic="044525888"/>
			</Sender>
        	<Recipient>
				<Customer id="502036"/>
	    	</Recipient>
        	<Document notifyRequired="false"
        	signResponse="false"
        	encrypted="false"
        	compressed="false"
        	testOnly="false"
        	formatVersion="2.02"
        	dockind="01"
        	id="0f6d6032-31b0-8337-e053-248c410a1832">
        		<Data contentType="text/xml"fileName="">
PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPFN0YXR1c1BhY2tldE5vdGljZSB4bWxucz0iaHR0cDovL2JhbmsuMWMucnUvWE1MU2NoZW1hIiBpZD0iMGY2ZDYwMzItMzFiMC04MzM3LWUwNTMtMjQ4YzQxMGExODMyIiBmb3JtYXRWZXJzaW9uPSIyLjAyIiBjcmVhdGlvbkRhdGU9IjIwMTUtMDItMTlUMTE6MDM6NDgiPgogIDxTZW5kZXI+CiAgICA8QmFuayBiaWM9IjA0NDUyNTk4NSIvPgogIDwvU2VuZGVyPgogIDxSZWNpcGllbnQ+CiAgICA8Q3VzdG9tZXIgaWQ9IjUwMjAzNjI3MTIxIi8+CiAgPC9SZWNpcGllbnQ+CiAgPElEUmVzdWx0U3VjY2Vzc1Jlc3BvbnNlPjUwMjE0NTgwODM4PC9JRFJlc3VsdFN1Y2Nlc3NSZXNwb25zZT4KICA8UmVzdWx0PgogICAgPFN0YXR1cz4KICAgICAgPENvZGU+MDE8L0NvZGU+CiAgICAgIDxOYW1lPtCf0YDQuNC90Y/RgjwvTmFtZT4KICAgIDwvU3RhdHVzPgogIDwvUmVzdWx0Pgo8L1N0YXR1c1BhY2tldE5vdGljZT4K
         		</Data>
         		<Signature x509SerialNumber="076C03010BBF0109029965" x509IssuerName="CN=Удостоверяющий Центр Банка">
            		<SignedData>
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
            		</SignedData>
         		</Signature>
        	</Document>
    	</GetPacketResponse>
	</Success>
</ResultBank>

```

### <a name="2.5"></a> Процедура синхронизации 1С с банковским сервисом

При синхронизации 1С с банковским сервисом последовательно выполняются процедуры отправки всех подготовленных в 1С транспортных контейнеров с данными электронных документов в банк и получения всех транспортных контейнеров документов из банка.

[Процедура отправки в банк](#2.3) выполняется в цикле. Для каждого подготовленного к отправке транспортного контейнера будут вызваны следующие методы:
- Аутентификация:
 -	Для аутентификации по логину и паролю, только [Logon](#1.2.1);
 -	Для аутентификации по логину и паролю с двухфакторной авторизацией — [Logon](#1.2.1), а затем — [LogonOTP](#1.2.2).
- Отправка транспортного контейнера с данными электронных документов из 1С — [SendPack](#2.3.1).


При [получении электронных документов из банковского сервиса в 1С](#2.4) будут последовательно вызваны следующие методы:
- Аутентификация:
 - Для аутентификации по логину и паролю, только [Logon](#1.2.1);
 - Для аутентификации по логину и паролю с двухфакторной авторизацией — [Logon](#1.2.1), а затем — [LogonOTP](#1.2.2).
- Запрос на получение списка GUID транспортных контейнеров, готовых к отправке клиенту банком — [GetPackList](#2.4.1).
- Для каждого GUID из ранее полученного списка запрос на получение транспортного контейнера — [GetPack](#2.4.1).

