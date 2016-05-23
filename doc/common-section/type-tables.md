# ВНИМАНИЕ! Это не финальная версия, раздел может быть дополнен.

#Приложение. Таблицы типов

+ [Таблица типов **edo**](#1)
+ [Таблица типов **W3C**](#2)
+ [Таблицы **общих комплексных типов edo**](#3)
 + [Тип edo:ResultBank](#edo-ResultBank)
 + [Тип edo:ErrorType](#edo-ErrorType)
 + [Тип edo:SuccessResultType](#edo-SuccessResultType)
 + [Тип edo:SendPacketResponseType](#edo-SendPacketResponseType)
 + [Тип edo:GetPacketListResponseType](#edo-GetPacketListResponseType)
 + [Тип edo:LogonResponseType](#edo-LogonResponseType)
 + [Тип edo:LogonCertResponseType](#edo-LogonCertResponseType)
 + [Тип edo:GetSettingsResponse](#edo-GetSettingsResponse)
 + [Тип edo:Packet](#edo-Packet)
 + [Тип edo:ParticipantType](#edo-ParticipantType)
 + [Тип edo:BankPartyType](#edo-BankPartyType)
 + [Тип edo:CustomerPartyType](#edo-CustomerPartyType)
 + [Тип edo:DocumentType](#edo-DocumentType)
 + [Тип edo:DigestType](#edo-DigestType)
 + [Тип edo:ResultStatusType](#edo-ResultStatusType)
 + [Тип edo:StatusType](#edo-StatusType)
 + [Тип edo:PayDocRuApp](#edo-PayDocRuApp)
 + [Тип edo:PayRequestApp](#edo-PayRequestApp)
 + [Тип edo:PaymentDataType](#edo-PaymentDataType)
 + [Тип edo:CustomerDetailsType](#edo-CustomerDetailsType)
 + [Тип edo:BankType](#edo-BankType)


## <a name="1"></a> Таблица типов edo
(<http://directbank.1c.ru/XMLSchema>)

| Наименование              | Тип                           | Описание                                                                                                                    |
|---------------------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| <a name="IDType"></a> IDType                    | [string](#string)                        | Уникальный идентификатор                                                                                                    |
| <a name="FormatVersionType"></a> FormatVersionType         | [string](#string) (до 12)                | Версия формата                                                                                                              |
| <a name="UserAgentType"></a> UserAgentType             | [string](#string) (до 100)               | Версия ПО                                                                                                                   |
| <a name="ResultBank"></a> ResultBank                | [edo:ResultBank](#edo-ResultBank)                | Ответ банка                                                                                                                 |
| <a name="ErrorType"></a> ErrorType                 | [edo:ErrorType](#edo-ErrorType)                 | Ответ в случае возникновения ошибки                                                                                         |
| <a name="SuccessResultType"></a> SuccessResultType         | [edo:SuccessResultType](#edo-SuccessResultType)         | Успешный ответ банка                                                                                                        |
| <a name="SendPacketResponseType"></a> SendPacketResponseType    | [edo:SendPacketResponseType](#edo-SendPacketResponseType)    | Отправка пакета в банк                                                                                                      |
| <a name="Packet"></a> Packet                    | [edo:Packet](#edo-Packet)                    | Пакет электронных документов                                                                                                |
| <a name="GetPacketListResponseType"></a> GetPacketListResponseType | [edo:GetPacketListResponseType](#edo-GetPacketListResponseType) | Список ID пакетов, готовых к передачи клиенту                                                                               |
| <a name="GetPacketResponse"></a> GetPacketResponse         | [edo:Packet](#edo-Packet)                    | Пакет электронных документов для получения клиентом                                                                         |
| <a name="LogonResponseType"></a> LogonResponseType         | [edo:LogonResponseType](#edo-LogonResponseType)         | Аутентификация по логину + ОТР (опционально)                                                                                |
| <a name="LogonCertResponseType"></a> LogonCertResponseType     | [edo:LogonCertResponseType](#edo-LogonCertResponseType)     | Аутентификация по сертификату                                                                                               |
| <a name="GetSettingsResponseType"></a> GetSettingsResponseType   | [edo:GetSettingsResponseType](#edo-GetSettingsResponseType)   | Получение настроек обмена в автоматическом режиме                                                                           |
| <a name="ParticipantType"></a> ParticipantType           |[ edo:ParticipantType](#edo-ParticipantType)           | Одна из сторон, принимающая участие в обмене электронными документами (Участник)                                            |
| <a name="BankPartyType"></a> BankPartyType             | [edo:BankPartyType](#edo-BankPartyType)             | Отправитель                                                                                                                 |
| <a name="CustomerPartyType"></a> CustomerPartyType         | [edo:CustomerPartyType](#edo-CustomerPartyType)         | Получатель                                                                                                                  |
| <a name="IDCustomerType"></a> IDCustomerType            | [string](#string) (от 1 до 50)           | Уникальный идентификатор клиента в банке                                                                                    |
| <a name="DocumentType"></a> DocumentType              | [edo:DocumentType](#edo-DocumentType)              | Данные электронного документа                                                                                               |
| <a name="DocKindType"></a> DocKindType               | [string](#string) (2)                    | Вид электронного документа, как он задан в описании к стандарту                                                             |
| <a name="ContentType"></a> ContentType               | [string](#string)                        | Тип контента передаваемого файла. <br> Доступные значения: <br> • application/xml; <br> • application/octet-stream; <br>  • text/plain; <br> • text/xml |
| <a name="AccNumType"></a> AccNumType                | [string](#string) (20)                   | Номер счета (расчетного, корреспондентского).  <br>Макет: [0-9]{20}                                                             |
| <a name="DigestType"></a> DigestType                | [edo:DigestType](#edo-DigestType)                | Дайджест электронного документа                                                                                             |
| <a name="ResultStatusType"></a> ResultStatusType          | [edo:ResultStatusType](#edo-ResultStatusType) (Выбор)  | Состояние электронного документа                                                                                            |
| <a name="StatusType"></a> StatusType                | [edo:StatusType](#edo-StatusType)                | Успешный ответ                                                                                                              |
| <a name="DateString"></a> DateString                | [string](#string)                        | Дата строкой в формате ДД.ММ.ГГГГ                                                                                           |
| <a name="PayDocRuApp"></a> PayDocRuApp               | [edo:PayDocRuApp](#edo-PayDocRuApp)               | Данные платежного поручения                                                                                                 |
| <a name="PayRequestApp"></a> PayRequestApp             | [edo:PayRequestApp](#edo-PayRequestApp)             | Данные платежного требования                                                                                                |
| <a name="PaymentDataType"></a> PaymentDataType           | [edo:PaymentDataType](#edo-PaymentDataType)           | Данные платежного документа                                                                                                 |
| <a name="SumType"></a> SumType                   | [decimal](#decimal) (18,2)                | Сумма в документе                                                                                                           |
| <a name="CustomerDetailsType"></a> CustomerDetailsType       | [edo:CustomerDetailsType](#edo-CustomerDetailsType)       | Реквизиты налогоплательщика                                                                                                 |
| <a name="BankType"></a> BankType                  | [edo:BankType](#edo-BankType)                  | Реквизиты банка                                                                                                             |
| <a name="StatementKindType"></a> StatementKindType         | [string](#string) (1)                    | Тип выписки банка <br> Доступные значения: 0, 1, 2. <br> • 0 - Окончательная выписка <br> • 1 - Промежуточная выписка <br> • 2 - Текущий остаток на счете |


## <a name="2"></a> Таблица типов W3C
(<http://www.w3.org/2001/XMLSchema>)

| Наименование 								|
|-------------------------------------------|
|<a name="string"></a> string				|
|<a name="dateTime"></a> dateTime			|
|<a name="date"></a> date     				|
|<a name="boolean"></a> boolean  			|
|<a name="int"></a> int      				|
|<a name="integer"></a> integer  			|
|<a name="byte"></a> byte					|
|<a name="dateTime"></a> dateTime  			|
|<a name="decimal"></a> decimal   			|
|<a name="base64Binary"></a> base64Binary 	|
|<a name="hexBinary"></a> hexBinary  |

## <a name="3"></a> Таблицы общих комплексных типов edo

### <a name="edo-ResultBank"></a> Тип edo:ResultBank

| Параметр | Тип               | Кратность | Описание                                  |
|----------|-------------------|:---------:|-------------------------------------------|
| Success  | [SuccessResultType](#SuccessResultType) |    [1]    | Успешный ответ банка                      |
| Error    | [ErrorType](#ErrorType)         |    [1]    | Ответ банка в случае возникновения ошибки |

### <a name="edo-ErrorType"></a> Тип edo:ErrorType

| Параметр    | Тип             | Кратность | Описание                                                             |
|-------------|-----------------|:---------:|----------------------------------------------------------------------|
| Code        | [string](#string) (4)      |   [0-1]   | Код ошибки, как он задан в описании к стандарту (см. таблицу)        |
| Description | [string](#string) (до 255) |   [0-1]   | Описание ошибки, как оно задано в описании к стандарту (см. таблицу) |
| MoreInfo    | [string](#string)          |   [0-1]   | Подробное пояснение к ошибке для пользователя                        |

### <a name="edo-SuccessResultType"></a> Тип edo:SuccessResultType

| Параметр              | Тип                       | Кратность | Описание                                            |
|-----------------------|---------------------------|:---------:|-----------------------------------------------------|
| SendPacketResponse    | [SendPacketResponseType](#SendPacketResponseType)    |    [1]    | Отправка пакета в банк                              |
| GetPacketListResponse | [GetPacketListResponseType](#GetPacketListResponseType) |    [1]    | Список ID пакетов, готовых к передачи клиенту       |
| GetPacketResponse     | [GetPacketResponseType](#GetPacketResponseType)     |    [1]    | Пакет электронных документов для получения клиентом |
| LogonResponse         | [LogonResponseType](#LogonResponseType)         |    [1]    | Аутентификация по логину + ОТР (опционально)        |
| LogonCertResponse     | [LogonCertResponseType](#LogonCertResponseType)     |    [1]    | Аутентификация по сертификату                       |
| GetSettingsResponse   | [GetSettingsResponse](#GetSettingsResponse)       |    [1]    | Получение настроек обмена в автоматическом режиме   |

### <a name="edo-SendPacketResponseType"></a> Тип edo:SendPacketResponseType

| Параметр | Тип    | Кратность | Описание                                                               |
|----------|--------|:---------:|------------------------------------------------------------------------|
| ID       | [IDType](#IDType) |    [1]    | идентификатор пакета (GUID), который был ему назначен на стороне банка |

### <a name="edo-GetPacketListResponseType"></a> Тип edo:GetPacketListResponseType

| Параметр            | Тип      | Кратность | Описание                                                            |
|---------------------|----------|:---------:|---------------------------------------------------------------------|
| TimeStampLastPacket | [dateTime](#dateTime) |   [0-1]   | Метка времени, на которую вернули всю актуальную информацию         |
| PacketID            | [IDType](#IDType)   |   [0-n]   | Идентификатор пакета (GUID), по которому его можно получить клиенту |

### <a name="edo-LogonResponseType"></a> Тип edo:LogonResponseType

| Параметр  | Тип       | Кратность | Описание                                                                        |
|-----------|-----------|:---------:|---------------------------------------------------------------------------------|
| SID       | [IDType](#IDType)    |    [1]    | Идентификатор сессии                                                            |
| ExtraAuth | [ExtraAuth](#edo-LogonResponseType_ExtraAuth) |   [0-1]   | Дополнительная аутентификация. Указывается, если требуется доп. аутентификация) |

>###### <a name="edo-LogonResponseType_ExtraAuth"></a> ExtraAuth
| Параметр | Тип | Кратность | Описание                                                       |
|----------|-----|:---------:|----------------------------------------------------------------|
| OTP      | [OTP](#edo-LogonResponseType_ExtraAuth_OTP) |    [1]    | Параметры доп.аутентификации, которые будут направлены клиенту |

>>###### <a name="edo-LogonResponseType_ExtraAuth_OTP"></a> OTP
| Параметр  | Тип         | Кратность | Описание                                                 |
|-----------|-------------|:---------:|----------------------------------------------------------|
| phoneMask | [string](#string) (12) |   [0-1]   | Маска телефона или номер  клиента                        |
| code      | [string](#string) (10) |   [0-1]   | Короткий код сессии, который будет показан при вводе OTP |

### <a name="edo-LogonCertResponseType"></a> Тип edo:LogonCertResponseType

| Параметр     | Тип          | Кратность | Описание                           |
|--------------|--------------|:---------:|------------------------------------|
| EncryptedSID | [base64Binary](#base64Binary) |    [1]    | Зашифрованный Идентификатор сессии |

### <a name="edo-GetSettingsResponse"></a> Тип edo:GetSettingsResponse

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](#IDType)            |    [1]    | Идентификатор настроек          |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Data          | [Data](#edo-GetSettingsResponse_Data)              |    [1]    | Настройки обмена с банком       |

>###### <a name="edo-GetSettingsResponse_Data"></a> Data
| Параметр                   | Тип         | Кратность | Описание                                                             |
|----------------------------|-------------|:---------:|----------------------------------------------------------------------|
| Тип значения: [base64Binary](#base64Binary) |             |           |                                                                      |
| dockind                    | [DocKindType](#DocKindType) |    [1]    | Код вида электронного документа, как он задан в описании к стандарту |

### <a name="edo-Packet"></a> Тип edo:Packet

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](#IDType)            |    [1]    | Идентификатор пакета            |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [ParticipantType](#ParticipantType)   |    [1]    | Отправитель                     |
| Recipient     | [ParticipantType](#ParticipantType)   |    [1]    | Получатель                      |
| Document      | [DocumentType](#DocumentType)      |   [1-n]   | Электронный документ            |

### <a name="edo-ParticipantType"></a> Тип edo:ParticipantType

| Параметр | Тип               | Кратность | Описание             |
|----------|-------------------|:---------:|----------------------|
| Customer | [CustomerPartyType](#CustomerPartyType) |    [1]    | Идентификатор пакета |
| Bank     | [BankPartyType](#BankPartyType)     |    [1]    | Версия формата       |

### <a name="edo-BankPartyType"></a> Тип edo:BankPartyType

| Параметр | Тип             | Кратность | Описание       |
|----------|-----------------|:---------:|----------------|
| bic      | [string](#string) (9)      |    [1]    | БИК банка      |
| name     | [string](#string) (до 160) |   [0-1]   | Название банка |

### <a name="edo-CustomerPartyType"></a> Тип edo:CustomerPartyType

| Параметр | Тип                  | Кратность | Описание                                             |
|----------|----------------------|:---------:|------------------------------------------------------|
| id       | [IDCustomerType](#IDCustomerType)       |    [1]    | Идентификатор клиента, как он задан на стороне банка |
| name     | [string](#string) (до 160)      |   [0-1]   | Название банка                                       |
| inn      | [string](#string) (от 10 до 12) |   [0-1]   | ИНН клиента                                          |
| kpp      | [string](#string) (9)           |   [0-1]   | КПП клиента                                          |

### <a name="edo-DocumentType"></a> Тип edo:DocumentType

| Параметр       | Тип               | Кратность | Описание                                                             |
|----------------|-------------------|:---------:|----------------------------------------------------------------------|
| id             | [IDType](#IDType)            |    [1]    | Идентификатор электронного документа                                 |
| dockind        | [DocKindType](#DocKindType)       |    [1]    | Код вида электронного документа, как он задан в описании к стандарту |
| formatVersion  | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                       |
| testOnly       | [boolean](#boolean)           |   [0-1]   | Тестовый документ                                                    |
| compressed     | [boolean](#boolean)[boolean](#boolean)           |   [0-1]   | Документ сжат                                                        |
| encrypted      | [boolean](#boolean)[boolean](#boolean)           |   [0-1]   | Документ зашифрован                                                  |
| signResponse   | [boolean](#boolean)[boolean](#boolean)           |   [0-1]   | Требуется ответная подпись                                           |
| notifyRequired | [boolean](#boolean)           |   [0-1]   | Требуется извещение о получении                                      |
| extID          | [IDType](#IDType)            |   [0-1]   | ID исходного документа, если такой был                               |
| Data           | [Data](#edo-DocumentType_Data)              |    [1]    | Данные электронного документа                                        |
| Signature      | [Signature](#edo-DocumentType_Signature)         |   [0-n]   | Данные электронных подписей                                          |

>###### <a name="edo-DocumentType_Data"></a> Data
| Параметр                   | Тип         | Кратность | Описание                         |
|----------------------------|-------------|:---------:|----------------------------------|
| Тип значения: base64Binary |             |           |                                  |
| fileName                   | [string](#string)      |   [0-1]   | Имя файла                        |
| contentType                | [ContentType](#ContentType) |   [0-1]   | Тип контента передаваемого файла |

>###### <a name="edo-DocumentType_Signature"></a> Signature
| Параметр         | Тип          | Кратность | Описание                                                             |
|------------------|--------------|:---------:|----------------------------------------------------------------------|
| x509IssuerName   | [string](#string)       |    [1]    | Имя издателя сертификата открытого ключа ЭП <br> (значение атрибута "CN") |
| x509SerialNumber | [hexBinary](#hexBinary)    |    [1]    | Серийный номер сертификата открытого ключа ЭП                        |
| SignedData       | [base64Binary](#base64Binary) |    [1]    | Электронная подпись                                                  |

### <a name="edo-DigestType"></a> Тип edo:DigestType

| Параметр | Тип  | Кратность | Описание                  |
|----------|------|:---------:|---------------------------|
| Data     | [Data](#edo-DigestType_Data) |   [0-1]   | Данные дайджеста в base64 |

>###### <a name="edo-DigestType_Data"></a> Data
| Параметр                   | Тип               | Кратность | Описание                                |
|----------------------------|-------------------|:---------:|-----------------------------------------|
| Тип значения: [base64Binary](#base64Binary) |                   |           |                                         |
| algorithmVersion           | [FormatVersionType](#FormatVersionType) |   [0-1]   | Версия алгоритма формирования дайджеста |

### <a name="edo-ResultStatusType"></a> Тип edo:ResultStatusType

| Параметр | Тип        | Кратность | Описание                            |
|----------|------------|:---------:|-------------------------------------|
| Error    | [ErrorType](#ErrorType)  |   [0-1]   | Ответ в случае возникновения ошибки |
| Status   | [StatusType](#StatusType) |   [0-1]   | Успешный ответ                      |

### <a name="edo-StatusType"></a> Тип edo:StatusType

| Параметр | Тип            | Кратность | Описание                                                       |
|----------|----------------|:---------:|----------------------------------------------------------------|
| Code     | [string](#string) (2)     |    [1]    | Код статуса, как он задан в описании к стандарту (см. таблицу) |
| Name     | [string](#string) (до 25) |   [0-1]   | Наименование статуса на стороне банка                          |
| MoreInfo | [string](#string)         |   [0-1]   | Дополнительная информация к статусу                            |

### <a name="edo-PayDocRuApp"></a> Тип edo:PayDocRuApp

| Параметр                                                                                                        | Тип                   | Кратность | Описание                         |
|-----------------------------------------------------------------------------------------------------------------|-----------------------|:---------:|----------------------------------|
| Тип значения: [PaymentDataType](#PaymentDataType)                                                                                   |                       |           |                                  |
| BudgetPaymentInfo                                                                                               | [BudgetPaymentInfoType](#edo-PayDocRuApp_BudgetPaymentInfoType) |   [0-1]   | Реквизиты бюджетного документа. <br> См.правила заполнения платежных поручений, утвержденные приказом Минфина России от 12 ноября 2013 года № 107н. |

>###### <a name="edo-PayDocRuApp_BudgetPaymentInfoType"></a> BudgetPaymentInfoType
| Параметр     | Тип                 | Кратность | Описание                                                                                          |
|--------------|---------------------|:---------:|---------------------------------------------------------------------------------------------------|
| DrawerStatus | [string](#string) <br> (от 1 до 2)  |   [0-1]   | Статус составителя  (поле 101).                                                                   |
| CBC          | [string](#string)  <br> (20)         |   [0-1]   | Код бюджетной классификации (КБК) в соответствии с классификацией доходов бюджетов РФ (поле 104). |
| OKTMO        | [string](#string)  <br> (от 1 до 11) |   [0-1]   | Значение кода ОКТМО муниципального образования или 0 (ноль) (поле 105).                           |
| Reason       | [string](#string)  <br> (от 1 до 2)  |   [0-1]   | Основание налогового платежа или 0 (ноль) (поле 106).                                             |
| TaxPeriod    | [string](#string)  <br> (от 1 до 10) |   [0-1]   | Налоговый период или 0 (ноль) / код таможенного органа (поле 107).                                |
| DocNo        | [string](#string) <br>  (15)         |   [0-1]   | Номер налогового документа (поле 108).                                                            |
| DocDate      | [DateString](#DateString) (от 1 до 10)          |   [0-1]   | Дата налогового документа или 0 (ноль) (поле 109).                                                |
| PayType      | [string](#string) <br>(от 1 до 2)  |   [0-1]   | Тип платежа (поле 110).                                                                           |

### <a name="edo-PayRequestApp"></a> Тип edo:PayRequestApp

| Параметр                      | Тип        | Кратность | Описание                                                                                                     |
|-------------------------------|------------|:---------:|--------------------------------------------------------------------------------------------------------------|
| Тип значения: [PaymentDataType](#PaymentDataType) |            |           |                                                                                                              |
| PaymentCondition              | [string](#string) (1) |    [1]    | Условие оплаты (поле 35): <br> 1 - заранее данный акцепт плательщика; <br> 2 - требуется получение акцепта плательщика |
| AcceptTerm                    | [byte](#byte)       |   [0-1]   | Срок для акцепта (поле 36): количество дней.                                                                 |
| DocDispatchDate               | [DateString](DateString) |   [0-1]   | Дата отсылки (вручения) плательщику предусмотренных договором документов (поле 37).                          |


### <a name="edo-PaymentDataType"></a> Тип edo:PaymentDataType

| Параметр    | Тип                 | Кратность | Описание                                                                                                                |
|-------------|---------------------|:---------:|-------------------------------------------------------------------------------------------------------------------------|
| DocNo       | [string](#string)              |    [1]    | Номер документа (поле 3).                                                                                               |
| DocDate     | [date](#date)                |    [1]    | Дата составления (поле 4).                                                                                              |
| Sum         | [SumType](#SumType)             |    [1]    | Сумма документа (поле 7).                                                                                               |
| Payer       | [CustomerDetailsType](#CustomerDetailsType) |    [1]    | Плательщик <br> (поля 8, 9, 10, 11, 12, 60, 102).                                                                            |
| Payee       | [CustomerDetailsType](#CustomerDetailsType) |    [1]    | Получатель <br> (поля 13, 14, 15, 16, 17, 61, 103).                                                                          |
| PaymentKind | [string](#string) (15)         |   [0-1]   | Вид платежа (поле 5). <br> Указывается "срочно",  "телеграфом",  "почтой",   иное значение в порядке, установленном  банком. |
| TransitionKind | [string](#string)  (2)   | [0-1] | Вид операции (поле 18). <br> Указывается условное цифровое обозначение документа, согласно установленного ЦБР перечня условных обозначений (шифров) документов, проводимых по счетам в кредитных организациях. |
| Priority       | [string](#string)  (1)   | [0-1] | Очередность платежа (поле 21).                                                                                                                                                                            |
| Code           | [string](#string)  (25)  | [0-1] | Уникальный идентификатор платежа (поле 22). <br>  С 31 марта 2014 года согласно Указанию N 3025-У ЦБР.                                                                                                          |
| Purpose        | [string](#string)  (210) | [0-1] | Назначение платежа (поле 24).                                                                                                                                                                             |

### <a name="edo-CustomerDetailsType"></a> Тип edo:CustomerDetailsType

| Параметр | Тип                | Кратность | Описание                                                                                     |
|----------|--------------------|:---------:|----------------------------------------------------------------------------------------------|
| Name     | [string](#string)             |    [1]    | Наименование налогоплательщика                                                               |
| INN      | [string](#string)             |   [0-1]   | Идентификационный номера налогоплательщика (ИНН)                                             |
| KPP      | [string](#string) (от 1 до 9) |   [0-1]   | Для платежей в бюджет - указывать обязательно                                                |
| Account  | [AccNumType](#AccNumType)         |    [1]    | Расчетный счет клиента в его банке, независимо от того, прямые расчеты у этого банка или нет |
| Bank     | [BankType](#BankType)           |    [1]    | Реквизиты банка                                                                              |

### <a name="edo-BankType"></a> Тип edo:BankType

| Параметр   | Тип             | Кратность | Описание                       |
|------------|-----------------|:---------:|--------------------------------|
| BIC        | [string](#string) (9)      |    [1]    | БИК банка                      |
|            |                 |           | Макет: [0-9]{9}                |
| Name       | [string](#string) (до 160) |   [0-1]   | Название банка                 |
| City       | [string](#string) (до 30)  |   [0-1]   | Город (населенный пункт) банка |
| CorrespAcc | [AccNumType](#AccNumType)       |   [0-1]   | Коррсчет банка                 |
