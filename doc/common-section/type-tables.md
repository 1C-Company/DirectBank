# Описание типов


+ [Типы **W3C**](#typesW3C)
+ [Простые типы **edo**](#typesEDO)
+ [Комплексные типы **edo**](#complexTypes)
   + [Тип edo:BankOrderApp](#edo-BankOrderApp)
   + [Тип edo:BankPartyType](#edo-BankPartyType)
   + [Тип edo:BankType](#edo-BankType)
   + [Тип edo:CancelationRequest](#edo-CancelationRequest)
   + [Тип edo:CashContributionType](#edo-CashContributionType)
   + [Тип edo:CheckType](#edo-CheckType)
   + [Тип edo:CollectionOrderApp](#edo-CollectionOrderApp)
   + [Тип edo:CustomerDetailsType](#edo-CustomerDetailsType)
   + [Тип edo:CustomerPartyType](#edo-CustomerPartyType)
   + [Тип edo:DigestType](#edo-DigestType)
   + [Тип edo:DocumentType](#edo-DocumentType)
   + [Тип edo:ErrorType](#edo-ErrorType)
   + [Тип edo:GetPacketListResponseType](#edo-GetPacketListResponseType)
   + [Тип edo:GetSettingsResponseType](#edo-GetSettingsResponseType)
   + [Тип edo:Letter](#edo-Letter)
   + [Тип edo:LogonCertResponseType](#edo-LogonCertResponseType)
   + [Тип edo:LogonResponseType](#edo-LogonResponseType)
   + [Тип edo:MemOrderApp](#edo-MemOrderApp)
   + [Тип edo:OtherCustomerDetailsType](#edo-OtherCustomerDetailsType)
   + [Тип edo:OtherPaymentDataType](#edo-OtherPaymentDataType)
   + [Тип edo:Packet](#edo-Packet)
   + [Тип edo:ParticipantType](#edo-ParticipantType)
   + [Тип edo:PayDocRu](#edo-PayDocRu)
   + [Тип edo:PayDocRuApp](#edo-PayDocRuApp)
   + [Тип edo:PaymentDataType](#edo-PaymentDataType)
   + [Тип edo:PaymentOrderApp](#edo-PaymentOrderApp)
   + [Тип edo:PayRequest](#edo-PayRequest)
   + [Тип edo:PayRequestApp](#edo-PayRequestApp)
   + [Тип edo:Probe](#edo-Probe)
   + [Тип edo:ResultBank](#edo-ResultBank)
   + [Тип edo:ResultStatusType](#edo-ResultStatusType)
   + [Тип edo:Settings](#edo-Settings)
   + [Тип edo:SendPacketResponseType](#edo-SendPacketResponseType)
   + [Тип edo:SignatureType](#edo-SignatureType)
   + [Тип edo:Statement](#edo-Statement)
   + [Тип edo:StatementRequest](#edo-StatementRequest)
   + [Тип edo:StatusPacketNotice](#edo-StatusPacketNotice)
   + [Тип edo:StatusDocNotice](#edo-StatusDocNotice)
   + [Тип edo:StatusType](#edo-StatusType)
   + [Тип edo:StatusRequest](#edo-StatusRequest)
   + [Тип edo:SuccessResultType](#edo-SuccessResultType)


## <a name="typesW3C"></a> Типы W3C
(<http://www.w3.org/2001/XMLSchema>)

| Наименование 								|
|-------------------------------------------|
| <a name="base64Binary"></a> base64Binary  |
| <a name="boolean"></a> boolean           	|
| <a name="date"></a> date                 	|
| <a name="dateTime"></a> dateTime         	|
| <a name="decimal"></a> decimal           	|
| <a name="integer"></a> integer           	|
| <a name="hexBinary"></a> hexBinary       	|
| <a name="string"></a> string             	|

## <a name="typesEDO"></a> Простые типы edo

| Наименование                                       | Тип                            | Описание                                                                                                                                                  |
| -------------------------------------------------- | ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <a name="AccNumType"></a> AccNumType               | [string](#string) (от 1 до 20) | Номер счета (расчетного, корреспондентского).  <br>Макет: [0-9]{от 1 до 20}                                                                               |
| <a name="ContentType"></a> ContentType             | [string](#string)              | Тип контента передаваемого файла. <br> Доступные значения: <br> • application/xml; <br> • application/octet-stream; <br>  • text/plain; <br> • text/xml   |
| <a name="DateString"></a> DateString               | [string](#string)              | Дата строкой в формате ДД.ММ.ГГГГ                                                                                                                         |
| <a name="DocKindType"></a> DocKindType             | [string](#string) (2)          | Вид электронного документа, как он задан в описании к стандарту                                                                                           |
| <a name="FormatVersionType"></a> FormatVersionType | [string](#string) (до 12)      | Версия формата                                                                                                                                            |
| <a name="IDCustomerType"></a> IDCustomerType       | [string](#string) (от 1 до 50) | Уникальный идентификатор клиента в банке                                                                                                                  |
| <a name="IDType"></a> IDType                       | [string](#string)              | Уникальный идентификатор                                                                                                                                  |
| <a name="StatementKindType"></a> StatementKindType | [string](#string) (1)          | Тип выписки банка <br> Доступные значения: 0, 1, 2. <br> • 0 - Окончательная выписка <br> • 1 - Промежуточная выписка <br> • 2 - Текущий остаток на счете |
| <a name="SumType"></a> SumType                     | [decimal](#decimal) (18,2)     | Сумма в документе                                                                                                                                         |
| <a name="UserAgentType"></a> UserAgentType         | [string](#string) (до 100)     | Версия ПО                                                                                                                                                 |



## <a name="complexTypes"></a> Комплексные типы edo

### <a name="edo-BankPartyType"></a> Тип edo:BankPartyType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр | Тип                        | Кратность | Описание       |
| -------- | -------------------------- | :-------: | -------------- |
| bic      | [string](#string) (9)      |    [1]    | БИК банка      |
| name     | [string](#string) (до 160) |   [0-1]   | Название банка |


### <a name="edo-BankType"></a> Тип edo:BankType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр   | Тип                                        | Кратность | Описание                       |
| ---------- | ------------------------------------------ | :-------: | ------------------------------ |
| BIC        | [string](#string) (9) <br> фасет: [0-9]{9} |    [1]    | БИК банка                      |
| Name       | [string](#string) (до 160)                 |   [0-1]   | Название банка                 |
| City       | [string](#string) (до 30)                  |   [0-1]   | Город (населенный пункт) банка |
| CorrespAcc | [AccNumType](#edo-AccNumType)                  |   [0-1]   | Коррсчет банка                 |

### <a name="edo-CashContributionType"></a> Тип edo:CashContributionType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

- Базовый тип: [OtherPaymentDataType](#OtherPaymentDataType)

| Параметр | Тип                                        | Кратность | Описание                                                                                                                                            |
| -------- | ------------------------------------------ | :-------: | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Person   | [Person](#edo-CashContributionType_Person) |   [0-1]   | От кого                                                                                                                                             |
| Symbol   | [string](#string) (до 10)                  |   [0-1]   | Указываются цифрами символы, <br> предусмотренные отчетностью по форме 0409202                                                                      |
| Source   | [string](#string) (до 255)                 |   [0-1]   | Указываются источники поступления наличных денег <br> в соответствии с содержанием символов отчетности <br> по форме 0409202 и содержанием операции |

>###### <a name="edo-CashContributionType_Person"></a> Person
>
| Параметр         | Тип                        | Кратность | Описание                          |
| ---------------- | -------------------------- | :-------: | --------------------------------- |
| FullName         | [string](#string) (до 255) |   [0-1]   | ФИО вносителя                     |
| IdentityDocument | [string](#string) (до 255) |   [0-1]   | Документ, удостоверяющий личность |

### <a name="edo-CheckType"></a> Тип edo:CheckType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

- Базовый тип: [OtherPaymentDataType](#OtherPaymentDataType)

| Параметр     | Тип                             | Кратность | Описание                                                                                                                                            |
| ------------ | ------------------------------- | :-------: | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Person       | [Person](#edo-CheckType_Person) |   [0-1]   | Кому                                                                                                                                                |
| DataPrinting | [string](#string) (до 10)       |   [0-1]   | Данные бумажной формы чека                                                                                                                          |
| Details      | [string](#string) (до 255)      |   [0-n]   | Указываются источники поступления наличных денег <br> в соответствии с содержанием символов отчетности <br> по форме 0409202 и содержанием операции |

>###### <a name="edo-CheckType_Person"></a> Person
>
| Параметр         | Тип                        | Кратность | Описание                          |
| ---------------- | -------------------------- | :-------: | --------------------------------- |
| FullName         | [string](#string) (до 255) |   [0-1]   | ФИО получателя                    |
| IdentityDocument | [string](#string) (до 255) |   [0-1]   | Документ, удостоверяющий личность |

>###### <a name="edo-CheckType_DataPrinting"></a> DataPrinting
>
| Параметр    | Тип                        | Кратность | Описание   |
| ----------- | -------------------------- | :-------: | ---------- |
| CheckSeries | [string](#string) (до 255) |   [0-1]   | Серия чека |
| CheckNumber | [string](#string) (до 255) |   [0-1]   | Номер чека |

>###### <a name="edo-CheckType_Details"></a> Details
>
| Параметр | Тип                        | Кратность | Описание                                                                                                                                      |
| -------- | -------------------------- | :-------: | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Symbol   | [string](#string) (до 10)  |    [1]    | Указываются цифрами символы, предусмотренные отчетностью по форме 0409202                                                                     |
| Purpose  | [string](#string) (до 255) |   [0-1]   | Указываются направления (цели) выдачи наличных денег в соответствии с содержанием символов отчетности по форме 0409202 и содержанием операции |
| Sum      | [SumType](#SumType)        |    [1]    | Номер чека                                                                                                                                    |

### <a name="edo-BudgetPaymentInfoType"></a> Тип edo:BudgetPaymentInfoType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр     | Тип                                    | Кратность | Описание                                                                                          |
| ------------ | -------------------------------------- | :-------: | ------------------------------------------------------------------------------------------------- |
| DrawerStatus | [string](#string) <br> (от 1 до 2)     |   [0-1]   | Статус составителя  (поле 101).                                                                   |
| CBC          | [string](#string)  <br> (20)           |   [0-1]   | Код бюджетной классификации (КБК) в соответствии с классификацией доходов бюджетов РФ (поле 104). |
| OKTMO        | [string](#string)  <br> (от 1 до 11)   |   [0-1]   | Значение кода ОКТМО муниципального образования или 0 (ноль) (поле 105).                           |
| Reason       | [string](#string)  <br> (от 1 до 2)    |   [0-1]   | Основание налогового платежа или 0 (ноль) (поле 106).                                             |
| TaxPeriod    | [string](#string)  <br> (от 1 до 10)   |   [0-1]   | Налоговый период или 0 (ноль) / код таможенного органа (поле 107).                                |
| DocNo        | [string](#string) <br>  (15)           |   [0-1]   | Номер налогового документа (поле 108).                                                            |
| DocDate      | [DateString](#DateString) (от 1 до 10) |   [0-1]   | Дата налогового документа или 0 (ноль) (поле 109).                                                |
| PayType      | [string](#string) <br>(от 1 до 2)      |   [0-1]   | Код выплат (поле 110).                                                                            |

### <a name="edo-CancelationRequest"></a> Тип edo:CancelationRequest (*[1C-Bank_CancelationRequest.xsd](../xsd-scheme/readme.md#1C-Bank_CancelationRequest)*)

| Параметр      | Тип                                     | Кратность | Описание                                                        |
| ------------- | --------------------------------------- | :-------: | --------------------------------------------------------------- |
| id            | [IDType](#IDType)                       |    [1]    | Идентификатор запроса                                           |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                  |
| creationDate  | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования                                       |
| userAgent     | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы                                 |
| Sender        | [CustomerPartyType](#edo-CustomerPartyType) |    [1]    | Отправитель                                                     |
| Recipient     | [BankPartyType](#edo-BankPartyType)     |    [1]    | Получатель                                                      |
| ExtID         | [IDType](#IDType)                       |    [1]    | ID исходного электронного документа, который требуется отозвать |
| Reason        | [string](#string)                       |   [0-1]   | Причина, основание отзыва электронного документа                |
| Digest        | [DigestType](#edo-DigestType)               |   [0-1]   | Дайджест запроса                                                |

### <a name="edo-CollectionOrderApp"></a> Тип edo:CollectionOrderApp (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

- Базовый тип: [PaymentDataType](#PaymentDataType)

| Параметр          | Тип                                                 | Кратность | Описание                                                                                                         |
| ----------------- | --------------------------------------------------- | :-------: | ---------------------------------------------------------------------------------------------------------------- |
| BudgetPaymentInfo | [BudgetPaymentInfoType](#edo-BudgetPaymentInfoType) |   [0-1]   | Реквизиты бюджетного документа.  См.правила заполнения платежных поручений, утвержденные приказом Минфина России |


### <a name="edo-CustomerDetailsType"></a> Тип edo:CustomerDetailsType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр | Тип                           | Кратность | Описание                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| -------- | ----------------------------- | :-------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name     | [string](#string)             |    [1]    | Наименование налогоплательщика                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| INN      | [string](#string)             |   [0-1]   | Идентификационный номера налогоплательщика (ИНН)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| KPP      | [string](#string) (от 1 до 9) |   [0-1]   | Для платежей в бюджет - указывать обязательно                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Account  | [AccNumType](#edo-AccNumType)     |   [0-1]   | Расчетный счет клиента в его банке, независимо от того, прямые расчеты у этого банка или нет. Номер счета может не указываться в следующих случаях: в распоряжении, если получателем средств является кредитная организация, филиал кредитной организации, в том числе в целях выдачи наличных денежных средств получателю средств - физическому лицу без открытия банковского счета; в платежном поручении на общую сумму с реестром, в котором указаны получатели средств, обслуживаемые одним банком, составляемом плательщиком; в платежном поручении на общую сумму с реестром, в котором указаны плательщики, обслуживаемые одним банком, и получатели средств, обслуживаемые другим банком, составляемом банком плательщика |
| Bank     | [BankType](#edo-BankType)         |    [1]    | Реквизиты банка                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

### <a name="edo-CustomerPartyType"></a> Тип edo:CustomerPartyType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр | Тип                               | Кратность | Описание                                             |
| -------- | --------------------------------- | :-------: | ---------------------------------------------------- |
| id       | [IDCustomerType](#IDCustomerType) |    [1]    | Идентификатор клиента, как он задан на стороне банка |
| name     | [string](#string) (до 160)        |   [0-1]   | Название клиента                                     |
| inn      | [string](#string) (от 10 до 12)   |   [0-1]   | ИНН клиента                                          |
| kpp      | [string](#string) (9)             |   [0-1]   | КПП клиента                                          |

### <a name="edo-DigestType"></a> Тип edo:DigestType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр | Тип                          | Кратность | Описание                  |
| -------- | ---------------------------- | :-------: | ------------------------- |
| Data     | [Data](#edo-DigestType_Data) |    [1]    | Данные дайджеста в base64 |

>###### <a name="edo-DigestType_Data"></a> Data
> - Базовый тип: [base64Binary](#base64Binary)
>
| Параметр         | Тип                                     | Кратность | Описание                                |
| ---------------- | --------------------------------------- | :-------: | --------------------------------------- |
| algorithmVersion | [FormatVersionType](#FormatVersionType) |   [0-1]   | Версия алгоритма формирования дайджеста |


### <a name="edo-DocumentType"></a> Тип edo:DocumentType (*[1C-Bank_Packet.xsd](../xsd-scheme/readme.md#1C-Bank_Packet)*)

| Параметр       | Тип                                     | Кратность | Описание                                                                                                      |
| -------------- | --------------------------------------- | :-------: | ------------------------------------------------------------------------------------------------------------- |
| id             | [IDType](#IDType)                       |    [1]    | Идентификатор электронного документа. Совпадает с идентификатором электронного документа, помещенного в Data. |
| dockind        | [DocKindType](#DocKindType)             |    [1]    | Код вида электронного документа, как он задан в описании к стандарту                                          |
| formatVersion  | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                                                                |
| testOnly       | [boolean](#boolean)                     |   [0-1]   | Тестовый документ                                                                                             |
| compressed     | [boolean](#boolean)                     |   [0-1]   | Документ сжат                                                                                                 |
| encrypted      | [boolean](#boolean)                     |   [0-1]   | Документ зашифрован                                                                                           |
| signResponse   | [boolean](#boolean)                     |   [0-1]   | Требуется ответная подпись                                                                                    |
| notifyRequired | [boolean](#boolean)                     |   [0-1]   | Требуется извещение о получении                                                                               |
| extID          | [IDType](#IDType)                       |   [0-1]   | ID исходного документа, если такой был                                                                        |
| Data           | [Data](#edo-DocumentType_Data)          |    [1]    | Данные электронного документа                                                                                 |
| Signature      | [Signature](#edo-SignatureType)         |   [0-n]   | Данные электронных подписей                                                                                   |

>###### <a name="edo-DocumentType_Data"></a> Data
> - Базовый тип: [base64Binary](#base64Binary)
>
| Параметр    | Тип                         | Кратность | Описание                         |
| ----------- | --------------------------- | :-------: | -------------------------------- |
| fileName    | [string](#string)           |   [0-1]   | Имя файла                        |
| contentType | [ContentType](#ContentType) |   [0-1]   | Тип контента передаваемого файла |


### <a name="edo-ErrorType"></a> Тип edo:ErrorType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр    | Тип                        | Кратность | Описание                                                                                 |
| ----------- | -------------------------- | :-------: | ---------------------------------------------------------------------------------------- |
| Code        | [string](#string) (4)      |    [1]    | Код ошибки, как он задан в описании к стандарту (см. [таблицу](tables.md#errors))        |
| Description | [string](#string) (до 255) |    [1]    | Описание ошибки, как оно задано в описании к стандарту (см. [таблицу](tables.md#errors)) |
| MoreInfo    | [string](#string)          |   [0-1]   | Подробное пояснение к ошибке для пользователя                                            |

### <a name="edo-GetPacketListResponseType"></a> Тип edo:GetPacketListResponseType (*[1C-Bank_ResultBank.xsd](../xsd-scheme/readme.md#1C-Bank_ResultBank)*)

| Параметр            | Тип                   | Кратность | Описание                                                                                                              |
| ------------------- | --------------------- | :-------: | --------------------------------------------------------------------------------------------------------------------- |
| TimeStampLastPacket | [dateTime](#dateTime) |   [0-1]   | Метка времени, на которую вернули всю актуальную информацию. Значение не должно содержать информацию о часовом поясе. |
| PacketID            | [IDType](#IDType)     |   [0-n]   | Идентификатор транспортного контейнера (GUID), по которому его можно получить клиенту                                 |

### <a name="edo-GetSettingsResponseType"></a> Тип edo:GetSettingsResponseType (*[1C-Bank_ResultBank.xsd](../xsd-scheme/readme.md#1C-Bank_ResultBank)*)

| Параметр      | Тип                                       | Кратность | Описание                        |
| ------------- | ----------------------------------------- | :-------: | ------------------------------- |
| id            | [IDType](#IDType)                         |    [1]    | Идентификатор настроек          |
| formatVersion | [FormatVersionType](#FormatVersionType)   |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)                     |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)           |   [0-1]   | Наименование и версия программы |
| Data          | [Data](#edo-GetSettingsResponseType_Data) |    [1]    | Настройки обмена с банком       |

>#### <a name="edo-GetSettingsResponseType_Data"></a> Data
> - Базовый тип: [base64Binary](#base64Binary)
>
| Параметр | Тип                         | Кратность | Описание                                                             |
| -------- | --------------------------- | :-------: | -------------------------------------------------------------------- |
| dockind  | [DocKindType](#DocKindType) |    [1]    | Код вида электронного документа, как он задан в описании к стандарту |

### <a name="edo-LetterType"></a> Тип edo:LetterType (*[1C-Bank_Letter.xsd](../xsd-scheme/readme.md#1C-Bank_Letter)*)

| Параметр      | Тип                                     | Кратность | Описание                           |
| ------------- | --------------------------------------- | :-------: | ---------------------------------- |
| Id            | [IDType](#IDType)                       |    [1]    | Уникальный идентификатор документа |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                     |
| creationDate  | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования          |
| userAgent     | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы    |
| Sender        | [ParticipantType](#edo-ParticipantType)     |    [1]    | Отправитель письма                 |
| Recipient     | [ParticipantType](#edo-ParticipantType)     |    [1]    | Получатель письма                  |
| Data          | [Data](#edo-LetterType_Data)            |    [1]    | Данные письма                      |

>#### <a name="edo-LetterType_Data"></a> Data

| Параметр         | Тип                                           | Кратность | Описание                                                                                       |
| ---------------- | --------------------------------------------- | :-------: | ---------------------------------------------------------------------------------------------- |
| linkedID         | [IDType](#IDType)                             |   [0-1]   | Идентификатор письма, в ответ на которое сформировано текущее письмо                           |
| correspondenceID | [IDType](#IDType)                             |   [0-1]   | Идентификатор переписки. Данный идентификатор совпадает у всех писем в рамках одной переписки. |
| DocNum           | [string](#string)                             |    [1]    | Номер документа.                                                                               |
| DocDate          | [date](#date)                                 |    [1]    | Дата документа.                                                                                |
| LetterTypeCode   | [string](#string)                             |   [0-1]   | Код типа письма. Перечень кодов приходят в файле настроек из банка                             |
| Theme            | [string](#string)                             |   [0-1]   | Тема письма                                                                                    |
| Text             | [string](#string)                             |   [0-1]   | Текст письма                                                                                   |
| Attachment       | [Attachment](#edo-LetterType_Data_Attachment) |   [0-n]   | Присоединенные файлы                                                                           |
| LinkedDoc        | [LinkedDoc](#edo-LetterType_Data_LinkedDoc)   |   [0-1]   | Документ, который является предметом обсуждения текущей переписки                              |

>##### <a name="edo-LetterType_Data_Attachment"></a> Attachment

| Параметр   | Тип                                                      | Кратность | Описание                     |
| ---------- | -------------------------------------------------------- | :-------: | ---------------------------- |
| BinaryFile | [BinaryFile](#edo-LetterType_Data_Attachment_BinaryFile) |    [1]    | Данные присоединенного файла |
| Signature  | [SignatureType](#edo-SignatureType)                      |   [0-n]   | Данные электронной подписи   |

>###### <a name="edo-LetterType_Data_Attachment_BinaryFile"></a> BinaryFile
> - Базовый тип: [base64Binary](#base64Binary)

| Параметр     | Тип                   | Кратность | Описание                                 |
| ------------ | --------------------- | :-------: | ---------------------------------------- |
| id           | [IDType](#IDType)     |    [1]    | Уникальный идентификатор вложения        |
| name         | [string](#string)     |    [1]    | Имя файла с расширением                  |
| extension    | [string](#string)     |    [1]    | Расширение файла                         |
| size         | [integer](#integer)   |    [1]    | Размер файла в байтах                    |
| crc          | [integer](#integer)   |    [1]    | Контрольная сумма файла (алгоритм CRC32) |
| creationDate | [dateTime](#dateTime) |    [1]    | Дата создания файла                      |

>##### <a name="edo-LetterType_Data_LinkedDoc"></a> LinkedDoc

| Параметр | Тип                     | Кратность | Описание                           |
| -------- | ----------------------- | :-------: | ---------------------------------- |
| id       | [IDType](#IDType)       |    [1]    | Уникальный идентификатор документа |
| dockind  | [dockind](#DocKindType) |    [1]    | Код вида электронного документа    |

### <a name="edo-LogonCertResponseType"></a> Тип edo:LogonCertResponseType (*[1C-Bank_ResultBank.xsd](../xsd-scheme/readme.md#1C-Bank_ResultBank)*)

| Параметр     | Тип                           | Кратность | Описание                           |
| ------------ | ----------------------------- | :-------: | ---------------------------------- |
| EncryptedSID | [base64Binary](#base64Binary) |    [1]    | Зашифрованный Идентификатор сессии |

### <a name="edo-LogonResponseType"></a> Тип edo:LogonResponseType (*[1C-Bank_ResultBank.xsd](../xsd-scheme/readme.md#1C-Bank_ResultBank)*)

| Параметр  | Тип                                           | Кратность | Описание                                                                        |
| --------- | --------------------------------------------- | :-------: | ------------------------------------------------------------------------------- |
| SID       | [IDType](#IDType)                             |    [1]    | Идентификатор сессии (не должен содержать национальных символов)                |
| ExtraAuth | [ExtraAuth](#edo-LogonResponseType_ExtraAuth) |   [0-1]   | Дополнительная аутентификация. Указывается, если требуется доп. аутентификация) |

>###### <a name="edo-LogonResponseType_ExtraAuth"></a> ExtraAuth
| Параметр | Тип                                         |   Кратность    | Описание                                                       |
| -------- | ------------------------------------------- | :------------: | -------------------------------------------------------------- |
| OTP      | [OTP](#edo-LogonResponseType_ExtraAuth_OTP) | выбор <br> [1] | Параметры доп.аутентификации, которые будут направлены клиенту |

>>###### <a name="edo-LogonResponseType_ExtraAuth_OTP"></a> OTP
| Параметр  | Тип                    | Кратность | Описание                                                 |
| --------- | ---------------------- | :-------: | -------------------------------------------------------- |
| phoneMask | [string](#string) (12) |   [0-1]   | Маска телефона или номер  клиента                        |
| code      | [string](#string) (10) |   [0-1]   | Короткий код сессии, который будет показан при вводе OTP |

### <a name="edo-BankOrderApp"></a> Тип edo:BankOrderApp (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр              | Тип                   | Кратность | Описание                                |
| --------------------- | --------------------- | :-------: | --------------------------------------- |
| DocNo                 | [string](#string)     |    [1]    | Номер документа (поле 3)                |
| DocDate               | [date](#date)         |    [1]    | Дата составления (поле 4)               |
| Sum                   | [SumType](#SumType)   |    [1]    | Сумма документа (поле 9)                |
| Payer          | [CustomerDetailsType](#edo-CustomerDetailsType) |    [1]    | Плательщик <br> (поля 8, 9, 10, 11, 12, 60, 102).                                                                                                                                                              |
| Payee          | [CustomerDetailsType](#edo-CustomerDetailsType) |    [1]    | Получатель <br> (поля 13, 14, 15, 16, 17, 61, 103).                                                                                                                                                            |
| PaymentKind    | [string](#string) (15)                      |   [0-1]   | Вид платежа (поле 5). <br> Указывается "срочно",  "телеграфом",  "почтой",   иное значение в порядке, установленном  банком.                                                                                   |
| TransitionKind | [string](#string)  (2)                      |   [0-1]   | Вид операции (поле 18). <br> Указывается условное цифровое обозначение документа, согласно установленного ЦБР перечня условных обозначений (шифров) документов, проводимых по счетам в кредитных организациях. |
| Priority       | [string](#string)  (1)                      |   [0-1]   | Очередность платежа (поле 21).                                                                                                                                                                                 |
| Code           | [string](#string)  (25)                     |   [0-1]   | Уникальный идентификатор платежа (поле 22). <br>  С 31 марта 2014 года согласно Указанию N 3025-У ЦБР.                                                                                                         |
| Purpose        | [string](#string)  (210)                    |    [1]    | Назначение платежа (поле 24).                                                                                                                                                                                  |
### <a name="edo-MemOrderApp"></a> Тип edo:MemOrderApp (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр              | Тип                   | Кратность | Описание                                |
| --------------------- | --------------------- | :-------: | --------------------------------------- |
| DocNo                 | [string](#string)     |    [1]    | Номер документа (поле 3)                |
| DocDate               | [date](#date)         |    [1]    | Дата составления (поле 4)               |
| SpareField5           | [string](#string)     |   [0-1]   | Свободное поле (поле 5)                 |
| Author                | [BankType](#BankType) |   [0-1]   | Составитель (поле 6)                    |
| AccountNameDebit      | [string](#string)     |   [0-1]   | Наименование счета по дебету (поле 7)   |
| AccountDebit          | [string](#string)     |   [0-1]   | Счет по дебету (поле 8)                 |
| Sum                   | [SumType](#SumType)   |    [1]    | Сумма документа (поле 9)                |
| SpareField9a          | [string](#string)     |   [0-1]   | Свободное поле (поле 9a)                |
| AccountNameCredit     | [string](#string)     |   [0-1]   | Наименование счета по кредиту (поле 10) |
| AccountCredit         | [string](#string)     |   [0-1]   | Счет по кредиту (поле 11)               |
| PartialTransitionKind | [string](#string) (2) |   [0-1]   | Шифр документа (поле 13)                |
| SpareField14          | [string](#string)     |   [0-1]   | Свободное поле (поле 14)                |
| SpareField15          | [string](#string)     |   [0-1]   | Свободное поле (поле 15)                |
| TransitionContent     | [string](#string)     |   [0-1]   | Содержание операции (поле 16)           |
| SpareField20          | [string](#string)     |   [0-1]   | Свободное поле (поле 20)                |

### <a name="edo-OtherCustomerDetailsType"></a> Тип edo:OtherCustomerDetailsType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр | Тип                           | Кратность | Описание                                                                                     |
| -------- | ----------------------------- | :-------: | -------------------------------------------------------------------------------------------- |
| Name     | [string](#string)             |    [1]    | Наименование плательщика                                                                     |
| INN      | [string](#string)             |   [0-1]   | Идентификационный номера плательщика (ИНН)                                                   |
| KPP      | [string](#string) (от 1 до 9) |   [0-1]   | Для платежей в бюджет - указывать обязательно                                                |
| Account  | [AccNumType](#AccNumType)     |   [0-1]   | Расчетный счет клиента в его банке, независимо от того, прямые расчеты у этого банка или нет |
| Bank     | [BankType](#edo-BankType)         |   [0-1]   | Реквизиты банка                                                                              |

### <a name="edo-OtherPaymentDataType"></a> Тип edo:OtherPaymentDataType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр       | Тип                                                   | Кратность | Описание                         |
| -------------- | ----------------------------------------------------- | :-------: | -------------------------------- |
| DocNo          | [string](#string)                                     |   [0-1]   | Номер документа                  |
| DocDate        | [date](#date)                                         |   [0-1]   | Дата составления                 |
| Sum            | [SumType](#SumType)                                   |    [1]    | Сумма документа                  |
| Payer          | [OtherCustomerDetailsType](#edo-OtherCustomerDetailsType) |   [0-1]   | Плательщик                       |
| Payee          | [OtherCustomerDetailsType](#edo-OtherCustomerDetailsType) |   [0-1]   | Получатель                       |
| TransitionKind | [string](#string)  (2)                                |   [0-1]   | Вид операции                     |
| Code           | [string](#string)  (до 25)                            |   [0-1]   | Уникальный идентификатор платежа |
| Purpose        | [string](#string)                                     |   [0-1]   | Назначение                       |

### <a name="edo-Packet"></a> Тип edo:Packet (*[1C-Bank_Packet.xsd](../xsd-scheme/readme.md#1C-Bank_Packet)*)

| Параметр      | Тип                                     | Кратность | Описание                               |
| ------------- | --------------------------------------- | :-------: | -------------------------------------- |
| id            | [IDType](#IDType)                       |    [1]    | Идентификатор транспортного контейнера |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                         |
| creationDate  | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования              |
| userAgent     | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы        |
| Sender        | [ParticipantType](#edo-ParticipantType)     |    [1]    | Отправитель                            |
| Recipient     | [ParticipantType](#edo-ParticipantType)     |    [1]    | Получатель                             |
| Document      | [DocumentType](#edo-DocumentType)           |   [1-n]   | Электронный документ                   |

### <a name="edo-ParticipantType"></a> Тип edo:ParticipantType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр | Тип                                     |   Кратность    | Описание                               |
| -------- | --------------------------------------- | :------------: | -------------------------------------- |
| Customer | [CustomerPartyType](#edo-CustomerPartyType) | выбор <br> [1] | Идентификатор транспортного контейнера |
| Bank     | [BankPartyType](#edo-BankPartyType)         | выбор <br> [1] | Версия формата                         |

### <a name="edo-PayDocRu"></a> Тип edo:PayDocRu (*[1C-Bank_PayDocRu.xsd](../xsd-scheme/readme.md#1C-Bank_PayDocRu)*)

| Параметр      | Тип                                     | Кратность | Описание                        |
| ------------- | --------------------------------------- | :-------: | ------------------------------- |
| id            | [IDType](#IDType)                       |    [1]    | Идентификатор платежа           |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы |
| Sender        | [CustomerPartyType](#edo-CustomerPartyType) |    [1]    | Отправитель                     |
| Recipient     | [BankPartyType](#edo-BankPartyType)         |    [1]    | Получатель                      |
| Data          | [PayDocRuApp](#edo-PayDocRuApp)             |    [1]    | Данные платежного поручения     |
| Digest        | [DigestType](#edo-DigestType)               |   [0-1]   | Дайджест электронного документа |


### <a name="edo-PayDocRuApp"></a> Тип edo:PayDocRuApp (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

- Базовый тип: [PaymentDataType](#PaymentDataType)

| Параметр          | Тип                                                 | Кратность | Описание                                                                                                              |
| ----------------- | --------------------------------------------------- | :-------: | --------------------------------------------------------------------------------------------------------------------- |
| BudgetPaymentInfo | [BudgetPaymentInfoType](#edo-BudgetPaymentInfoType) |   [0-1]   | Реквизиты бюджетного документа. <br> См.правила заполнения платежных поручений, утвержденные приказом Минфина России. |

### <a name="edo-PaymentDataType"></a> Тип edo:PaymentDataType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр       | Тип                                         | Кратность | Описание                                                                                                                                                                                                       |
| -------------- | ------------------------------------------- | :-------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| DocNo          | [string](#string)                           |    [1]    | Номер документа (поле 3).                                                                                                                                                                                      |
| DocDate        | [date](#date)                               |    [1]    | Дата составления (поле 4).                                                                                                                                                                                     |
| Sum            | [SumType](#SumType)                         |    [1]    | Сумма документа (поле 7).                                                                                                                                                                                      |
| Payer          | [CustomerDetailsType](#edo-CustomerDetailsType) |    [1]    | Плательщик <br> (поля 8, 9, 10, 11, 12, 60, 102).                                                                                                                                                              |
| Payee          | [CustomerDetailsType](#edo-CustomerDetailsType) |    [1]    | Получатель <br> (поля 13, 14, 15, 16, 17, 61, 103).                                                                                                                                                            |
| PaymentKind    | [string](#string) (15)                      |   [0-1]   | Вид платежа (поле 5). <br> Указывается "срочно",  "телеграфом",  "почтой",   иное значение в порядке, установленном  банком.                                                                                   |
| TransitionKind | [string](#string)  (2)                      |   [0-1]   | Вид операции (поле 18). <br> Указывается условное цифровое обозначение документа, согласно установленного ЦБР перечня условных обозначений (шифров) документов, проводимых по счетам в кредитных организациях. |
| Priority       | [string](#string)  (1)                      |   [0-1]   | Очередность платежа (поле 21).                                                                                                                                                                                 |
| Code           | [string](#string)  (25)                     |   [0-1]   | Уникальный идентификатор платежа (поле 22). <br>  С 31 марта 2014 года согласно Указанию N 3025-У ЦБР.                                                                                                         |
| IncomeTypeCode | [string](#string)  (1)                      |   [0-1]   | Код вида дохода (поле 20). <br>  Согласно Указанию N 5286-У ЦБРФ.                                                                                                                                              |
| Purpose        | [string](#string)  (210)                    |    [1]    | Назначение платежа (поле 24).                                                                                                                                                                                  |

### <a name="edo-PaymentOrderApp"></a> Тип edo:PaymentOrderApp (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

- Базовый тип: [PaymentDataType](#PaymentDataType)

| Параметр              | Тип                                             | Кратность | Описание                                                                                                         |
| --------------------- | ----------------------------------------------- | :-------: | ---------------------------------------------------------------------------------------------------------------- |
| TransitionContent     | [string](#string) (до 16)                       |   [0-1]   | Содержание операции (поле 70)                                                                                    |
| PartialPaymentNo      | [string](#string) (до 3)                        |   [0-1]   | Номер частичного платежа (поле 38)                                                                               |
| PartialTransitionKind | [string](#string) (2)                           |   [0-1]   | Шифр платежного документа (поле 39)                                                                              |
| SumResidualPayment    | [SumType](#SumType)                             |   [0-1]   | Сумма остатка платежа (поле 42)                                                                                  |
| PartialDocNo          | [string](#string) (до 6)                        |   [0-1]   | Номер платежного документа (поле 40)                                                                             |
| PartialDocDate        | [DateString](#DateString)                       |   [0-1]   | Дата платежного документа (поле 41)                                                                              |
| BudgetPaymentInfo     | [BudgetPaymentInfoType](#edo-BudgetPaymentInfoType) |   [0-1]   | Реквизиты бюджетного документа.  См.правила заполнения платежных поручений, утвержденные приказом Минфина России |


### <a name="edo-PayRequest"></a> Тип edo:PayRequest (*[1C-Bank_PayRequest.xsd](../xsd-scheme/readme.md#1C-Bank_PayRequest)*)

| Параметр      | Тип                                     | Кратность | Описание                        |
| ------------- | --------------------------------------- | :-------: | ------------------------------- |
| id            | [IDType](#IDType)                       |    [1]    | Идентификатор требования        |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы |
| Sender        | [CustomerPartyType](#edo-CustomerPartyType) |    [1]    | Отправитель                     |
| Recipient     | [BankPartyType](#edo-BankPartyType)         |    [1]    | Получатель                      |
| Data          | [PayRequestApp](#edo-PayRequestApp)         |    [1]    | Данные платежного требования    |
| Digest        | [DigestType](#edo-DigestType)               |   [0-1]   | Дайджест электронного документа |

### <a name="edo-PayRequestApp"></a> Тип edo:PayRequestApp (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

- Базовый тип: [PaymentDataType](#PaymentDataType)

| Параметр         | Тип                       | Кратность | Описание                                                                                                               |
| ---------------- | ------------------------- | :-------: | ---------------------------------------------------------------------------------------------------------------------- |
| PaymentCondition | [string](#string) (1)     |    [1]    | Условие оплаты (поле 35): <br> 1 - заранее данный акцепт плательщика; <br> 2 - требуется получение акцепта плательщика |
| AcceptTerm       | [byte](#byte)             |   [0-1]   | Срок для акцепта (поле 36): количество дней.                                                                           |
| DocDispatchDate  | [DateString](#DateString) |   [0-1]   | Дата отсылки (вручения) плательщику предусмотренных договором документов (поле 37).                                    |

### <a name="edo-Probe"></a> Тип edo:Probe (*[1C-Bank_Probe.xsd](../xsd-scheme/readme.md#1C-Bank_Probe)*)

| Параметр      | Тип                                     | Кратность | Описание                        |
| ------------- | --------------------------------------- | :-------: | ------------------------------- |
| id            | [IDType](#IDType)                       |    [1]    | Идентификатор запроса           |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы |
| Sender        | [CustomerPartyType](#edo-CustomerPartyType) |    [1]    | Отправитель                     |
| Recipient     | [BankPartyType](#edo-BankPartyType)         |    [1]    | Получатель                      |
| Digest        | [DigestType](#edo-DigestType)               |   [0-1]   | Дайджест электронного документа |


### <a name="edo-ResultBank"></a> Тип edo:ResultBank (*[1C-Bank_ResultBank.xsd](../xsd-scheme/readme.md#1C-Bank_ResultBank)*)

| Параметр | Тип                                         |   Кратность    | Описание                                  |
| -------- | ------------------------------------------- | :------------: | ----------------------------------------- |
| Success  | [SuccessResultType](#edo-SuccessResultType) | выбор <br> [1] | Успешный ответ банка                      |
| Error    | [ErrorType](#edo-ErrorType)                     | выбор <br> [1] | Ответ банка в случае возникновения ошибки |

### <a name="edo-ResultStatusType"></a> Тип edo:ResultStatusType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр | Тип                       | Кратность | Описание                            |
| -------- | ------------------------- | :-------: | ----------------------------------- |
| Error    | [ErrorType](#edo-ErrorType)   |   [0-1]   | Ответ в случае возникновения ошибки |
| Status   | [StatusType](#edo-StatusType) |   [0-1]   | Успешный ответ                      |

### <a name="edo-SendPacketResponseType"></a> Тип edo:SendPacketResponseType (*[1C-Bank_ResultBank.xsd](../xsd-scheme/readme.md#1C-Bank_ResultBank)*)

| Параметр | Тип               | Кратность | Описание                                                                                 |
| -------- | ----------------- | :-------: | ---------------------------------------------------------------------------------------- |
| ID       | [IDType](#IDType) |    [1]    | идентификатор транспортного контейнера (GUID), который был ему назначен на стороне банка |

### <a name="edo-Settings"></a> Тип edo:Settings (*[1C-Bank_Settings.xsd](../xsd-scheme/readme.md#1C-Bank_Settings)*)

| Параметр      | Тип                                     | Кратность | Описание                                                               |
| ------------- | --------------------------------------- | :-------: | ---------------------------------------------------------------------- |
| id            | [IDType](#IDType)                       |    [1]    | Идентификатор набора данных                                            |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                         |
| creationDate  | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования                                              |
| userAgent     | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы                                        |
| Sender        | [BankPartyType](#edo-BankPartyType)         |    [1]    | Отправитель                                                            |
| Recipient     | [CustomerPartyType](#edo-CustomerPartyType) |    [1]    | Получатель. Атрибуты inn и kpp (при наличии)  обязательны к заполнению |
| Data          | [Data](#edo-Settings_Data)              |    [1]    | Параметры обмена                                                       |

>#### <a name="edo-Settings_Data"></a> Data:
| Параметр          | Тип                                                     | Кратность | Описание                                                                    |
| ----------------- | ------------------------------------------------------- | :-------: | --------------------------------------------------------------------------- |
| CustomerID        | [IDCustomerType](#IDCustomerType)                       |    [1]    | Уникальный идентификатор клиента в банке                                    |
| BankServerAddress | [string](#string) (от 1)                                |    [1]    | Адрес ресурса банка                                                         |
| FormatVersion     | [FormatVersionType](#FormatVersionType)                 |    [1]    | Актуальная версия формата обмена данными                                    |
| Encoding          | [string](#string)                                       |    [1]    | Кодировка файлов обмена. По умолчанию: "UTF-8"                              |
| Compress          | [boolean](#boolean)                                     |   [0-1]   | Признак сжатия электронных документов при обмене. По умолчанию: false       |
| Logon             | [Logon](#edo-Settings_Data_Logon)                       |    [1]    | Способ аутентификации на ресурсе банка                                      |
| CryptoParameters  | [CryptoParameters](#edo-Settings_Data_CryptoParameters) |   [0-1]   | Настройки криптографии                                                      |
| Document          | [Document](#edo-Settings_Data_Document)                 |   [1-n]   | Настройки по видам электронных документов, которыми возможен обмен с банком |
| ReceiptStatement  | [ReceiptStatement](#edo-Settings_Data_ReceiptStatement) |   [0-1]   | Параметры получения выписки в автоматическом режиме                         |
| Letters           | [Letters](#edo-Settings_Data_Letters)                   |   [0-1]   | Параметры обмена письмами                                                   |


>>##### <a name="edo-Settings_Data_Logon"></a> Logon:
| Параметр    | Тип                                                 |   Кратность    | Описание                           |
| ----------- | --------------------------------------------------- | :------------: | ---------------------------------- |
| Login       | [Login](#edo-Settings_Data_Logon_Login)             | выбор <br> [1] | По логину и паролю                 |
| Certificate | [Certificate](#edo-Settings_Data_Logon_Certificate) | выбор <br> [1] | По сертификату электронной подписи |


>>>###### <a name="edo-Settings_Data_Logon_Login"></a> Login:
| Параметр | Тип                       | Кратность | Описание           |
| -------- | ------------------------- | :-------: | ------------------ |
| User     | [string](#string) (до 50) |    [1]    | Логин пользователя |

>>>###### <a name="edo-Settings_Data_Logon_Certificate"></a> Certificate:
| Параметр            | Тип                       | Кратность | Описание                                     |
| ------------------- | ------------------------- | :-------: | -------------------------------------------- |
| EncryptingAlgorithm | [string](#string) (до 50) |    [1]    | Алгоритм шифрования, например, GOST 28147-89 |

>>##### <a name="edo-Settings_Data_CryptoParameters"></a> CryptoParameters:
| Параметр                   | Тип                                                                        | Кратность | Описание                                                                                         |
| -------------------------- | -------------------------------------------------------------------------- | :-------: | ------------------------------------------------------------------------------------------------ |
| CSPName                    | [string](#string) (до 256)                                                 |    [1]    | Имя CSP (cryptographic service provider)                                                         |
| CSPType                    | [int](#int)                                                                |    [1]    | Тип CSP (cryptographic service provider)                                                         |
| SignAlgorithm              | [string](#string) (до 50)                                                  |    [1]    | Алгоритм подписи, например, GOST R 34.10-2001                                                    |
| HashAlgorithm              | [string](#string) (до 50)                                                  |    [1]    | Алгоритм хэширования, например, GOST R 34.11-94                                                  |
| Encrypted                  | [Encrypted](#edo-Settings_Data_CryptoParameters_Encrypted)                 |   [0-1]   | Применение шифрования данных на прикладном уровне                                                |
| BankTrustedRootCertificate | [base64Binary](#base64Binary)                                              |   [0-1]   | Доверенный корневой сертификат УЦ банка                                                          |
| BankCertificate            | [base64Binary](#base64Binary)                                              |   [0-1]   | Сертификат электронной подписи банка                                                             |
| CustomerSignature          | [CustomerSignature](#edo-Settings_Data_CryptoParameters_CustomerSignature) |    [1]    | Карточка электронных подписей клиента                                                            |
| URLAddinInfo               | [string](#string)                                                          |   [0-1]   | Адрес-ссылка, откуда будет загружаться файл описания внешн.модуля, если он используется в обмене |

>>>###### <a name="edo-Settings_Data_CryptoParameters_Encrypted"></a> Encrypted:
| Параметр         | Тип                       | Кратность | Описание                                     |
| ---------------- | ------------------------- | :-------: | -------------------------------------------- |
| EncryptAlgorithm | [string](#string) (до 50) |    [1]    | Алгоритм шифрования, например, GOST 28147-89 |

>>>###### <a name="edo-Settings_Data_CryptoParameters_CustomerSignature"></a> CustomerSignature:
| Параметр        | Тип                                                                                      | Кратность | Описание                          |
| --------------- | ---------------------------------------------------------------------------------------- | :-------: | --------------------------------- |
| numberGroup     | [integer](#integer)                                                                      |    [1]    | Номер группы электронных подписей |
| GroupSignatures | [GroupSignatures](#edo-Settings_Data_CryptoParameters_CustomerSignature_GroupSignatures) |   [1-n]   | Группа электронных подписей       |

>>>>###### <a name="edo-Settings_Data_CryptoParameters_CustomerSignature_GroupSignatures"></a> GroupSignatures:
| Параметр    | Тип                           | Кратность | Описание                                     |
| ----------- | ----------------------------- | :-------: | -------------------------------------------- |
| Certificate | [base64Binary](#base64Binary) |   [1-9]   | Алгоритм шифрования, например, GOST 28147-89 |

>>##### <a name="edo-Settings_Data_Document"></a> Document:
| Параметр | Тип                                          | Кратность | Описание                                                               |
| -------- | -------------------------------------------- | :-------: | ---------------------------------------------------------------------- |
| docKind  | [DocKindType](#DocKindType)                  |    [1]    | Код вида электронного документа, как он задан в описании к стандарту   |
| Signed   | [Signed](#edo-Settings_Data_Document_Signed) |   [0-1]   | Применение электронной подписи для данного вида электронного документа |

>>>###### <a name="edo-Settings_Data_Document_Signed"></a> Signed:
| Параметр       | Тип               | Кратность | Описание                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| -------------- | ----------------- | :-------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RuleSignatures | [string](#string) |    [1]    | Правило, задающее наличие электронных подписей для данного вида электронного документа. Сложные правила подписания применяются только для платежных поручений и требований. Все запросы подписываются одной подписью.<br> Правила формируются следующим образом :<br> 1) при описании правила используется цифровой шифр; <br> 2) шифр содержит столько символов, сколько групп подписантов ([GroupSignatures](#edo-Settings_Data_CryptoParameters_CustomerSignature_GroupSignatures)) обозначено в настройках;<br>3) каждая цифра в шифре означает количество подписантов из группы N, где N – номер места цифры в шифре. Значение не может превышать 1.<br>4) каждый шифр заключает в скобки "()" (без кавычек);<br>5) для вида документа можно указать несколько правил, каждое из них описывается отдельно между собой правила отделяются символами "&#124;&#124;" (без кавычек).<br> Примеры:<br> а) Шифр "(110)" означает, что для этого вида документа требуется "подпись из 1-ой группы подписантов", "подпись из 2-ой группы подписантов" и "не требуется подпись подписантов из 3-ей группы".<br>б) Шифр "(110)&#124;&#124;(111)" означает, что для этого вида документа может быть использовано любое из 2-х правил:<br>см.п.(а)<br>или<br>требуется "подпись из 1-ой группы подписантов", "подпись из 2-ой группы подписантов", "подпись из 3-ой группы подписантов". |

>>##### <a name="edo-Settings_Data_ReceiptStatement"></a> ReceiptStatement:
| Параметр     | Тип                       | Кратность | Описание                                                 |
| ------------ | ------------------------- | :-------: | -------------------------------------------------------- |
| Login        | [string](#string) (до 50) |    [1]    | Логин, по которому можно получать только выписку банка   |
| Instructions | [string](#string)         |    [1]    | Инструкция по получению пароля для вышеуказанного логина |

>>##### <a name="edo-Settings_Data_Letters"></a> Letters:
| Параметр         | Тип                                                 | Кратность | Описание                                                  |
| ---------------- | --------------------------------------------------- | :-------: | --------------------------------------------------------- |
| AttachmentsLimit | [decimal](#decimal)                                 |    [1]    | Максимальный объем прикрепленных к письму файлов в байтах |
| LetterType       | [LetterType](#edo-Settings_Data_Letters_LetterType) |   [1-n]   | Типы писем, которые принимает банк                        |

>>>###### <a name="edo-Settings_Data_Letters_LetterType"></a> LetterType:
| Параметр | Тип               | Кратность | Описание                                               |
| -------- | ----------------- | :-------: | ------------------------------------------------------ |
| Code     | [string](#string) |    [1]    | Код типа письма                                        |
| Name     | [string](#string) |    [1]    | Представление типа письма для отображения пользователю |

### <a name="edo-SignatureType"></a> Тип edo:SignatureType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр         | Тип                           | Кратность | Описание                                                                  |
| ---------------- | ----------------------------- | :-------: | ------------------------------------------------------------------------- |
| x509IssuerName   | [string](#string)             |    [1]    | Имя издателя сертификата открытого ключа ЭП <br> (значение атрибута "CN") |
| x509SerialNumber | [hexBinary](#hexBinary)       |    [1]    | Серийный номер сертификата открытого ключа ЭП                             |
| SignedData       | [base64Binary](#base64Binary) |    [1]    | Электронная подпись                                                       |

### <a name="edo-Statement"></a> Тип edo:Statement (*[1C-Bank_Statement.xsd](../xsd-scheme/readme.md#1C-Bank_Statement)*)

| Параметр              | Тип                                     | Кратность | Описание                                        |
| --------------------- | --------------------------------------- | :-------: | ----------------------------------------------- |
| id                    | [IDType](#IDType)                       |    [1]    | Идентификатор выписки                           |
| formatVersion         | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                  |
| creationDate          | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования                       |
| userAgent             | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы                 |
| Sender                | [BankPartyType](#edo-BankPartyType)         |    [1]    | Отправитель                                     |
| Recipient             | [CustomerPartyType](#edo-CustomerPartyType) |    [1]    | Получатель                                      |
| Data                  | [Data](#edo-Statement_Data)             |    [1]    | Данные выписки по лиц. счету                    |
| ExtIDStatementRequest | [IDType](#IDType)                       |   [0-1]   | ID исходного запроса на выписку, если такой был |

>#### <a name="edo-Statement_Data"></a> Data:
| Параметр       | Тип                                                | Кратность | Описание                                                                                                                         |
| -------------- | -------------------------------------------------- | :-------: | -------------------------------------------------------------------------------------------------------------------------------- |
| StatementType  | [StatementKindType](#StatementKindType)            |    [1]    | Тип выписки                                                                                                                      |
| DateFrom       | [dateTime](#dateTime)                              |   [0-1]   | Начало периода выписки                                                                                                           |
| DateTo         | [dateTime](#dateTime)                              |    [1]    | Конец периода выписки                                                                                                            |
| Account        | [AccNumType](#AccNumType)                          |    [1]    | Номер лиц. счета                                                                                                                 |
| Bank           | [BankType](#edo-BankType)                              |    [1]    | Банк, в котором открыт счет                                                                                                      |
| OpeningBalance | [SumType](#SumType)                                |   [0-1]   | Остаток на счете на начало периода                                                                                               |
| TotalDebits    | [SumType](#SumType)                                |   [0-1]   | Общая сумма документов по дебету счета (списание). Обязательно заполняется при наличии расходных операций за период выписки.     |
| TotalCredits   | [SumType](#SumType)                                |   [0-1]   | Общая сумма документов по кредиту счета (поступление). Обязательно заполняется при наличии приходных операций за период выписки. |
| ClosingBalance | [SumType](#SumType)                                |    [1]    | Остаток на счете на конец периода                                                                                                |
| OperationInfo  | [OperationInfo](#edo-Statement_Data_OperationInfo) |   [0-n]   | Информация об одной операции по лицевому счету в выписке                                                                         |
| Stamp          | [Stamp](#edo-Statement_Data_Stamp)                 |   [0-1]   | Данные штампа банка по выписке в целом                                                                                           |

>>##### <a name="edo-Statement_Data_OperationInfo"></a> OperationInfo:
| Параметр | Тип                                                                          | Кратность | Описание                                                                                                                     |
| -------- | ---------------------------------------------------------------------------- | :-------: | ---------------------------------------------------------------------------------------------------------------------------- |
| PayDoc   | [PayDoc](#edo-Statement_Data_OperationInfo_PayDoc)                           |    [1]    | Данные платежного документа                                                                                                  |
| DC       | [string](#string) (1)                                                        |    [1]    | Признак дебета/кредита: <br>  1 - Операция по дебету (списание со счета), <br> 2 - Операция по кредиту (поступление на счет) |
| Date     | [date](#date)                                                                |    [1]    | Дата проводки документа по лиц. счету                                                                                        |
| ExtID    | [IDType](#IDType)                                                            |   [0-1]   | ID исходного платежного документа плательщика. Обязателен для заполнения для исходящих платежей.                             |
| Stamp    | [OperationInfo_Stamp](#edo-Statement_Data_OperationInfo_OperationInfo_Stamp) |   [0-1]   | Данные штампа банка по каждому платежному документу                                                                          |

>>##### <a name="edo-Statement_Data_Stamp"></a> Stamp:
>> - Базовый тип: [BankType](#BankType)
>>
| Параметр | Тип                        | Кратность | Описание        |
| -------- | -------------------------- | :-------: | --------------- |
| Branch   | [string](#string) (до 255) |   [0-1]   | Отделение банка |

>>>###### <a name="edo-Statement_Data_OperationInfo_PayDoc"></a> PayDoc:
>>>
| Параметр         | Тип                                           |   Кратность    | Описание                                                             |
| ---------------- | --------------------------------------------- | :------------: | -------------------------------------------------------------------- |
| id               | [IDType](#IDType)                             |      [1]       | ID платежного документа в банке                                      |
| docKind          | [DocKindType](#DocKindType)                   |      [1]       | Код вида электронного документа, как он задан в описании к стандарту |
| PayDocRu         | [PayDocRuApp](#edo-PayDocRuApp)                   | выбор <br> [1] | Данные платежного поручения                                          |
| PayRequest       | [PayRequestApp](#edo-PayRequestApp)               | выбор <br> [1] | Данные платежного требования                                         |
| CollectionOrder  | [CollectionOrderApp](#edo-CollectionOrderApp)     | выбор <br> [1] | Данные инкассового поручения                                         |
| PaymentOrder     | [PaymentOrderApp](#edo-PaymentOrderApp)           | выбор <br> [1] | Данные платежного ордера                                             |
| BankOrder        | [BankOrderApp](#edo-BankOrderApp)                 | выбор <br> [1] | Данные банковского ордера                                            |
| MemOrder         | [MemOrderApp](#edo-MemOrderApp)                   | выбор <br> [1] | Данные мемориального ордера                                          |
| InnerDoc         | [OtherPaymentDataType](#edo-OtherPaymentDataType) | выбор <br> [1] | Данные внутр.банковского документа                                   |
| CashContribution | [CashContributionType](#edo-CashContributionType) | выбор <br> [1] | Данные объявления на взнос наличными                                 |
| Check            | [CheckType](#edo-CheckType)                       | выбор <br> [1] | Данные денежного чека                                                |
>>>>###### <a name="edo-Statement_Data_OperationInfo_PayDoc_InnerDoc"></a> InnerDoc:
>>>> - Базовый тип: [OtherPaymentDataType](#edo-OtherPaymentDataType)
>>>>
| Параметр     | Тип                     | Кратность | Описание                                  |
| ------------ | ----------------------- | :-------: | ----------------------------------------- |
| InnerDocKind | [string](#string) (255) |    [1]    | Название типа внутр.банковского документа |

>>>###### <a name="edo-Statement_Data_OperationInfo_OperationInfo_Stamp"></a> OperationInfo_Stamp:
>>> - Базовый тип: [BankType](#edo-BankType)
>>>
| Параметр | Тип                        | Кратность | Описание                            |
| -------- | -------------------------- | :-------: | ----------------------------------- |
| Branch   | [string](#string) (до 255) |   [0-1]   | Отделение банка                     |
| Status   | [StatusType](#edo-StatusType)  |    [1]    | Статус платежного документа в банке |


### <a name="edo-StatementRequest"></a> Тип edo:StatementRequest (*[1C-Bank_StatementRequest.xsd](../xsd-scheme/readme.md#1C-Bank_StatementRequest)*)

| Параметр      | Тип                                     | Кратность | Описание                        |
| ------------- | --------------------------------------- | :-------: | ------------------------------- |
| id            | [IDType](#IDType)                       |    [1]    | Идентификатор запроса           |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы |
| Sender        | [CustomerPartyType](#edo-CustomerPartyType) |    [1]    | Отправитель                     |
| Recipient     | [BankPartyType](#edo-BankPartyType)         |    [1]    | Получатель                      |
| Data          | [Data](#edo-StatementRequest_Data)      |    [1]    | Данные запроса                  |
| Digest        | [DigestType](#edo-DigestType)               |   [0-1]   | Дайджест электронного документа |

> ##### <a name="edo-StatementRequest_Data"></a> Data:
| Параметр      | Тип                                     | Кратность | Описание                                     |
| ------------- | --------------------------------------- | :-------: | -------------------------------------------- |
| StatementType | [StatementKindType](#StatementKindType) |    [1]    | Тип выписки                                  |
| DateFrom      | [dateTime](#dateTime)                   |    [1]    | Начало периода формирования выписки          |
| DateTo        | [dateTime](#dateTime)                   |    [1]    | Конец периода формирования выписки           |
| Account       | [AccNumType](#AccNumType)               |    [1]    | Номер счета, по которому производится запрос |
| Bank          | [BankType](#edo-BankType)                   |    [1]    | Банк, в котором открыт счет                  |

### <a name="edo-StatusPacketNotice"></a> Тип edo:StatusPacketNotice (*[1C-Bank_StatusPacketNotice.xsd](../xsd-scheme/readme.md#1C-Bank_StatusPacketNotice)*)

| Параметр                | Тип                                     | Кратность | Описание                                                                   |
| ----------------------- | --------------------------------------- | :-------: | -------------------------------------------------------------------------- |
| id                      | [IDType](#IDType)                       |    [1]    | Идентификатор извещения                                                    |
| formatVersion           | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                             |
| creationDate            | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования                                                  |
| userAgent               | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы                                            |
| Sender                  | [ParticipantType](#edo-ParticipantType)     |    [1]    | Отправитель                                                                |
| Recipient               | [ParticipantType](#edo-ParticipantType)     |    [1]    | Получатель                                                                 |
| IDResultSuccessResponse | [IDType](#IDType)                       |    [1]    | ID, который сервис вернул в ответ после получения транспортного контейнера |
| Result                  | [ResultStatusType](#edo-ResultStatusType)   |    [1]    | Состояние электронного документа                                           |
| ExtIDPacket             | [IDType](#IDType)                       |   [0-1]   | ID исходного транспортного контейнера, по которому возвращается состояния  |

### <a name="edo-StatusDocNotice"></a> Тип edo:StatusDocNotice (*[1C-Bank_StatusDocNotice.xsd](../xsd-scheme/readme.md#1C-Bank_StatusDocNotice)*)

| Параметр           | Тип                                     | Кратность | Описание                                                                |
| ------------------ | --------------------------------------- | :-------: | ----------------------------------------------------------------------- |
| id                 | [IDType](#IDType)                       |    [1]    | Идентификатор извещения                                                 |
| formatVersion      | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                          |
| creationDate       | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования                                               |
| userAgent          | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы                                         |
| Sender             | [ParticipantType](#edo-ParticipantType)     |    [1]    | Отправитель                                                             |
| Recipient          | [ParticipantType](#edo-ParticipantType)     |    [1]    | Получатель                                                              |
| ExtID              | [IDType](#IDType)                       |    [1]    | ID исходного электронного документа, по которому возвращается состояния |
| Result             | [ResultStatusType](#edo-ResultStatusType)   |    [1]    | Состояние электронного документа                                        |
| ExtIDStatusRequest | [IDType](#IDType)                       |   [0-1]   | ID запроса о состоянии электронного документа, если был такой           |

### <a name="edo-StatusType"></a> Тип edo:StatusType (*[1C-Bank_Exch-Common.xsd](../xsd-scheme/readme.md#1C-Bank_Exch-Common)*)

| Параметр | Тип                       | Кратность | Описание                                                       |
| -------- | ------------------------- | :-------: | -------------------------------------------------------------- |
| Code     | [string](#string) (2)     |    [1]    | Код статуса, как он задан в описании к стандарту (см. таблицу) |
| Name     | [string](#string) (до 25) |   [0-1]   | Наименование статуса на стороне банка                          |
| MoreInfo | [string](#string)         |   [0-1]   | Дополнительная информация к статусу                            |

### <a name="edo-StatusRequest"></a> Тип edo:StatusRequest (*[1C-Bank_StatusRequest.xsd](../xsd-scheme/readme.md#1C-Bank_StatusRequest)*)

| Параметр      | Тип                                     | Кратность | Описание                                                                |
| ------------- | --------------------------------------- | :-------: | ----------------------------------------------------------------------- |
| id            | [IDType](#IDType)                       |    [1]    | Идентификатор запроса                                                   |
| formatVersion | [FormatVersionType](#FormatVersionType) |    [1]    | Версия формата                                                          |
| creationDate  | [dateTime](#dateTime)                   |    [1]    | Дата и время формирования                                               |
| userAgent     | [UserAgentType](#UserAgentType)         |   [0-1]   | Наименование и версия программы                                         |
| Sender        | [CustomerPartyType](#edo-CustomerPartyType) |    [1]    | Отправитель                                                             |
| Recipient     | [BankPartyType](#edo-BankPartyType)         |    [1]    | Получатель                                                              |
| ExtID         | [IDType](#IDType)                       |    [1]    | ID исходного электронного документа, статус которого требуется получить |


### <a name="edo-SuccessResultType"></a> Тип edo:SuccessResultType (*[1C-Bank_ResultBank.xsd](../xsd-scheme/readme.md#1C-Bank_ResultBank)*)

| Параметр              | Тип                                                     |   Кратность    | Описание                                                                       |
| --------------------- | ------------------------------------------------------- | :------------: | ------------------------------------------------------------------------------ |
| SendPacketResponse    | [SendPacketResponseType](#edo-SendPacketResponseType)       | выбор <br> [1] | Отправка транспортного контейнера в банк                                       |
| GetPacketListResponse | [GetPacketListResponseType](#edo-GetPacketListResponseType) | выбор <br> [1] | Список ID транспортных контейнеров, готовых к передачи клиенту                 |
| GetPacketResponse     | [Packet](#edo-Packet)                               | выбор <br> [1] | Транспортный контейнер с данными электронных документов для получения клиентом |
| LogonResponse         | [LogonResponseType](#edo-LogonResponseType)                 | выбор <br> [1] | Аутентификация по логину + ОТР (опционально)                                   |
| LogonCertResponse     | [LogonCertResponseType](#edo-LogonCertResponseType)         | выбор <br> [1] | Аутентификация по сертификату                                                  |
| GetSettingsResponse   | [GetSettingsResponseType](#edo-GetSettingsResponseType)     | выбор <br> [1] | Получение настроек обмена в автоматическом режиме                              |
