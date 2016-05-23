# ВНИМАНИЕ! Это не финальная версия, раздел может быть дополнен.
 
#Описание типов


+ [Типы **W3C**](#1)
+ [Простые типы **edo**](#2)
+ [Общие **комплексные типы edo**](#3)
 + [Тип edo:BankPartyType](#edo-BankPartyType)
 + [Тип edo:BankType](#edo-BankType)
 + [Тип edo:CancelationRequest](#edo-CancelationRequest)
 + [Тип edo:CustomerDetailsType](#edo-CustomerDetailsType)
 + [Тип edo:CustomerPartyType](#edo-CustomerPartyType)
 + [Тип edo:DigestType](#edo-DigestType)
 + [Тип edo:DocumentType](#edo-DocumentType)
 + [Тип edo:ErrorType](#edo-ErrorType)
 + [Тип edo:GetPacketListResponseType](#edo-GetPacketListResponseType)
 + [Тип edo:GetSettingsResponse](#edo-GetSettingsResponse)
 + [Тип edo:LogonCertResponseType](#edo-LogonCertResponseType)
 + [Тип edo:LogonResponseType](#edo-LogonResponseType)
 + [Тип edo:Packet](#edo-Packet)
 + [Тип edo:ParticipantType](#edo-ParticipantType)
 + [Тип edo:PayDocRu](#edo-PayDocRu)
 + [Тип edo:PayDocRuApp](#edo-PayDocRuApp)
 + [Тип edo:PaymentDataType](#edo-PaymentDataType)
 + [Тип edo:PayRequest](#edo-PayRequest)
 + [Тип edo:PayRequestApp](#edo-PayRequestApp)
 + [Тип edo:Probe](#edo-Probe)
 + [Тип edo:ResultBank](#edo-ResultBank)
 + [Тип edo:ResultStatusType](#edo-ResultStatusType)
 + [Тип edo:SendPacketResponseType](#edo-SendPacketResponseType)
 + [Тип edo:Statement](#edo-Statement)
 + [Тип edo:StatementRequest](#edo-StatementRequest)
 + [Тип edo:StatusDocNotice](#edo-StatusDocNotice)
 + [Тип edo:StatusType](#edo-StatusType)
 + [Тип edo:StatusRequest](#edo-StatusRequest)
 + [Тип edo:SuccessResultType](#edo-SuccessResultType)


## <a name="1"></a> Типы W3C
(<http://www.w3.org/2001/XMLSchema>)

| Наименование 								|
|-------------------------------------------|
| <a name="base64Binary"></a> base64Binary  |
| <a name="boolean"></a> boolean           	|
| <a name="byte"></a> byte                 	|
| <a name="date"></a> date                 	|
| <a name="dateTime"></a> dateTime         	|
| <a name="dateTime"></a> dateTime         	|
| <a name="decimal"></a> decimal           	|
| <a name="hexBinary"></a> hexBinary       	|
| <a name="int"></a> int                   	|
| <a name="integer"></a> integer           	|
| <a name="string"></a> string             	|

## <a name="2"></a> Простые типы edo
(<http://directbank.1c.ru/XMLSchema>)

| Наименование                                                       | Тип                                                             | Описание                                                                                                                                                  |
|--------------------------------------------------------------------|-----------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <a name="AccNumType"></a> AccNumType                               | [string](#string) (20)                                          | Номер счета (расчетного, корреспондентского).  <br>Макет: [0-9]{20}                                                                                       |
| <a name="BankPartyType"></a> BankPartyType                         | [edo:BankPartyType](#edo-BankPartyType)                         | Отправитель                                                                                                                                               |
| <a name="BankType"></a> BankType                                   | [edo:BankType](#edo-BankType)                                   | Реквизиты банка                                                                                                                                           |
| <a name="ContentType"></a> ContentType                             | [string](#string)                                               | Тип контента передаваемого файла. <br> Доступные значения: <br> • application/xml; <br> • application/octet-stream; <br>  • text/plain; <br> • text/xml   |
| <a name="CustomerDetailsType"></a> CustomerDetailsType             | [edo:CustomerDetailsType](#edo-CustomerDetailsType)             | Реквизиты налогоплательщика                                                                                                                               |
| <a name="CustomerPartyType"></a> CustomerPartyType                 | [edo:CustomerPartyType](#edo-CustomerPartyType)                 | Получатель                                                                                                                                                |
| <a name="DateString"></a> DateString                               | [string](#string)                                               | Дата строкой в формате ДД.ММ.ГГГГ                                                                                                                         |
| <a name="DigestType"></a> DigestType                               | [edo:DigestType](#edo-DigestType)                               | Дайджест электронного документа                                                                                                                           |
| <a name="DocKindType"></a> DocKindType                             | [string](#string) (2)                                           | Вид электронного документа, как он задан в описании к стандарту                                                                                           |
| <a name="DocumentType"></a> DocumentType                           | [edo:DocumentType](#edo-DocumentType)                           | Данные электронного документа                                                                                                                             |
| <a name="ErrorType"></a> ErrorType                                 | [edo:ErrorType](#edo-ErrorType)                                 | Ответ в случае возникновения ошибки                                                                                                                       |
| <a name="FormatVersionType"></a> FormatVersionType                 | [string](#string) (до 12)                                       | Версия формата                                                                                                                                            |
| <a name="GetPacketListResponseType"></a> GetPacketListResponseType | [edo:GetPacketListResponseType](#edo-GetPacketListResponseType) | Список ID пакетов, готовых к передачи клиенту                                                                                                             |
| <a name="GetPacketResponse"></a> GetPacketResponse                 | [edo:Packet](#edo-Packet)                                       | Пакет электронных документов для получения клиентом                                                                                                       |
| <a name="GetSettingsResponseType"></a> GetSettingsResponseType     | [edo:GetSettingsResponseType](#edo-GetSettingsResponseType)     | Получение настроек обмена в автоматическом режиме                                                                                                         |
| <a name="IDCustomerType"></a> IDCustomerType                       | [string](#string) (от 1 до 50)                                  | Уникальный идентификатор клиента в банке                                                                                                                  |
| <a name="IDType"></a> IDType                                       | [string](#string)                                               | Уникальный идентификатор                                                                                                                                  |
| <a name="LogonCertResponseType"></a> LogonCertResponseType         | [edo:LogonCertResponseType](#edo-LogonCertResponseType)         | Аутентификация по сертификату                                                                                                                             |
| <a name="LogonResponseType"></a> LogonResponseType                 | [edo:LogonResponseType](#edo-LogonResponseType)                 | Аутентификация по логину + ОТР (опционально)                                                                                                              |
| <a name="Packet"></a> Packet                                       | [edo:Packet](#edo-Packet)                                       | Пакет электронных документов                                                                                                                              |
| <a name="ParticipantType"></a> ParticipantType                     | [ edo:ParticipantType](#edo-ParticipantType)                    | Одна из сторон, принимающая участие в обмене электронными документами (Участник)                                                                          |
| <a name="PayDocRuApp"></a> PayDocRuApp                             | [edo:PayDocRuApp](#edo-PayDocRuApp)                             | Данные платежного поручения                                                                                                                               |
| <a name="PaymentDataType"></a> PaymentDataType                     | [edo:PaymentDataType](#edo-PaymentDataType)                     | Данные платежного документа                                                                                                                               |
| <a name="PayRequestApp"></a> PayRequestApp                         | [edo:PayRequestApp](#edo-PayRequestApp)                         | Данные платежного требования                                                                                                                              |
| <a name="ResultBank"></a> ResultBank                               | [edo:ResultBank](#edo-ResultBank)                               | Ответ банка                                                                                                                                               |
| <a name="ResultStatusType"></a> ResultStatusType                   | [edo:ResultStatusType](#edo-ResultStatusType) (Выбор)           | Состояние электронного документа                                                                                                                          |
| <a name="SendPacketResponseType"></a> SendPacketResponseType       | [edo:SendPacketResponseType](#edo-SendPacketResponseType)       | Отправка пакета в банк                                                                                                                                    |
| <a name="StatementKindType"></a> StatementKindType                 | [string](#string) (1)                                           | Тип выписки банка <br> Доступные значения: 0, 1, 2. <br> • 0 - Окончательная выписка <br> • 1 - Промежуточная выписка <br> • 2 - Текущий остаток на счете |
| <a name="StatusType"></a> StatusType                               | [edo:StatusType](#edo-StatusType)                               | Успешный ответ                                                                                                                                            |
| <a name="SuccessResultType"></a> SuccessResultType                 | [edo:SuccessResultType](#edo-SuccessResultType)                 | Успешный ответ банка                                                                                                                                      |
| <a name="SumType"></a> SumType                                     | [decimal](#decimal) (18,2)                                      | Сумма в документе                                                                                                                                         |
| <a name="UserAgentType"></a> UserAgentType                         | [string](#string) (до 100)                                      | Версия ПО                                                                                                                                                 |



## <a name="3"></a> Общие комплексные типы edo

### <a name="edo-BankPartyType"></a> Тип edo:BankPartyType

| Параметр | Тип             | Кратность | Описание       |
|----------|-----------------|:---------:|----------------|
| bic      | [string](#string) (9)      |    [1]    | БИК банка      |
| name     | [string](#string) (до 160) |   [0-1]   | Название банка |


### <a name="edo-BankType"></a> Тип edo:BankType

| Параметр   | Тип             | Кратность | Описание                       |
|------------|-----------------|:---------:|--------------------------------|
| BIC        | [string](#string) (9) <br> фасет: [0-9]{9}     |    [1]    | БИК банка                      |
| Name       | [string](#string) (до 160) |   [0-1]   | Название банка                 |
| City       | [string](#string) (до 30)  |   [0-1]   | Город (населенный пункт) банка |
| CorrespAcc | [AccNumType](#AccNumType)       |   [0-1]   | Коррсчет банка                 |

### <a name="edo-CancelationRequest"></a> Тип edo:CancelationRequest

| Параметр      | Тип               | Кратность | Описание                                                        |
|---------------|-------------------|:---------:|-----------------------------------------------------------------|
| id            | [IDType](#IDType)            |    [1]    | Идентификатор запроса                                           |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                  |
| creationDate  | [dateTime](#dateTime)          |    [1]    | Дата и время формирования                                       |
| userAgent     | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы                                 |
| Sender        | [BankPartyType](#BankPartyType)     |    [1]    | Отправитель                                                     |
| Recipient     | [CustomerPartyType](#CustomerPartyType) |    [1]    | Получатель                                                      |
| ExtID         | [IDType](#IDType)            |    [1]    | ID исходного электронного документа, который требуется отозвать |
| Reason        | [string](#string)            |   [0-1]   | Причина, основание отзыва электронного документа                |

### <a name="edo-CustomerDetailsType"></a> Тип edo:CustomerDetailsType

| Параметр | Тип                | Кратность | Описание                                                                                     |
|----------|--------------------|:---------:|----------------------------------------------------------------------------------------------|
| Name     | [string](#string)             |    [1]    | Наименование налогоплательщика                                                               |
| INN      | [string](#string)             |   [0-1]   | Идентификационный номера налогоплательщика (ИНН)                                             |
| KPP      | [string](#string) (от 1 до 9) |   [0-1]   | Для платежей в бюджет - указывать обязательно                                                |
| Account  | [AccNumType](#AccNumType)         |    [1]    | Расчетный счет клиента в его банке, независимо от того, прямые расчеты у этого банка или нет |
| Bank     | [BankType](#BankType)           |    [1]    | Реквизиты банка                                                                              |

### <a name="edo-CustomerPartyType"></a> Тип edo:CustomerPartyType

| Параметр | Тип                  | Кратность | Описание                                             |
|----------|----------------------|:---------:|------------------------------------------------------|
| id       | [IDCustomerType](#IDCustomerType)       |    [1]    | Идентификатор клиента, как он задан на стороне банка |
| name     | [string](#string) (до 160)      |   [0-1]   | Название клиента                                       |
| inn      | [string](#string) (от 10 до 12) |   [0-1]   | ИНН клиента                                          |
| kpp      | [string](#string) (9)           |   [0-1]   | КПП клиента                                          |

### <a name="edo-DigestType"></a> Тип edo:DigestType

| Параметр | Тип  | Кратность | Описание                  |
|----------|------|:---------:|---------------------------|
| Data     | [Data](#edo-DigestType_Data) |   [0-1]   | Данные дайджеста в base64 |

>###### <a name="edo-DigestType_Data"></a> Data
> - Тип значения: [base64Binary](#base64Binary)
>
| Параметр                   | Тип               | Кратность | Описание                                |
|----------------------------|-------------------|:---------:|-----------------------------------------|
| algorithmVersion           | [FormatVersionType](#FormatVersionType) |   [0-1]   | Версия алгоритма формирования дайджеста |


### <a name="edo-DocumentType"></a> Тип edo:DocumentType

| Параметр       | Тип               | Кратность | Описание                                                             |
|----------------|-------------------|:---------:|----------------------------------------------------------------------|
| id             | [IDType](#IDType)            |    [1]    | Идентификатор электронного документа                                 |
| dockind        | [DocKindType](#DocKindType)       |    [1]    | Код вида электронного документа, как он задан в описании к стандарту |
| formatVersion  | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                       |
| testOnly       | [boolean](#boolean)           |   [0-1]   | Тестовый документ                                                    |
| compressed     | [boolean](#boolean)          |   [0-1]   | Документ сжат                                                        |
| encrypted      | [boolean](#boolean)           |   [0-1]   | Документ зашифрован                                                  |
| signResponse   | [boolean](#boolean)          |   [0-1]   | Требуется ответная подпись                                           |
| notifyRequired | [boolean](#boolean)           |   [0-1]   | Требуется извещение о получении                                      |
| extID          | [IDType](#IDType)            |   [0-1]   | ID исходного документа, если такой был                               |
| Data           | [Data](#edo-DocumentType_Data)              |    [1]    | Данные электронного документа                                        |
| Signature      | [Signature](#edo-DocumentType_Signature)         |   [0-n]   | Данные электронных подписей                                          |

>###### <a name="edo-DocumentType_Data"></a> Data
> - Тип значения: [base64Binary](#base64Binary)
>
| Параметр                   | Тип         | Кратность | Описание                         |
|----------------------------|-------------|:---------:|----------------------------------|
| fileName                   | [string](#string)      |   [0-1]   | Имя файла                        |
| contentType                | [ContentType](#ContentType) |   [0-1]   | Тип контента передаваемого файла |

>###### <a name="edo-DocumentType_Signature"></a> Signature
| Параметр         | Тип          | Кратность | Описание                                                             |
|------------------|--------------|:---------:|----------------------------------------------------------------------|
| x509IssuerName   | [string](#string)       |    [1]    | Имя издателя сертификата открытого ключа ЭП <br> (значение атрибута "CN") |
| x509SerialNumber | [hexBinary](#hexBinary)    |    [1]    | Серийный номер сертификата открытого ключа ЭП                        |
| SignedData       | [base64Binary](#base64Binary) |    [1]    | Электронная подпись                                                  |

### <a name="edo-ErrorType"></a> Тип edo:ErrorType

| Параметр    | Тип             | Кратность | Описание                                                             |
|-------------|-----------------|:---------:|----------------------------------------------------------------------|
| Code        | [string](#string) (4)      |   [0-1]   | Код ошибки, как он задан в описании к стандарту (см. таблицу)        |
| Description | [string](#string) (до 255) |   [0-1]   | Описание ошибки, как оно задано в описании к стандарту (см. таблицу) |
| MoreInfo    | [string](#string)          |   [0-1]   | Подробное пояснение к ошибке для пользователя                        |

### <a name="edo-GetPacketListResponseType"></a> Тип edo:GetPacketListResponseType

| Параметр            | Тип      | Кратность | Описание                                                            |
|---------------------|----------|:---------:|---------------------------------------------------------------------|
| TimeStampLastPacket | [dateTime](#dateTime) |   [0-1]   | Метка времени, на которую вернули всю актуальную информацию         |
| PacketID            | [IDType](#IDType)   |   [0-n]   | Идентификатор пакета (GUID), по которому его можно получить клиенту |

### <a name="edo-GetSettingsResponse"></a> Тип edo:GetSettingsResponse

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](#IDType)            |    [1]    | Идентификатор настроек          |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Data          | [Data](#edo-GetSettingsResponse_Data)              |    [1]    | Настройки обмена с банком       |

>###### <a name="edo-GetSettingsResponse_Data"></a> Data
> - Тип значения: [base64Binary](#base64Binary)
>
| Параметр                   | Тип         | Кратность | Описание                                                             |
|----------------------------|-------------|:---------:|----------------------------------------------------------------------|                                    |
| dockind                    | [DocKindType](#DocKindType) |    [1]    | Код вида электронного документа, как он задан в описании к стандарту |

### <a name="edo-LogonCertResponseType"></a> Тип edo:LogonCertResponseType

| Параметр     | Тип          | Кратность | Описание                           |
|--------------|--------------|:---------:|------------------------------------|
| EncryptedSID | [base64Binary](#base64Binary) |    [1]    | Зашифрованный Идентификатор сессии |

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

### <a name="edo-PayDocRu"></a> Тип edo:PayDocRu

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](#IDType)            |    [1]    | Идентификатор платежа           |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [BankPartyType](#BankPartyType)     |    [1]    | Отправитель                     |
| Recipient     | [CustomerPartyType](#CustomerPartyType) |    [1]    | Получатель                      |
| Data          | [PayDocRuApp](#PayDocRuApp)       |    [1]    | Данные платежного поручения     |
| Digest        | [DigestType](#DigestType)        |   [0-1]   | Дайджест электронного документа |


### <a name="edo-PayDocRuApp"></a> Тип edo:PayDocRuApp

- Тип значения: [PaymentDataType](#PaymentDataType)

| Параметр                                                                                                        | Тип                   | Кратность | Описание                         |
|-----------------------------------------------------------------------------------------------------------------|-----------------------|:---------:|----------------------------------|
| BudgetPaymentInfo                                                                                               | [BudgetPaymentInfoType](#edo-PayDocRuApp_BudgetPaymentInfoType) |   [0-1]   | Реквизиты бюджетного документа. <br> См.правила заполнения платежных поручений, утвержденные приказом Минфина России. |

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
| Purpose        | [string](#string)  (210) | [0-1] | Назначение платежа (поле 24).                                      

### <a name="edo-PayRequest"></a> Тип edo:PayRequest

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](#IDType)            |    [1]    | Идентификатор требования        |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [BankPartyType](#BankPartyType)     |    [1]    | Отправитель                     |
| Recipient     | [CustomerPartyType](#CustomerPartyType) |    [1]    | Получатель                      |
| Data          | [PayRequestApp](#PayRequestApp)     |    [1]    | Данные платежного требования    |
| Digest        | [DigestType](#DigestType)        |   [0-1]   | Дайджест электронного документа |

### <a name="edo-PayRequestApp"></a> Тип edo:PayRequestApp

- Тип значения: [PaymentDataType](#PaymentDataType)

| Параметр                      | Тип        | Кратность | Описание                                                                                                     |
|-------------------------------|------------|:---------:|--------------------------------------------------------------------------------------------------------------|
| PaymentCondition              | [string](#string) (1) |    [1]    | Условие оплаты (поле 35): <br> 1 - заранее данный акцепт плательщика; <br> 2 - требуется получение акцепта плательщика |
| AcceptTerm                    | [byte](#byte)       |   [0-1]   | Срок для акцепта (поле 36): количество дней.                                                                 |
| DocDispatchDate               | [DateString](DateString) |   [0-1]   | Дата отсылки (вручения) плательщику предусмотренных договором документов (поле 37).                          |

### <a name="edo-Probe"></a> Тип edo:Probe

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](#IDType)            |    [1]    | Идентификатор запроса           |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [BankPartyType](#BankPartyType)     |    [1]    | Отправитель                     |
| Recipient     | [CustomerPartyType](#CustomerPartyType) |    [1]    | Получатель                      |
| Digest        | [DigestType](#DigestType)        |   [0-1]   | Дайджест электронного документа |


### <a name="edo-ResultBank"></a> Тип edo:ResultBank

| Параметр | Тип               | Кратность | Описание                                  |
|----------|-------------------|:---------:|-------------------------------------------|
| Success  | [SuccessResultType](#SuccessResultType) |    [1]    | Успешный ответ банка                      |
| Error    | [ErrorType](#ErrorType)         |    [1]    | Ответ банка в случае возникновения ошибки |

### <a name="edo-ResultStatusType"></a> Тип edo:ResultStatusType

| Параметр | Тип        | Кратность | Описание                            |
|----------|------------|:---------:|-------------------------------------|
| Error    | [ErrorType](#ErrorType)  |   [0-1]   | Ответ в случае возникновения ошибки |
| Status   | [StatusType](#StatusType) |   [0-1]   | Успешный ответ                      |

### <a name="edo-SendPacketResponseType"></a> Тип edo:SendPacketResponseType

| Параметр | Тип    | Кратность | Описание                                                               |
|----------|--------|:---------:|------------------------------------------------------------------------|
| ID       | [IDType](#IDType) |    [1]    | идентификатор пакета (GUID), который был ему назначен на стороне банка |

### <a name="edo-Settings"></a> Тип edo:Settings

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](#IDType)            |    [1]    | Идентификатор набора данных     |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [BankPartyType](#BankPartyType)     |    [1]    | Отправитель                     |
| Recipient     | [CustomerPartyType](#CustomerPartyType) |    [1]    | Получатель                      |
| Data          | [Data](#edo-Settings_Data)              |   [1-n]   | Параметры обмена                |

>###### <a name="edo-Settings_Data"></a> Data:
| Параметр          | Тип               | Кратность | Описание                                                                    |
|-------------------|-------------------|:---------:|-----------------------------------------------------------------------------|
| CustomerID        | [IDCustomerType](#IDCustomerType)    |    [1]    | Уникальный идентификатор клиента в банке                                    |
| BankServerAddress | [string](#string) (от 1)     |    [1]    | Адрес ресурса банка                                                         |
| FormatVersion     | [FormatVersionType](#FormatVersionType) |    [1]    | Актуальная версия формата обмена данными                                    |
| Encoding          | [string](#string)            |    [1]    | Кодировка файлов обмена. По умолчанию: "UTF-8"                              |
| Compress          | [boolean](#boolean)           |   [0-1]   | Признак сжатия электронных документов при обмене. По умолчанию: false       |
| Logon             | [Logon](#edo-Settings_Data_Logon)             |    [1]    | Способ аутентификации на ресурсе банка                                      |
| CryptoParameters  | [CryptoParameters](#edo-Settings_Data_CryptoParameters)  |   [0-n]   | Настройки криптографии                                                      |
| Document          | [Document](#edo-Settings_Data_Document)          |   [1-n]   | Настройки по видам электронных документов, которыми возможен обмен с банком |


>>###### <a name="edo-Settings_Data_Logon"></a> Logon:
| Параметр    | Тип         | Кратность | Описание                           |
|-------------|-------------|:---------:|------------------------------------|
| Login       | [Login](#edo-Settings_Data_Logon_Login)       |    [1]    | По логину и паролю                 |
| Certificate | [Certificate](#edo-Settings_Data_Logon_Certificate) |    [1]    | По сертификату электронной подписи |


>>>###### <a name="edo-Settings_Data_Logon_Login"></a> Login:
| Параметр | Тип            | Кратность | Описание           |
|----------|----------------|:---------:|--------------------|
| User     | [string](#string) (до 50) |    [1]    | Логин пользователя |

>>>###### <a name="edo-Settings_Data_Logon_Certificate"></a> Certificate:
| Параметр            | Тип            | Кратность | Описание                                     |
|---------------------|----------------|:---------:|----------------------------------------------|
| EncryptingAlgorithm | [string](#string) (до 50) |    [1]    | Алгоритм шифрования, например, GOST 28147-89 |

>>###### <a name="edo-Settings_Data_CryptoParameters"></a> CryptoParameters:
| Параметр                   | Тип               | Кратность | Описание                                                                                         |
|----------------------------|-------------------|:---------:|--------------------------------------------------------------------------------------------------|
| CSPName                    | [string](#string) (до 256)   |    [1]    | Имя CSP (cryptographic service provider)                                                         |
| CSPType                    | [int](#int)               |    [1]    | Тип CSP (cryptographic service provider)                                                         |
| SignAlgorithm              | [string](#string) (до 50)    |    [1]    | Алгоритм подписи, например, GOST R 34.10-2001                                                    |
| HashAlgorithm              | [string](#string) (до 50)    |    [1]    | Алгоритм хэширования, например, GOST R 34.11-94                                                  |
| Encrypted                  | [Encrypted](#edo-Settings_Data_CryptoParameters_Encrypted)         |   [1-n]   | Применение шифрования данных на прикладном уровне                                                |
| BankTrustedRootCertificate | [base64Binary](#base64Binary)      |   [0-1]   | Доверенный корневой сертификат УЦ банка                                                          |
| BankCertificate            | [base64Binary](#base64Binary)      |   [0-1]   | Сертификат электронной подписи банка                                                             |
| CustomerSignature          | [CustomerSignature](#edo-Settings_Data_CryptoParameters_CustomerSignature) |   [1-n]   | Карточка электронных подписей клиента                                                            |
| URLAddinInfo               | [string](#string)            |   [0-1]   | Адрес-ссылка, откуда будет загружаться файл описания внешн.модуля, если он используется в обмене |

>>>###### <a name="edo-Settings_Data_CryptoParameters_Encrypted"></a> Encrypted:
| Параметр         | Тип            | Кратность | Описание                                     |
|------------------|----------------|:---------:|----------------------------------------------|
| EncryptAlgorithm | [string](#string) (до 50) |    [1]    | Алгоритм шифрования, например, GOST 28147-89 |

>>>###### <a name="edo-Settings_Data_CryptoParameters_CustomerSignature"></a> CustomerSignature:
| Параметр        | Тип             | Кратность | Описание                          |
|-----------------|-----------------|:---------:|-----------------------------------|
| numberGroup     | [integer](#integer)         |    [1]    | Номер группы электронных подписей |
| GroupSignatures | [GroupSignatures](#edo-Settings_Data_CryptoParameters_CustomerSignature_GroupSignatures) |   [1-n]   | Группа электронных подписей       |

>>>>###### <a name="edo-Settings_Data_CryptoParameters_CustomerSignature_GroupSignatures"></a> GroupSignatures:
| Параметр    | Тип          | Кратность | Описание                                     |
|-------------|--------------|:---------:|----------------------------------------------|
| Certificate | [base64Binary](#base64Binary) |    [1]    | Алгоритм шифрования, например, GOST 28147-89 |

>>###### <a name="edo-Settings_Data_Document"></a> Document:
| Параметр | Тип         | Кратность | Описание                                                               |
|----------|-------------|:---------:|------------------------------------------------------------------------|
| docKind  | [DocKindType](#DocKindType) |    [1]    | Код вида электронного документа, как он задан в описании к стандарту   |
| Signed   | [Signed](#edo-Settings_Data_Document_Signed)      |   [0-1]   | Применение электронной подписи для данного вида электронного документа |

>>>###### <a name="edo-Settings_Data_Document_Signed"></a> Signed:
| Параметр       | Тип    | Кратность | Описание                                                                               |
|----------------|--------|:---------:|----------------------------------------------------------------------------------------|
| RuleSignatures | [string](#string) |   [1-n]   | Правило, задающее наличие электронных подписей для данного вида электронного документа |


### <a name="edo-Statement"></a> Тип edo:Statement

| Параметр              | Тип               | Кратность | Описание                                        |
|-----------------------|-------------------|:---------:|-------------------------------------------------|
| id                    | [IDType](#IDType)            |    [1]    | Идентификатор выписки                           |
| formatVersion         | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                  |
| creationDate          | [dateTime](#dateTime)          |    [1]    | Дата и время формирования                       |
| userAgent             | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы                 |
| Sender                | [BankPartyType](#BankPartyType)     |    [1]    | Отправитель                                     |
| Recipient             | [CustomerPartyType](#CustomerPartyType) |    [1]    | Получатель                                      |
| Data                  | [Data](#edo-Statement_Data)              |    [1]    | Данные выписки по лиц. счету                    |
| ExtIDStatementRequest | [IDType](#IDType)            |   [0-1]   | ID исходного запроса на выписку, если такой был |

>###### <a name="edo-Statement_Data"></a> Data:
| Параметр       | Тип               | Кратность | Описание                                                 |
|----------------|-------------------|:---------:|----------------------------------------------------------|
| StatementType  | [StatementKindType](#StatementKindType) |    [1]    | Тип выписки                                              |
| DateFrom       | [dateTime](#dateTime)          |   [0-1]   | Начало периода выписки                                   |
| DateTo         | [dateTime](#dateTime)          |    [1]    | Конец периода выписки                                    |
| Account        | [AccNumType](#AccNumType)        |    [1]    | Номер лиц. счета                                         |
| Bank           | [BankType](#BankType)          |    [1]    | Банк, в котором открыт счет                              |
| OpeningBalance | [SumType](#SumType)           |   [0-1]   | Остаток на счете на начало периода                       |
| TotalDebits    | [SumType](#SumType)           |   [0-1]   | Общая сумма документов по дебету счета                   |
| TotalCredits   | [SumType](#SumType)           |   [0-1]   | Общая сумма документов по кредиту счета                  |
| ClosingBalance | [SumType](#SumType)           |    [1]    | Общая сумма документов по кредиту счета                  |
| OperationInfo  | [OperationInfo](#edo-Statement_Data_OperationInfo)     |   [0-n]   | Информация об одной операции по лицевому счету в выписке |
| Stamp          | [Stamp](#edo-Statement_Data_Stamp)             |   [0-1]   | Данные штампа банка по выписке в целом                   |

>>###### <a name="edo-Statement_Data_OperationInfo"></a> OperationInfo:
| Параметр | Тип                 | Кратность | Описание                                            |
|----------|---------------------|:---------:|-----------------------------------------------------|
| PayDoc   | [PayDoc](#edo-Statement_Data_OperationInfo_PayDoc)              |    [1]    | Данные платежного документа                         |
| DC       | [string](#string) (1)          |    [1]    | Признак дебета/кредита: <br>  1 - Операция по дебету, <br> 2 - Операция по кредиту                                |
| Date     | [date](#date)                |    [1]    | Дата проводки документа по лиц. счету               |
| ExtID    | [IDType](#IDType)              |   [0-1]   | ID исходного платежного документа плательщика       |
| Stamp    | [OperationInfo_Stamp](#edo-Statement_Data_OperationInfo_OperationInfo_Stamp) |    [1]    | Данные штампа банка по каждому платежному документу |

>>###### <a name="edo-Statement_Data_Stamp"></a> Stamp:
| Параметр               | Тип             | Кратность | Описание        |
|------------------------|-----------------|:---------:|-----------------|
| Тип значения: [BankType](#BankType) |                 |           |                 |
| Branch                 | [string](#string) (до 255) |    [1]    | Отделение банка |

-----

>>>###### <a name="edo-Statement_Data_OperationInfo_PayDoc"></a> PayDoc:
| Параметр               | Тип             | Кратность | Описание        |
|------------------------|-----------------|:---------:|-----------------|
| Тип значения: [BankType](#BankType) |                 |           |                 |
| Branch                 | [string](#string) (до 255) |    [1]    | Отделение банка |


>>>###### <a name="edo-Statement_Data_OperationInfo_OperationInfo_Stamp"></a> OperationInfo_Stamp:
| Параметр               | Тип             | Кратность | Описание                            |
|------------------------|-----------------|:---------:|-------------------------------------|
| Тип значения: [BankType](#BankType) |                 |           |                                     |
| Branch                 | [string](#string) (до 255) |    [1]    | Отделение банка                     |
| Status                 | [StatusType](#StatusType)      |    [1]    | Статус платежного документа в банке |


### <a name="edo-StatementRequest"></a> Тип edo:StatementRequest

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](#IDType)            |    [1]    | Идентификатор запроса           |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [BankPartyType](#BankPartyType)     |    [1]    | Отправитель                     |
| Recipient     | [CustomerPartyType](#CustomerPartyType) |    [1]    | Получатель                      |
| Data          | [Data](#edo-StatementRequest_Data)              |    [1]    | Данные запроса                  |
| Digest        | [DigestType](#DigestType)        |   [0-1]   | Дайджест электронного документа |

> ###### <a name="edo-StatementRequest_Data"></a> Data:
| Параметр      | Тип               | Кратность | Описание                                     |
|---------------|-------------------|:---------:|----------------------------------------------|
| StatementType | [StatementKindType](#StatementKindType) |    [1]    | Тип выписки                                  |
| DateFrom      | [dateTime](#dateTime)          |    [1]    | Начало периода формирования выписки          |
| DateTo        | [dateTime](#dateTime)          |    [1]    | Конец периода формирования выписки           |
| Account       | [AccNumType](#AccNumType)        |    [1]    | Номер счета, по которому производится запрос |
| Bank          | [BankType](#BankType)          |    [1]    | Банк, в котором открыт счет                  |


### <a name="edo-StatusDocNotice"></a> Тип edo:StatusDocNotice

| Параметр           | Тип               | Кратность | Описание                                                                |
|--------------------|-------------------|:---------:|-------------------------------------------------------------------------|
| id                 | [IDType](#IDType)            |    [1]    | Идентификатор извещения                                                 |
| formatVersion      | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                          |
| creationDate       | [dateTime](#dateTime)          |    [1]    | Дата и время формирования                                               |
| userAgent          | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы                                         |
| Sender             | [BankPartyType](#BankPartyType)     |    [1]    | Отправитель                                                             |
| Recipient          | [CustomerPartyType](#CustomerPartyType) |    [1]    | Получатель                                                              |
| ExtID              | [IDType](#IDType)            |    [1]    | ID исходного электронного документа, по которому возвращается состояния |
| Result             | [ResultStatusType](#ResultStatusType)  |    [1]    | Состояние электронного документа                                        |
| ExtIDStatusRequest | [IDType](#IDType)            |   [0-1]   | ID запроса о состоянии электронного документа, если был такой           |


### <a name="edo-StatusType"></a> Тип edo:StatusType

| Параметр | Тип            | Кратность | Описание                                                       |
|----------|----------------|:---------:|----------------------------------------------------------------|
| Code     | [string](#string) (2)     |    [1]    | Код статуса, как он задан в описании к стандарту (см. таблицу) |
| Name     | [string](#string) (до 25) |   [0-1]   | Наименование статуса на стороне банка                          |
| MoreInfo | [string](#string)         |   [0-1]   | Дополнительная информация к статусу                            |

### <a name="edo-StatusRequest"></a> Тип edo:StatusRequest

| Параметр      | Тип               | Кратность | Описание                                                                |
|---------------|-------------------|:---------:|-------------------------------------------------------------------------|
| id            | [IDType](#IDType)            |    [1]    | Идентификатор запроса                                                   |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                          |
| creationDate  | [dateTime](#dateTime)          |    [1]    | Дата и время формирования                                               |
| userAgent     | [UserAgentType](#UserAgentType)     |   [0-1]   | Наименование и версия программы                                         |
| Sender        | [BankPartyType](#BankPartyType)     |    [1]    | Отправитель                                                             |
| Recipient     | [CustomerPartyType](#CustomerPartyType) |    [1]    | Получатель                                                              |
| ExtID         | [IDType](#IDType)            |    [1]    | ID исходного электронного документа, статус которого требуется получить |


### <a name="edo-SuccessResultType"></a> Тип edo:SuccessResultType

| Параметр              | Тип                       | Кратность | Описание                                            |
|-----------------------|---------------------------|:---------:|-----------------------------------------------------|
| SendPacketResponse    | [SendPacketResponseType](#SendPacketResponseType)    |    [1]    | Отправка пакета в банк                              |
| GetPacketListResponse | [GetPacketListResponseType](#GetPacketListResponseType) |    [1]    | Список ID пакетов, готовых к передачи клиенту       |
| GetPacketResponse     | [GetPacketResponseType](#GetPacketResponseType)     |    [1]    | Пакет электронных документов для получения клиентом |
| LogonResponse         | [LogonResponseType](#LogonResponseType)         |    [1]    | Аутентификация по логину + ОТР (опционально)        |
| LogonCertResponse     | [LogonCertResponseType](#LogonCertResponseType)     |    [1]    | Аутентификация по сертификату                       |
| GetSettingsResponse   | [GetSettingsResponse](#GetSettingsResponse)       |    [1]    | Получение настроек обмена в автоматическом режиме   |
