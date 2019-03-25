# API прямого обмена данными с банком



API обмена данными – уровень, который описывает подходы и методы осуществления операций обмена данными, он в свою очередь делится на следующие уровни:

- Уровень аутентификации

- Настройка обмена

- Транспортный уровень



- - -

+ [Порядок взаимодействия на уровне аутентификации](#authentication)
	+ [Аутентификация по логину и паролю](#baseAuthentication)

 		+ [Метод **Logon** (HTTP-метод POST)](#logon)
 		
 		+ [Метод **LogonOTP** (HTTP-метод POST)](#logonOTP)

	+ [Рекомендации для банковского сервиса](#recommendations)

+ [Настройка обмена электронными документами](#installation)

	+ [Получение настроек обмена с банком в автоматическом режиме](#installationGet)

		+ [Метод **GetSettings** (HTTP-метод POST)](#getSettings)

	+ [Пример XML-файла настроек обмена с банком](#exampleSettings)

+ [Порядок взаимодействия на транспортном уровне](#transport)

	+ [Формирование и разбор транспортного контейнера](#transportPacket)

	+ [Отправка электронных документов из 1С](#send)

		+ [Метод **SendPack** (HTTP-метод POST)](#sendPack)

	+ [Получение электронных документов в 1С](#get)

		+ [Метод **GetPackList** (HTTP-метод GET)](#getPackList)

		+ [Метод **GetPack** (HTTP-метод GET)](#getPack)

+ [Проверка работоспособности обмена электронными документами](#test)

	+ [Пример XML-файла запроса-зонда](#zond)

+ [Процедура синхронизации 1С с банковским сервисом](#synchronization)

+ [Обеспечение безопасности данных](../common-section/data-security.md#security)

	+ [Шифрование данных при передаче](../common-section/data-security.md#encryption)

	+ [Криптооперации, выполняемые на стороне Клиента и Банка](../common-section/data-security.md#crypto)

	+ [Защита электронных документов с помощью электронной подписи](../common-section/data-security.md#signature)

+ [Схемы данных](../xsd-scheme/readme.md)

+ [Классификаторы](../common-section/tables.md)

	+ [Таблица кодов статусов транспортных контейнеров](../common-section/tables.md#packet)

	+ [Таблица кодов видов электронных документов](../common-section/tables.md#ed)

	+ [Таблица кодов статусов электронных документов](../common-section/tables.md#status)

	+ [Таблица типов выписок банка](../common-section/tables.md#statementType)

	+ [Таблица кодов ошибок и их описание, которые может возвращать банковский сервис в «1С:Предприятие 8»](../common-section/tables.md#errors)

+ [Описание типов](../common-section/type-tables.md)



- - - -


## <a name="authentication"></a> Порядок взаимодействия на уровне аутентификации.



Аутентификация Клиента с целью создания сессии на сервисе Банка может проходить по логину и паролю + дополнительный OTP («one time password», опционально) ([Logon](#logon)) или по закрытому ключу сертификата ([LogonCert](#logoncert)) . 
Аутентификация по логину и паролю используется для получения настроек из банка, поэтому на сервисе банка данный вид аутентификации должен всегда поддерживаться. В файле настроек можно задать данный способ аутентификации как основной и единственный или указать аутентификацию по закрытому ключу сертификата при дальнейшем обмене.

### <a name="baseAuthentication"></a> Аутентификация по логину и паролю

Система «1С:Предприятия 8» передает в Банк уникальный идентификатор Клиента в банковской системе (строка может содержать только ANSI-символы в соответствии с [RFC 2616](http://tools.ietf.org/html/rfc2616) для передачи значений в HTTP-заголовке) и строку, содержащую «логин + пароль» (строка «Basic логин:пароль» в формате BASE64 в соответствии с [Basic access authentication](http://tools.ietf.org/html/rfc2617)) (отправка производится HTTP-методом POST - метод [Logon](#logon)).

Особенностью данного процесса является то, что в момент первой аутентификации уникальный идентификатор Клиента неизвестен, тогда следует передавать «0».

В случае использования однофакторной аутентификации на стороне Банка и успешного завершения процесса сервис возвращает идентификатор сессии («маркер») в «1С:Предприятия 8» в синхронном режиме (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)).



![Аутентификация по логину и паролю. Logon](../doc_imgs/Logon.png)



В случае использования двухфакторной аутентификации на стороне Банка (применение OTP – «one time password») и успешного начала процесса аутентификации сервис в синхронном режиме возвращает в «1С:Предприятия 8» идентификатор сессии, пока еще неавторизованной на стороне Банка, и признак, что требуется расширенная аутентификация (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)), а также направляет OTP на мобильный телефон Клиента.



«1С:Предприятии 8» выводит форму с текстом, содержащим идентификатор неавторизованной сессии и, возможно, маску мобильного телефона Клиента (если Банк ее прислал), и предлагает пользователю ввести OTP.



Система «1С:Предприятия 8» передает в Банк уникальный идентификатор Клиента в банковской системе и введенный OTP (отправка производится HTTP-методом POST - метод LogonOTP). В случае успешного завершения процесса банковский сервис возвращает идентификатор авторизованной сессии в «1С:Предприятия 8» в синхронном режиме (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)).

Особенностью данного процесса является то, что в момент первой аутентификации уникальный идентификатор Клиента неизвестен, тогда следует передавать «0».

В дальнейшем, идентификатор авторизованной сессии будет указан в каждом HTTP-запросе для обмена данными на транспортном уровне (в заголовке SID).



Результатом неуспешной аутентификации должна быть ошибка, возвращаемая банковской системой в синхронном режиме (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)).



![Аутентификация по логину и паролю. LogonOTP](../doc_imgs/LogonOTP.png)



## <a name="logon"></a> Метод Logon (HTTP-метод POST) 

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

- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)>





**Параметры запроса:**



| Параметр      | Тип               | Кратность | Описание                                                         |
|---------------|-------------------|:---------:|------------------------------------------------------------------|
| Host          | [string](../common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| CustomerID    | [string](../common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен |
| Authorization | [base64Binary](../common-section/type-tables.md#base64Binary)      | [1]       | «логин + пароль» в соответствии с [Basic access authentication](http://tools.ietf.org/html/rfc2617)    |
| APIVersion    | [FormatVersionType](../common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |
| AvailableAPIVersion | [FormatVersionType](../common-section/type-tables.md#FormatVersionType) | [0-1]       | Максимальная доступная для текущей информационной базы версия API обмена данными                                                                                                                  |



**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](../common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |





**Пример запроса** аутентификации по **логину и паролю**:

```http

POST https://dbogate.demobank.ru/Logon HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
CustomerID: 502036
Authorization: Basic NjY5NzcxNDczMTo5MzcyMjkxMzIx
APIVersion: 2.1.1
AvailableAPIVersion: 2.2.1
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
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1">
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
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1">
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

## <a name="logonOTP"></a> Метод LogonOTP (HTTP-метод POST)

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

- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)>



**Параметры запроса:**



| Параметр   | Тип               | Кратность | Описание                                                         |
|------------|-------------------|:---------:|------------------------------------------------------------------|
| Host       | [string](../common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| SID        | [IDType](../common-section/type-tables.md#IDType)            | [1]       | Идентификатор неавторизованной сессии                            |
| CustomerID | [string](../common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен |
| OTP        | [int](../common-section/type-tables.md#int)               | [1]       | Пользовательский OTP - целые числа 6-7 знаков                    |
| APIVersion | [FormatVersionType](../common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |



**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](../common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |





**Пример запроса** передачи **OTP**:



```http

POST https://dbogate.demobank.ru/LogonOTP HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
SID: 7767755b6fbb4ae296aa0ac6b179aef9
CustomerID: 502036
OTP: 034494
APIVersion: 2.2.1
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
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1">
    <Success>
        <LogonResponse>
			<SID>8867755b6fbb4ae296aa0ac6b179ae88</SID>
        </LogonResponse>
    </Success>
</ResultBank>

```

### <a name="certAuthentication"></a> Аутентификация по закрытому ключу сертификата

Система «1С:Предприятия 8» передает в Банк уникальный идентификатор Клиента в банковской системе (строка может содержать только ANSI-символы в соответствии с [RFC 2616](http://tools.ietf.org/html/rfc2616) для передачи значений в HTTP-заголовке), а также  файл открытой части ключа электронной подписи Клиента, импортированный в систему «1С:Предприятие 8» на этапе настроек обмена (отправка производится HTTP-методом POST - метод _LogonCert,_ передается  XML-файл, соответствующий [XML-схеме данных для аутентификации по закр.ключу](#СхемаДанныхАутПоЗакрКлючу)).
Банковский сервис по идентификатору клиента, серийному номеру сертификата и имени издателя выполняет поиск сертификата электронной подписи Клиента, если находит, то возвращает уникальный идентификатор (например, идентификатор неавторизованной сессии на стороне Банка) - «маркер» (строка в формате BASE64), зашифрованный с использованием открытого ключа электронной подписи Клиента (формат зашифрованных данных - PKCS#7). Если не находит, то возвращает ошибку аутентификации (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](#СхемаОтветаБанкСервиса)).

Полученный маркер должен быть расшифрован на стороне «1С:Предприятия 8» с использованием закрытого ключа электронной подписи. В дальнейшем, расшифрованный уникальный идентификатор должен быть указан в каждом HTTP-запросе для обмена данными на транспортном уровне (в заголовке SID).


## <a name="logonCert"></a> Метод LogonCert (HTTP-метод POST) 

**Заголовки:**

- Host: <Адрес ресурса банка>

- Content-Type: application/xml; charset=utf-8

- CustomerID: <Уникальный идентификатор Клиента, содержащий только ANSI-символы.>

- APIVersion: <Версия API обмена данными> 

- AvailableAPIVersion: <Доступная версия API обмена данными>

**Тело запроса:**

Content: <XML-файл, соответствующий [XML-схеме данных для аутентификации по закр.ключу](#СхемаДанныхАутПоЗакрКлючу)>

**Успешный ответ:**

- HTTP/1.1 200 OK

- Content-Type: application/xml; charset=utf-8

- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)>


**Параметры запроса:**


| Параметр      | Тип               | Кратность | Описание                                                         |
|---------------|-------------------|:---------:|------------------------------------------------------------------|
| Host          | [string](../common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| CustomerID    | [string](../common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы. |
| APIVersion    | [FormatVersionType](../common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |
| AvailableAPIVersion | [FormatVersionType](../common-section/type-tables.md#FormatVersionType) | [0-1]       | Максимальная доступная для текущей информационной базы версия API обмена данными                                                                                                                  |


**Параметры ответа:**

| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](../common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |





### <a name="recommendations"></a> Рекомендации для банковского сервиса



Время жизни авторизованной сессии на стороне банк.сервиса рекомендуется устанавливать 5 минут, а при получении новых запросов из «1С:Предприятия 8» – автоматически его продлевать.



В случае обнаружения 3-х подряд неверных попыток аутентификации на банковском сервисе в течение 30 секунд с одного IP-адреса рекомендуется отклонять все последующие попытки аутентификации с этого IP-адреса в течение 60 секунд.



В случае обнаружения 10-ти подряд неверных попыток аутентификации с одного IP-адреса в течение 120 секунд банковскому сервису рекомендуется отклонять все последующие попытки аутентификации с этого IP-адреса в течение 240 секунд, а также проинформировать клиента банка о возникшей ситуации альтернативными способами связи (SMS-оповещением и/или письмом по эл.почте).



При отклонении в систему «1С:Предприятие 8» банковский сервис должен в синхронном режиме возвращать соответствующую ошибку (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)).



## <a name="installation"></a> Настройка обмена электронными документами

Для начала использования прямого обмена электронными документами из решений  «1С:Предприятие 8» с банковской системой Клиент должен получить параметры обмена данными из Банка – в «1С:Предприятии 8» будут автоматически созданы настройки ЭДО с Банком.

- Банковская система формирует файл настроек обмена с банком (XML-файл, соответствующий [XML-схеме настроек обмена с банком](../xsd-scheme/readme.md#1C-Bank_Settings)), содержащий параметры обмена данными с банком.
- Получение настроек проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](#transport).
- После получения файла настроек обмена система «1С:Предприятие 8» автоматически настроит систему согласно полученным параметрам.



### <a name="installationGet"></a> Получение настроек обмена с банком в автоматическом режиме


Получение настроек обмена с банком в 1С выполняется в 3 этапа:

- аутентификация Клиента на стороне Банка (если нет ранее открытой сессии);

- передача в банк расчетного счета Клиента, банк по этому номеру формирует файл настроек и отправляет обратно;

- обработка ответа банка.



## <a name="getSettings"></a> Метод  GetSettings (HTTP-метод POST)


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

- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)>





**Параметры запроса:**



| Параметр   | Тип               | Кратность | Описание                                                                                                                                   |
|------------|-------------------|:---------:|--------------------------------------------------------------------------------------------------------------------------------------------|
| Host       | [string](../common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                                                                                                        |
| CustomerID | [string](../common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен |
| Account    | [AccNumType](../common-section/type-tables.md#AccNumType)        | [0-1]       | Номер расчетного счета Клиента, для зарплатных проектов может отсутствовать                                                                                                             |
| Inn    | [string](../common-section/type-tables.md#string) (от 10 до 12)        | [1]       | Инн организации, для которой запрашиваются настройки                                                                                                             |
| Bic        | [string](../common-section/type-tables.md#string) (9)        | [1]       | БИК Банка                                                                                                                                  |
| SID        | [IDType](../common-section/type-tables.md#IDType)            | [1]       | Идентификатор авторизованной сессии                                                                                                        |
| APIVersion | [FormatVersionType](../common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                                                                                                  |
| AvailableAPIVersion | [FormatVersionType](../common-section/type-tables.md#FormatVersionType) | [0-1]       | Максимальная доступная для текущей информационной базы версия API обмена данными                                                                                                                  |

**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](../common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |



**Описание:**



При запросе настроек обмена в автоматическом режиме система «1С:Предприятие 8» передает в Банк номер расчетного счета Клиента (отправка производится HTTP-методом POST - метод [GetSettings](#getSettings)). Банковский сервис по номеру расчетному счета клиента формирует файл настроек (XML-файл, соответствующий [XML-схеме настроек обмена с банком](../xsd-scheme/readme.md#1C-Bank_Settings) и в синхронном режиме возвращается в «1С:Предприятие 8» (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)).



![Получение настроек обмена с банком в автоматическом режиме. GetSettings](../doc_imgs/GetSettings.png)



Результатом неуспешного запроса настроек обмена будет ошибка, возвращаемая банковской системой также в синхронном режиме (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)).



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
AvailableAPIVersion: 2.2.1
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
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1">
    <Success>
		<GetSettingsResponse creationDate="2015-02-19T11:21:02" formatVersion="2.2.1" id="502036">
			<Data dockind="06">PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPFNldHRpbmdzIHhtbG5zPSJo
dHRwOi8vZGlyZWN0YmFuay4xYy5ydS9YTUxTY2hlbWEiCiAgICB4bWxuczp4c2k9Imh0dHA6Ly93
d3cudzMub3JnLzIwMDEvWE1MU2NoZW1hLWluc3RhbmNlIgogICAgeG1sbnM6eHNkPSJodHRwOi8v
d3d3LnczLm9yZy8yMDAxL1hNTFNjaGVtYSIKICAgIGlkPSJFRkQ4NTdCNS03RkE4LTQxOTUtODY2
Ni0yQ0NBREJDM0M4REUiCiAgICBmb3JtYXRWZXJzaW9uPSIyLjIuMSIKICAgIGNyZWF0aW9uRGF0
ZT0iMjAxNi0wNC0yMlQwOTozODo1MSIgCiAgICB1c2VyQWdlbnQ9IkRlbW9CYW5rU2VydmljZSI+
Cgk8U2VuZGVyIGJpYz0iMDQ0NTI1ODg4IiBuYW1lPSLQlNCV0JzQni3QkdCQ0J3QmiIgLz4KICAg
IDxSZWNpcGllbnQgaWQ9IjI4MDYiIG5hbWU9ItCi0L7RgNCz0L7QstGL0Lkg0LTQvtC8INCa0L7Q
vNC/0LvQtdC60YHQvdGL0LkiIGlubj0iNzcwNTI2MDY5OSIga3BwPSI3NzA1MDEwMDEiIC8+CiAg
ICA8RGF0YT4KICAgICAgICA8Q3VzdG9tZXJJRD4yODA2PC9DdXN0b21lcklEPgogICAgICAgIDxC
YW5rU2VydmVyQWRkcmVzcz5odHRwczovL2Rib2dhdGUuZGVtb2JhbmsucnUvPC9CYW5rU2VydmVy
QWRkcmVzcz4KICAgICAgICA8Rm9ybWF0VmVyc2lvbj4yLjIuMTwvRm9ybWF0VmVyc2lvbj4KICAg
ICAgICA8RW5jb2Rpbmc+VVRGLTg8L0VuY29kaW5nPgogICAgICAgIDxMb2dvbj4KICAgICAgICAg
ICAgPExvZ2luPgogICAgICAgICAgICAgICAgPFVzZXI+dXNlcl9sb2dpbjwvVXNlcj4KICAgICAg
ICAgICAgPC9Mb2dpbj4KICAgICAgICA8L0xvZ29uPgogICAgICAgIDxEb2N1bWVudCBkb2NLaW5k
PSIwMyIgLz4KICAgICAgICA8RG9jdW1lbnQgZG9jS2luZD0iMDUiIC8+CiAgICAgICAgPERvY3Vt
ZW50IGRvY0tpbmQ9IjEwIiAvPgogICAgICAgIDxEb2N1bWVudCBkb2NLaW5kPSIxMSIgLz4JCQog
ICAgICAgIDxEb2N1bWVudCBkb2NLaW5kPSIxNCIgLz4KCQk8RG9jdW1lbnQgZG9jS2luZD0iMzAi
IC8+CiAgICA8L0RhdGE+CjwvU2V0dGluZ3M+</Data>
        </GetSettingsResponse>
	</Success>
</ResultBank>

```


### <a name="exampleSettings"></a>  Пример XML-файла настроек обмена с банком:

- [XML-файл **настроек обмена с банком**](../application-layer/Settings.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Settings xmlns="http://directbank.1c.ru/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    id="EFD857B5-7FA8-4195-8666-2CCADBC3C8DE"
    formatVersion="2.2.1"
    creationDate="2016-04-22T09:38:51" 
    userAgent="DemoBankService">
	<Sender bic="044525888" name="ДЕМО-БАНК" />
    <Recipient id="2806" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001" />
    <Data>
        <CustomerID>2806</CustomerID>
        <BankServerAddress>https://dbogate.demobank.ru/</BankServerAddress>
        <FormatVersion>2.2.1</FormatVersion>
        <Encoding>UTF-8</Encoding>
        <Logon>
            <Login>
                <User>user_login</User>
            </Login>
        </Logon>
        <Document docKind="03" />
        <Document docKind="05" />
        <Document docKind="10" />
        <Document docKind="11" />
        <Document docKind="14" />
        <Document docKind="30" />
    </Data>
</Settings>
```

## <a name="3"></a> Порядок взаимодействия на транспортном уровне



Данный уровень позволяет отправлять и получать электронные документы между системой «1С:Предприятие 8» Клиента и Банком по согласованным между сторонами обмена настройкам, используя шифрованный канал (протокол TLS 1.0/1.2) (подробнее см. раздел [«Обеспечение безопасности данных»](../common-section/data-security.md#security)).



Инициатором сеанса обмена всегда выступает система «1С:Предприятие 8».

Для отправки и получения всех электронных документов используется единый адрес ресурса банка вида: `https://<host>:<port>`.



Для передачи электронных документов между участниками обмена используется «транспортный контейнер» - XML-файл, сформированный по определенному формату ([XML-схема транспортного контейнера](../xsd-scheme/readme.md#1C-Bank_Packet)).



Отправителем и получателем транспортного контейнера могут быть как Клиент (Организация), работающий на системе «1С:Предприятие 8», так и Банк (роли участников обмена зависят от конкретной бизнес-операции).



### <a name="transportPacket"></a> Формирование и разбор транспортного контейнера



Формирование и разбор транспортного контейнер зависит от настроек обмена с конкретным банковским сервисом и может быть представлен в виде схем:



![Формирование и разбор транспортного контейнера](../doc_imgs/shipping_container_write_and_read_rules.png)





### <a name="send"></a> Отправка электронных документов из 1С



Отправка электронных документов из 1С выполняется в 3 этапа:

- формирование транспортного контейнера, содержащего электронные документы;

- аутентификация Клиента на стороне Банка (если нет ранее открытой сессии);

- отправка транспортного контейнера в Банк и обработка ответа;


![Отправка электронных документов из 1С в банковский сервис](../doc_imgs/Sending_packages_of_electronic_documents_to_the_bank.png)

При отправке электронных документов из 1С будут последовательно вызваны следующие методы банковского сервиса:
- Аутентификация:
 -	Для аутентификации по логину и паролю, только [Logon](#logon);
 -	Для аутентификации по логину и паролю с двухфакторной авторизацией — [Logon](#logon), а затем — [LogonOTP](#logonOTP).
- Отправка транспортного контейнера с данными электронных документов из 1С — [SendPack](#sendPack).


## <a name="sendPack"></a> Метод SendPack (HTTP-метод POST)


**Заголовки:**

- Host: <Адрес ресурса банка>

- Content-Type: application/xml; charset=utf-8

- CustomerID: <Уникальный идентификатор Клиента, содержащий только ANSI-символы>

- SID: <Идентификатор авторизованной сессии>

- APIVersion: <Версия API обмена данными>




**Тело запроса:**

- < XML-файл, соответствующий [XML-схеме транспортного контейнера](../xsd-scheme/readme.md#1C-Bank_Packet)>





**Успешный ответ:**

- HTTP/1.1 200 OK
- Content-Type: application/xml; charset=utf-8
- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)>





**Параметры запроса:**



| Параметр   | Тип               | Кратность | Описание                                                         |
|------------|-------------------|:---------:|------------------------------------------------------------------|
| Host       | [string](../common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| CustomerID | [string](../common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы |
| SID        | [IDType](../common-section/type-tables.md#IDType)            | [1]       | Идентификатор авторизованной сессии                              |
| APIVersion | [FormatVersionType](../common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |
| Packet     | [Packet](../common-section/type-tables.md#Packet)            | [1]       | Транспортный контейнер с данными электронных документов                                     |



**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](../common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |





**Описание:**



- В «1С:Предприятии 8» формируется транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](../xsd-scheme/readme.md#1C-Bank_Packet)) с одним или несколькими электронными документами.

 - Если настройки обмена между Клиентом и Банком предусматривают сжатие данных, то электронные документы перед помещением в транспортный контейнер сжимаются в формате zip-архива, который описан в открытой спецификации, доступной по адресу <http://www.pkware.com/documents/casestudies/APPNOTE.TXT>.

- Далее проходит аутентификация на стороне Банка согласно протоколу, описанному в разделе «[Порядок взаимодействия на уровне аутентификации](#authentication)».

- Если аутентификация пройдена успешно, транспортный контейнер передается в Банк (отправка на ресурс Банка производится HTTP-методом POST – метод SendPack), в синхронном режиме банковская система возвращает либо ошибку получения, либо уникальный идентификатор на стороне Банка, который будет однозначно соответствовать полученному транспортному контейнеру (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)).

- Банковская система может проводить контроль формата и разбор транспортного контейнера:

 - либо в синхронном режиме (сразу в момент получения);

 - либо в асинхронном режиме (ставит входящие трасп.контейнеры в очередь на разбор).

- Для такого варианта уникальный идентификатор возвращается сразу, после того, как транспортный контейнер успешно был поставлен в очередь на разбор, не дожидаясь самого разбора.

- Далее запускается процесс разбора очереди входящих транспортных контейнеров банковской системой, результатом которого будет ответ о состоянии обработки (XML-файл, соответствующий [XML-схеме извещения о состояния обработки транспортного контейнера](../xsd-scheme/readme.md#1C-Bank_StatusPacketNotice)).

- Подготовленный ответ Банка упаковывается в транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](../xsd-scheme/readme.md#1C-Bank_Packet)) и ожидает на стороне Банка очередного запроса на наличие подготовленных к передаче транспортных контейнеров из «1С:Предприятие 8».

- Параллельно с процессом отправки ответа о состоянии обработки транспортного контейнера банковская система выполняет обработку каждого электронного документа, входящего в него.



![Отправка электронных документов из 1С. SendPack](../doc_imgs/SendPack.png)



**Пример отправки** транспортного контейнера:

```xml

POST https://dbogate.demobank.ru/SendPack HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
SID: 8867755b6fbb4ae296aa0ac6b179ae88
CustomerID: 502036
APIVersion: 2.2.1
User-Agent: 1C+Enterprise/8.3
Content-Type: application/xml; charset=utf-8
Content-Length: 5239

<?xml version="1.0" encoding="UTF-8"?>
<Packet xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    id="9bad1c35-b7ff-11e4-9a88-0003ffb697db"
    formatVersion="2.2.1"
    creationDate="2016-04-19T11:12:31"
    userAgent="1С - БЭД: 1.4">
	<Sender>
		<Customer id="502036" name="ИП Петрович и Ко" inn="761700021132"/>
	</Sender>
	<Recipient>
		<Bank bic="044525888" name="ДЕМО-БАНК"/>
	</Recipient>
	<Document id="a64225eb-9737-4d80-bd9d-1ffe5fdb63b1" dockind="10" formatVersion="2.2.1">
		<Data>
PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4NCjxQYXlEb2NSdSB4bWxucz0i
aHR0cDovL2RpcmVjdGJhbmsuMWMucnUvWE1MU2NoZW1hIiANCgl4bWxuczp4cz0iaHR0cDovL3d3
dy53My5vcmcvMjAwMS9YTUxTY2hlbWEiIA0KICAgIHhtbG5zOnhzaT0iaHR0cDovL3d3dy53My5v
cmcvMjAwMS9YTUxTY2hlbWEtaW5zdGFuY2UiIA0KICAgIGlkPSIwNTY4ODA5Ni0wODA2LTRlZjAt
YWY0Yy01NzJmNzVkYmFmN2MiIA0KICAgIGZvcm1hdFZlcnNpb249IjIuMi4xIiANCiAgICBjcmVh
dGlvbkRhdGU9IjIwMTYtMDQtMjJUMDk6Mzg6NTEiIA0KICAgIHVzZXJBZ2VudD0iMdChIC0g0JHQ
rdCUOiAxLjQuMS4xOyDQkdC40LHQu9C40L7RgtC10LrQsNCt0LvQtdC60YLRgNC+0L3QvdGL0YXQ
lNC+0LrRg9C80LXQvdGC0L7QsjogMS40LjEuMSI+DQogICAgPFNlbmRlciBpZD0iaWQ6NDI7czo5
OTk5IiBuYW1lPSLQotC+0YDQs9C+0LLRi9C5INC00L7QvCDQmtC+0LzQv9C70LXQutGB0L3Ri9C5
IiBpbm49Ijc3MDUyNjA2OTkiIGtwcD0iNzcwNTAxMDAxIi8+DQogICAgPFJlY2lwaWVudCBiaWM9
IjA0NDUyNTg4OCIgbmFtZT0i0JTQldCc0J4t0JHQkNCd0JoiLz4NCgk8RGF0YT4NCiAgICAgICAg
PERvY05vPjE0PC9Eb2NObz4NCiAgICAgICAgPERvY0RhdGU+MjAxNi0wNC0yMjwvRG9jRGF0ZT4N
CiAgICAgICAgPFN1bT4xNTwvU3VtPg0KICAgICAgICA8UGF5ZXI+DQogICAgICAgICAgICA8TmFt
ZT7QotC+0YDQs9C+0LLRi9C5INC00L7QvCAi0JrQvtC80L/Qu9C10LrRgdC90YvQuSI8L05hbWU+
DQogICAgICAgICAgICA8SU5OPjc3MDUyNjA2OTk8L0lOTj4NCiAgICAgICAgICAgIDxLUFA+Nzcw
NTAxMDAxPC9LUFA+DQogICAgICAgICAgICA8QWNjb3VudD40MDcwMjgxMDgxMzEyMzEyMzIyMjwv
QWNjb3VudD4NCiAgICAgICAgICAgIDxCYW5rPg0KICAgICAgICAgICAgICAgIDxCSUM+MDQ0NTI1
ODg4PC9CSUM+DQogICAgICAgICAgICAgICAgPE5hbWU+0JTQldCc0J4t0JHQkNCd0Jo8L05hbWU+
DQogICAgICAgICAgICAgICAgPENvcnJlc3BBY2M+MzAxMDE4MTA1MDAwMDAwMDAyMTk8L0NvcnJl
c3BBY2M+DQogICAgICAgICAgICA8L0Jhbms+DQogICAgICAgIDwvUGF5ZXI+DQogICAgICAgIDxQ
YXllZT4NCiAgICAgICAgICAgIDxOYW1lPtCe0J7QniAi0JrQsNC90YbRgtC+0LLQsNGA0YsiPC9O
YW1lPg0KICAgICAgICAgICAgPElOTj43NzA0NTk2MTgxPC9JTk4+DQogICAgICAgICAgICA8S1BQ
Pjc3MDQwMTAwMTwvS1BQPg0KICAgICAgICAgICAgPEFjY291bnQ+NDA3MDI4MTA0MDEyMDAwMDAw
MzU8L0FjY291bnQ+DQogICAgICAgICAgICA8QmFuaz4NCiAgICAgICAgICAgICAgICA8QklDPjA0
NDUyNTk5OTwvQklDPg0KICAgICAgICAgICAgICAgIDxOYW1lPtCU0JXQnNCeLdCR0JDQndCaMjwv
TmFtZT4NCiAgICAgICAgICAgICAgICA8Q29ycmVzcEFjYz4zMDEwMTgxMDIwMDAwMDAwMDU5Mzwv
Q29ycmVzcEFjYz4NCiAgICAgICAgICAgIDwvQmFuaz4NCiAgICAgICAgPC9QYXllZT4NCiAgICAg
ICAgPFRyYW5zaXRpb25LaW5kPjAxPC9UcmFuc2l0aW9uS2luZD4NCiAgICAgICAgPFByaW9yaXR5
PjM8L1ByaW9yaXR5Pg0KICAgICAgICA8UHVycG9zZT7Qt9CwINGC0L7QstCw0YA8L1B1cnBvc2U+
DQoJPC9EYXRhPg0KPC9QYXlEb2NSdT4=
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
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1">
    <Success>
        <SendPacketResponse>
			<ID>50214584626</ID>
        </SendPacketResponse>
    </Success>
</ResultBank>

```



### <a name="get"></a> Получение электронных документов в 1С



Получение электронных документов в 1С выполняется в 3 этапа:

- аутентификация Клиента на стороне Банка (если нет ранее открытой сессии);

- запрос у Банка списка подготовленных к передаче транспортных контейнеров, содержащих электронные документы для Клиента;

- запрос у Банка транспортного контейнера по его уникальному идентификатору и разбор в 1С.


![Получение электронных документов из банковского сервиса в 1С](../doc_imgs/Receive_packages_of_electronic_documents_from_the_bank.png)

При получении электронных документов из банковского сервиса в 1С будут последовательно вызваны следующие методы:
- Аутентификация:
 - Для аутентификации по логину и паролю, только [Logon](#logon);
 - Для аутентификации по логину и паролю с двухфакторной авторизацией — [Logon](#logon), а затем — [LogonOTP](#logonOTP).
- Запрос на получение списка GUID транспортных контейнеров, готовых к отправке клиенту банком, — [GetPackList](#getPackList).
- В цикле запрос на получение транспортного контейнера по GUID из ранее полученного списка [GetPack](#getPack)

Если после получения и разбора всех контейнеров из списка, 1С не находит нужный контейнер, процесс будет начат заново. Это происходит при ожидании получения выписки банка.


## <a name="getPackList"></a> Метод GetPackList (HTTP-метод GET)



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
- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)>





**Параметры запроса:**



| Параметр   | Тип               | Кратность | Описание                                                         |
|------------|-------------------|:---------:|------------------------------------------------------------------|
| Host       | [string](../common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| CustomerID | [string](../common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы |
| SID        | [IDType](../common-section/type-tables.md#IDType)            | [1]       | Идентификатор авторизованной сессии                              |
| APIVersion | [FormatVersionType](../common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |



**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](../common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |



**Важно:**

- Обратите внимание на параметр ***TimeStampLastPacket*** в ответе, это метка времени, на которую вернули всю актуальную информацию.





**Описание:**



- Проходит аутентификация на стороне Банка согласно протоколу, описанному в разделе «[Порядок взаимодействия на уровне аутентификации](#authentication)».

- Если аутентификация пройдена успешно, система «1С:Предприятия 8» запрашивает Банк на наличие подготовленных транспортных контейнеров для Клиента (запрос на ресурс Банка производится HTTP-методом GET – метод GetPackList). В синхронном режиме банковская система возвращает список уникальных идентификаторов транспортных контейнеров, готовых к передаче со стороны Банка и отметку времени, равную дате и времени формирования (передаются значения даты и времени сервера Банка) самого последнего транспортного контейнера, входящего в этот список (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)).

 - Если в запросе из «1С:Предприятия 8» о готовых к передаче транспортных контейнерах указать «Отметку времени» (указывается значение даты и времени сервера Банка с точностью до секунды), то именно с этого момента времени будет формироваться список уникальных идентификаторов (в формате GUID), подготовленных к передаче в хронологическом порядке.



   Для того, чтобы получить список GUID всех когда-либо сформированных транспортных контейнеров на банковской стороне, параметр «Отметка времени» в запросе не передается.


**Пример запроса** получения списка уникальных идентификаторов транспортных контейнеров:

```http

GET https://dbogate.demobank.ru/GetPackList?date=16.02.2015%2011:25:32 HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
SID: 8867755b6fbb4ae296aa0ac6b179ae88
CustomerID: 502036
APIVersion: 2.2.1
User-Agent: 1C+Enterprise/8.3

```



**Пример успешного ответа** на запрос получения списка уникальных идентификаторов:

```xml

HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 145


<?xml version="1.0" encoding="UTF-8"?>
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1">
    <Success>
        <GetPacketListResponse TimeStampLastPacket="2015-02-19T11:15:42">
			<PacketID>50214585876</PacketID>
        </GetPacketListResponse>
    </Success>
</ResultBank>

```

Система «1С:Предприятие 8» после получения списка GUID готовых к передаче транспортных контейнеров запрашивает конкретный транспортный контейнер по его GUID (запрос на ресурс Банка производится HTTP-методом GET – метод [GetPack](#getPack)). В синхронном режиме банковская система возвращает либо ошибку, либо транспортный контейнер (XML-файл, соответствующий XML-схеме ответа банк.сервиса).

Далее происходит разбор содержимого транспортного контейнера уже в системе «1С:Предприятие 8».





## <a name="getPack"></a> Метод GetPack (HTTP-метод GET)



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
- Content: < XML-файл, соответствующий [XML-схеме ответа банк.сервиса](../xsd-scheme/readme.md#1C-Bank_ResultBank)>



**Параметры запроса:**



| Параметр   | Тип               | Кратность | Описание                                                         |
|------------|-------------------|:---------:|------------------------------------------------------------------|
| Host       | [string](../common-section/type-tables.md#string)            | [1]       | Адрес ресурса банка                                              |
| CustomerID | [string](../common-section/type-tables.md#string)            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы |
| SID        | [IDType](../common-section/type-tables.md#IDType)            | [1]       | Идентификатор авторизованной сессии                              |
| APIVersion | [FormatVersionType](../common-section/type-tables.md#FormatVersionType) | [1]       | Версия API обмена данными                                        |





**Параметры ответа:**



| Параметр                         | Тип               | Кратность | Описание                        |
|----------------------------------|-------------------|:---------:|---------------------------------|
| ResultBank  |  [ResultBank](../common-section/type-tables.md#ResultBank)                 |   [1]                | Ответ банка                                |



**Пример запроса** получения транспортного контейнера:

```http

GET https://dbogate.demobank.ru/GetPack?id=50214585876 HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
SID: 8867755b6fbb4ae296aa0ac6b179ae88
CustomerID: 502036
APIVersion: 2.2.1
User-Agent: 1C+Enterprise/8.3

```



**Пример успешного ответа** на запрос получения транспортного контейнера:

```xml

HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 2502

<?xml version="1.0" encoding="UTF-8"?>
<ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1">
    <Success>
		<GetPacketResponse userAgent="Back Office"
        creationDate="2015-02-19T11:15:42"
        formatVersion="2.2.1"
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
        	formatVersion="2.2.1"
        	dockind="01"
        	id="0f6d6032-31b0-8337-e053-248c410a1832">
        		<Data contentType="text/xml"fileName="">
PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPFN0YXR1c1BhY2tldE5vdGlj
ZSB4bWxucz0iaHR0cDovL2RpcmVjdGJhbmsuMWMucnUvWE1MU2NoZW1hIiBpZD0iMGY2ZDYwMzIt
MzFiMC04MzM3LWUwNTMtMjQ4YzQxMGExODMyIiBmb3JtYXRWZXJzaW9uPSIyLjIuMSIgY3JlYXRp
b25EYXRlPSIyMDE1LTAyLTE5VDExOjAzOjQ4Ij4KICA8U2VuZGVyPgogICAgPEJhbmsgYmljPSIw
NDQ1MjU5ODUiLz4KICA8L1NlbmRlcj4KICA8UmVjaXBpZW50PgogICAgPEN1c3RvbWVyIGlkPSI1
MDIwMzYyNzEyMSIvPgogIDwvUmVjaXBpZW50PgogIDxJRFJlc3VsdFN1Y2Nlc3NSZXNwb25zZT41
MDIxNDU4MDgzODwvSURSZXN1bHRTdWNjZXNzUmVzcG9uc2U+CiAgPFJlc3VsdD4KICAgIDxTdGF0
dXM+CiAgICAgIDxDb2RlPjAxPC9Db2RlPgogICAgICA8TmFtZT7Qn9GA0LjQvdGP0YI8L05hbWU+
CiAgICA8L1N0YXR1cz4KICA8L1Jlc3VsdD4KPC9TdGF0dXNQYWNrZXROb3RpY2U+Cg==
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
### <a name="test"></a> Проверка работоспособности обмена электронными документами.
- На этапе отладки или профилактических работ часто возникает потребность проверить доступность банковского сервиса, не приступая к штатному обмену электронными документами. Для таких целей предназначен специальный вид электронного документа.
- По команде в «1С:Предприятии 8» формируется электронный документ «Запрос-зонд» (XML-файл, соответствующий [XML-схеме запроса-зонда](../xsd-scheme/readme.md#1C-Bank_Probe)).
- Если используется электронная подпись на стороне «1С:Предприятия 8» (см. раздел [«Обеспечение безопасности данных»](../common-section/data-security.md#security)), то система предложит пользователю подписать электронный документ.
- Электронный документ «Запрос-зонд» с электронной подписью (если используется, см. раздел [«Обеспечение безопасности данных»](../common-section/data-security.md#security)) помещаются в транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](../xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне).
- Далее передача данных в Банк проходит согласно протоколу, описанному в разделе «[Порядок взаимодействия на транспортном уровне](#transport)».
- Если отправка прошла успешно, то система «1С:Предприятие 8» изменит статус электронного документа «Запрос-зонд» на «Отправлен».
- После получения из Банка ответа по результатам обработки транспортного контейнера система «1С:Предприятие 8» назначит электронному документу запроса статус «Доставлен».
- По результатам контроля и обработки запроса-зонда на стороне Банка всегда формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](../xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), который содержит:
 - либо ошибку обработки запроса ([Error](../common-section/type-tables.md#ErrorType)) с соответствующим описанием исключительной ситуации;
 - либо ([Status](../common-section/type-tables.md#StatusType)) код «01», означающий что проверка успешно завершена.
- Если используется электронная подпись (см. раздел [«Обеспечение безопасности данных»](../common-section/data-security.md#security)), то электронный документ «Извещение о состоянии электронного документа» подписывается.
- Банковская система формирует транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](../xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне) и ставит в очередь на передачу в «1С:Предприятие 8».
- Далее получение данных из Банка проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](#transport).
- Статус запроса-зонда система «1С:Предприятие 8» сообщит пользователю после успешного разбора входящего транспортного контейнера из Банка.

#### <a name="zond"></a> Пример XML-файла запроса-зонда

- [XML-файл **запроса-зонда**](../application-layer/Probe.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Probe xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	id="eafa28b5-6600-424c-b0d3-3785274d570d"
	formatVersion="2.2.1"
	creationDate="2016-04-22T09:33:57"
	userAgent="1С - БЭД: 1.4.1.1; БиблиотекаЭлектронныхДокументов: 1.4.1.1">
	<Sender id="id:42;s:9999" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001"/>
	<Recipient bic="044525888" name="ДЕМО-БАНК "/>
</Probe>
```


### <a name="5"></a> Процедура синхронизации 1С с банковским сервисом

При синхронизации 1С с банковским сервисом последовательно выполняются процедуры отправки всех подготовленных в 1С транспортных контейнеров с данными электронных документов в банк и получения всех транспортных контейнеров документов из банка.

[Процедура отправки в банк](#send) выполняется в цикле. Для каждого подготовленного к отправке транспортного контейнера будут вызваны следующие методы:
- Аутентификация:
 -	Для аутентификации по логину и паролю, только [Logon](#logon);
 -	Для аутентификации по логину и паролю с двухфакторной авторизацией — [Logon](#logon), а затем — [LogonOTP](#logonOTP).
- Отправка транспортного контейнера с данными электронных документов из 1С — [SendPack](#sendPack).


При [получении электронных документов из банковского сервиса в 1С](#get) будут последовательно вызваны следующие методы:
- Аутентификация:
 - Для аутентификации по логину и паролю, только [Logon](#logon);
 - Для аутентификации по логину и паролю с двухфакторной авторизацией — [Logon](#logon), а затем — [LogonOTP](#logonOTP).
- Запрос на получение списка GUID транспортных контейнеров, готовых к отправке клиенту банком — [GetPackList](#getPackList).
- Для каждого GUID из ранее полученного списка запрос на получение транспортного контейнера — [GetPack](#getPack).

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjM4MDI3MDc2LDE3ODQ1OTE3NjcsLTIxMz
Q5MzQ4MjQsLTEyODUxMjM0MzIsLTE1OTA3NjQwMzMsLTY5NjM5
MzQzOF19
-->