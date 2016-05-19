#API обмена данными с банком из 1С

API обмена данными – уровень который описывает подходы и методы осуществления операций обмена данными, он в свою очередь делится на следующие уровни:
- Уровень аутентификации;
- Транспортный уровень

- - -

+ [Порядок взаимодействия на уровне аутентификации](#1)
 + [Аутентификация по закрытому ключу электронной подписи](#1.1)
   + [Метод **LogonCert** (HTTP-метод POST)](#1.1.1)
 + [Аутентификация по логину и паролю](#1.2)
   + [Метод **Logon** (HTTP-метод POST)](#1.2.1)
   + [Метод **LogonOTP** (HTTP-метод POST)](#1.2.2)
 + [Рекомендации для банковского сервиса](#1.3)
+ [Порядок взаимодействия на транспортном уровне](#2)
 + [Формирование и разбор транспортного контейнера](#2.1)
 + [Получение настроек обмена с банком в автоматическом режиме (**GetSettings**)](#2.2)
 + [Отправка электронных документов из 1С](#2.3)
   + [Метод **SendPack** (HTTP-метод POST)](#2.3.1)
 + [Получение электронных документов в 1С](#2.4)
   + [Метод **GetPackList** (HTTP-метод GET)](#2.4.1)
   + [Метод **GetPack** (HTTP-метод GET)](#2.4.2)
+ [Обеспечение безопасности данных](#3)
 + [Шифрование данных при передаче](#3.1)
 + [Криптооперации, выполняемые на стороне Клиента и Банка](#3.2)
 + [Защита электронных документов с помощью электронной подписи](#3.3)
+ [Таблица кодов статусов транспортных контейнеров](#4)
+ [Таблица кодов видов электронных документов](#5)
+ [Таблица кодов статусов электронных документов](#6)
+ [Таблица типов выписок банка](#7)
+ [Таблица кодов ошибок и их описание, которые может возвращать банковский сервис в «1С:Предприятие 8»](#8)


## <a name="1"></a> Порядок взаимодействия на уровне аутентификации.

Аутентификация Клиента с целью создания сессии на сервисе Банка может проходить:
- либо по закрытому ключу электронной подписи (LogonCert);
- либо по логину и паролю + дополнительный OTP («one time password», опционально) (Logon).

### <a name="1.1"></a> Аутентификация по закрытому ключу электронной подписи

Система «1С:Предприятия 8» передает в Банк уникальный идентификатор Клиента в банковской системе (строка может содержать только ANSI-символы в соответствии с [RFC 2616](http://tools.ietf.org/html/rfc2616) для передачи значений в HTTP-заголовке), а также серийный номер сертификата, имя издателя сертификата и файл открытой части ключа электронной подписи Клиента, импортированный в систему «1С:Предприятие 8» на этапе настроек обмена (отправка производится HTTP-методом POST - метод LogonCert, передается XML-файл, соответствующий XML-схеме данных для аутентификации по закр.ключу).

#### <a name="1.1.1"></a> Метод LogonCert (HTTP-метод POST)

**Заголовки:**
- Host: <Адрес ресурса банка>
- Content-Type: application/xml; charset=utf-8
- CustomerID: <Уникальный идентификатор Клиента, содержащий только ANSI-символы>
- APIVersion: <Версия API обмена данными>

**Тело запроса:**
- Content: < XML-файл, соответствующий XML-схеме данных для аутентификации по закр.ключу>


**Успешный ответ:**
- HTTP/1.1 200 OK
- Content-Type: application/xml; charset=utf-8
- Content: < XML-файл, соответствующий XML-схеме ответа банк.сервиса>

**Параметры:**

| Параметр         | Тип               | Кратность | Описание                                                              |
|------------------|-------------------|-----------|-----------------------------------------------------------------------|
| Host             | string            | [1]       | Адрес ресурса банка                                                   |
| CustomerID       | string            | [1]       | Уникальный идентификатор Клиента, содержащий только ANSI-символы      |
| APIVersion       | FormatVersionType | [1]       | Версия API обмена данными                                             |
| id               | IDType            | [1]       | Идентификатор набора данных                                           |
| formatVersion    | FormatVersionType | [1]       | Версия формата                                                        |
| creationDate     | dateTime          | [1]       | Дата и время формирования                                             |
| userAgent        | UserAgentType     | [0-1]     | Наименование и версия программы                                       |
| X509IssuerName   | string            | [1]       | Имя издателя сертификата электронной подписи  (значение атрибута"CN") |
| X509SerialNumber | hexBinary         | [1]       | Серийный номер сертификата электронной подписи                        |
| X509Certificate  | base64Binary      | [1]       | Двоичные данные сертификата электронной подписи                       |

**Описание:**

Система «1С:Предприятия 8» передает в Банк уникальный идентификатор Клиента в банковской системе (строка может содержать только ANSI-символы в соответствии с RFC 2616 для передачи значений в HTTP-заголовке), а также серийный номер сертификата, имя издателя сертификата и файл открытой части ключа электронной подписи Клиента, импортированный в систему «1С:Предприятие 8» на этапе настроек обмена (отправка производится HTTP-методом POST - метод LogonCert, передается XML-файл, соответствующий XML-схеме данных для аутентификации по закр.ключу).

Банковский сервис по идентификатору клиента, серийному номеру сертификата и имени издателя выполняет поиск сертификата электронной подписи Клиента (при необходимости получить другие данные по сертификату задействует файл сертификата открытой части ключа), если находит, то возвращает уникальный идентификатор (например, идентификатор неавторизованной сессии на стороне Банка) - «маркер» (строка в формате BASE64), зашифрованный с использованием открытого ключа электронной подписи Клиента (формат зашифрованных данных - PKCS#7). Если не находит, то возвращает ошибку аутентификации (XML-файл, соответствующий XML-схеме ответа банк.сервиса). 

Полученный маркер должен быть расшифрован на стороне «1С:Предприятия 8» с использованием закрытого ключа электронной подписи. В дальнейшем, расшифрованный уникальный идентификатор должен быть указан в каждом HTTP-запросе для обмена данными на транспортном уровне (в заголовке SID).

![Аутентификация по закрытому ключу электронной подписи](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/LogonCert.png)

**Пример запроса** аутентификации по закрытому ключу:
```xml
POST https://dbogate.demobank.ru/LogonCert HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
CustomerID: 502036
APIVersion: 2.1.1
User-Agent: 1C+Enterprise/8.3
Content-Type: application/xml; charset=utf-8
Content-Length: 2294

<?xml version="1.0" encoding="UTF-8"?>
<X509Data xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    id="d50eb1dd-52cc-487f-a504-9a2e31b9b741"
    formatVersion="2.1.1"
    creationDate="2015-02-19T09:28:03"
    userAgent="1С - БЭД: 1.3">
	<X509IssuerName>Удостоверяющий Центр Банка</X509IssuerName>
	<X509SerialNumber>022C03015B03010F022FE2</X509SerialNumber>
	<X509Certificate>
	MIIEvDCCBF2gAwIBAgILAWwDAVsDAQ8CH+YwDgYKKwYBBAGtWQEDAgUAMIHuMQs
	wCQYDVQQGEwJSVTEVMBMGA1UECB4MBBwEPgRBBDoEMgQwMRUwEwYDVQQHHgwEHAQ+
	BEEEOgQyBDAxNTAzBgNVBAoeLAQeBBAEHgAgBBEEMAQ9BDoAIAAiBCQEGgAgBB4E
	QgQ6BEAESwRCBDgENQAiMV8wXQYDVQQDHlYEIwQ0BD4EQQRCBD4EMgQ1BEAETwRO
	BEkEOAQ5ACAEJgQ1BD0EQgRAACAEHgQQBB4AIAQRBDAEPQQ6ACAAIgQkBBoAIAQe
	BEIEOgRABEsEQgQ4BDUAIjEZMBcGCSqGSIb3DQEJARYKcGtpQG9mYy5ydTAeFw0x
	NDEwMDYwNzEwMzJaFw0xNTEyMTAwNzEwMzJaMIHaMQswCQYDVQQGEwJSVTEVMBMG
    OARHACAEOAAgBBoEPjEPMA0GA1UECx4GAEQAQgBPMUUwQwYDVQQMHjwEGAQ9BDQE
    OAQyBDgENARDBDAEOwRMBD0ESwQ5ACAEPwRABDUENAQ/BEAEOAQ9BDgEPAQwBEIE
    NQQ7BEwxMTAvBgNVBAMeKAQfBDUEQgRABD4EMgAgBB8ENQRCBEAAIAQfBDUEQgRA
    BD4EMgQ4BEcwXjAWBgorBgEEAa1ZAQYCBggqhkjOPQMBBwNEAARBBK8x6CgOZbbQ
    Gvx5YBmKl4VtZ9RHsnt3+l0KI7ykzDaV8dfVWN63txNqIbS9CHCYP5MYg6H4WBEc
    GQxlc1UOXUOjggHpMIIB5TAdBgNVHQ4EFgQULxBfQTHaVCpIFkNW6OkJx5cIb/Aw
    ggEkBgNVHSMEggEbMIIBF4AUSd9T5T22FdM98MBt3ZnZOetl9lmhgfSkgfEwge4x
    CzAJBgNVBAYTAlJVMRUwEwYDVQQIHgwEHAQ+BEEEOgQyBDAxFTATBgNVBAceDAQc
    BD4EQQQ6BDIEMDE1MDMGA1UECh4sBB4EEAQeACAEEQQwBD0EOgAgACIEJAQaACAE
    HgRCBDoEQARLBEIEOAQ1ACIxXzBdBgNVBAMeVgQjBDQEPgRBBEIEPgQyBDUEQARP
    BE4ESQQ4BDkAIAQmBDUEPQRCBEAAIAQeBBAEHgAgBBEEMAQ9BDoAIAAiBCQEGgAg
    BB4EQgQ6BEAESwRCBDgENQAiMRkwFwYJKoZIhvcNAQkBFgpwa2lAb2ZjLnJ1gggB
    bAEBAQ8BAjALBgNVHQ8EBAMCA/gwgY4GA1UdIASBhjCBgzCBgAYKKwYBBAGCnAUB
    ATByMDEGCCsGAQUFBwIBFiVodHRwOi8vd3d3Lm9mYy5ydS9hYm91dC9jYS1yZWds
    YW1lbnQvMD0GCCsGAQUFBwICMDEaL9Ho8fLl7Psg5Ojx8uDt9uju7e3u4+4g4eDt
    6u7i8eru4+4g7uHx6/Pm6OLg7ej/MA4GCisGAQQBrVkBAwIFAANJADBGAiEAw/ML
	etfXiu0fRDsnvI4BXVE6Yw==
	</X509Certificate>
</X509Data>
```

**Пример успешного ответа** на запрос аутентификации по закрытому ключу:
```xml
HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 1145

<?xml version="1.0" encoding="UTF-8"?>
<ResultBank xmlns="http://directbank.1c.ru/XMLSchema" formatVersion="2.1.1">
    <Success>
        <LogonCertResponse>
        	<EncryptedSID>
       		    MIAGCSqGSIb3DQEHA6CAMIACAQAxggHTMIIBzwIBADCB/jCB7jELMAkGA
  	    	    1UEBhMCUlUxFTATBgNVBAgeDAQcBD4EQQQ6BDIEMDEVMBMGA1UEBx4MBB
	            wEPgRBBDoEMgQwMTUwMwYDVQQKHiwEHgQQBB4AIAQRBDAEPQQ6ACAAIgQ
	            kBBoAIAQeBEIEOgRABEsEQgQ4BDUAIjFfMF0GA1UEAx5WBCMENAQ+BEEE
	            QgQ+BDIENQRABE8ETgRJBDgEOQAgBCYENQQ9BEIEQAAgBB4EEAQeACAEE
	            QQwBD0EOgAgACIEJAQaACAEHgRCBDoEQARLBEIEOAQ1ACIxGTAXBgkqhk
	            iG9w0BCQEWCnBraUBvZmMucnUCCwFsAwFbAwEPAh/mMBYGCisGAQQBrVk
	            BBgIGCCqGSM49AwEHBIGwMIGtAgEDoFihVjAOBgorBgEEAa1ZAQYCBQAD
	            RAAEQQQotO2E39p4EZFUBdAcBe7AYYtq5Yub7FmoP07IhTDMI+nRlGJt+
	            95CZiwu7IUAlR0ldudpUOIeNCDAswUSUQzDMBwGCisGAQQBrVkBCAMwDg
	            YKKwYBBAGtWQEEAgUABDCQRpcoionI87dfDGkiG4mTUla34G1soJF8vj8
	            P1hyRX+ugfvCkjj7/zmVP39kwDrUwgAYJKoZIhvcNAQcBMB0GBiqFAwIC
            	FTATBAiZQUbDcZec7gYHKoUDAgIfAaCABEBvGw18jSvnUNSSphPzS6aa7
            	C+BIkUkU139E6Owmnpjz9sb1sWzhSnb3GX8GJIhaamBewT4BWyb2dwgEQ
                2YQywHAAAAAAAAAAAAAA==
            </EncryptedSID>
        </LogonCertResponse>
    </Success>
</ResultBank>
```