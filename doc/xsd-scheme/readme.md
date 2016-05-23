# ВНИМАНИЕ! Это не финальная версия, раздел может быть дополнен.

# Схемы данных

+ [XML-схема **транспортного контейнера**: *1C-Bank_Packet.xsd*](#1C-Bank_Packet)
+ [XML-схема **ответа банковского сервиса**: *1C-Bank_ResultBank.xsd*](#1C-Bank_ResultBank)
+ [XML-схема **запроса-зонда**: *1C-Bank_Probe.xsd*](#1C-Bank_Probe)
+ [XML-схема **извещения о состояния обработки транспортного контейнера**: *1C-Bank_StatusPacketNotice.xsd*](#1C-Bank_StatusPacketNotice)
+ [XML-схема **платежного поручения**: *1C-Bank_PayDocRu.xsd*](#1C-Bank_PayDocRu)
+ [XML-схема **платежного требования**: *1C-Bank_PayRequest.xsd*](#1C-Bank_PayRequest)
+ [XML-схема **извещения о состоянии электронного документа**: *1C-Bank_StatusDocNotice.xsd*](#1C-Bank_StatusDocNotice)
+ [XML-схема **запроса выписки**: *1C-Bank_StatementRequest.xsd*](#1C-Bank_StatementRequest)
+ [XML-схема **выписки банка**: *1C-Bank_Statement.xsd*](#1C-Bank_Statement)
+ [XML-схема **запроса о состоянии электронного документа**: *1C-Bank_StatusRequest.xsd*](#1C-Bank_StatusRequest)
+ [XML-схема **запрос об отзыве электронного документа**: *1C-Bank_CancelationRequest.xsd*](#1C-Bank_CancelationRequest)
+ [XML-схема **настроек обмена с банком**: *1C-Bank_Settings.xsd*](#1C-Bank_Settings)
+ [XML-схема **данных для аутентификации по закр.ключу**: *1C-Bank_AuthSign.xsd*](#1C-Bank_AuthSign)
+ [XML-схема **описания общих типов**: *1C-Bank_Exch-Common.xsd*](#1C-Bank_Exch-Common)
+ [XML-схема **библиотеки**: *1C-Bank_Library.xsd*](#1C-Bank_Library)


## <a name="1C-Bank_Packet"></a> XML-схема транспортного контейнера: *1C-Bank_Packet.xsd*

- [Файл схемы **1C-Bank_Packet.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_Packet.xsd)

## <a name="1C-Bank_ResultBank"></a> XML-схема **ответа банковского сервиса**: *1C-Bank_ResultBank.xsd*

- [Файл схемы **1C-Bank_ResultBank.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_ResultBank.xsd)

## <a name="1C-Bank_Probe"></a> XML-схема **запроса-зонда**: *1C-Bank_Probe.xsd*

- [Файл схемы **1C-Bank_Probe.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_Probe.xsd)

##### Описание:

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | Идентификатор запроса           |
| formatVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [BankPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankPartyType)     |    [1]    | Отправитель                     |
| Recipient     | [CustomerPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#CustomerPartyType) |    [1]    | Получатель                      |
| Digest        | [DigestType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#DigestType)        |   [0-1]   | Дайджест электронного документа |

## <a name="1C-Bank_StatusPacketNotice"></a> XML-схема **извещения о состояния обработки транспортного контейнера**: *1C-Bank_StatusPacketNotice.xsd*

- [Файл схемы **1C-Bank_StatusPacketNotice.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_StatusPacketNotice.xsd)

## <a name="1C-Bank_PayDocRu"></a> XML-схема **платежного поручения**: *1C-Bank_PayDocRu.xsd*

- [Файл схемы **1C-Bank_PayDocRu.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_PayDocRu.xsd)

##### Описание:

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | Идентификатор платежа           |
| formatVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [BankPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankPartyType)     |    [1]    | Отправитель                     |
| Recipient     | [CustomerPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#CustomerPartyType) |    [1]    | Получатель                      |
| Data          | [PayDocRuApp](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#PayDocRuApp)       |    [1]    | Данные платежного поручения     |
| Digest        | [DigestType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#DigestType)        |   [0-1]   | Дайджест электронного документа |


## <a name="1C-Bank_PayRequest"></a> XML-схема **платежного требования**: *1C-Bank_PayRequest.xsd*

- [Файл схемы **1C-Bank_PayRequest.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_PayRequest.xsd)

##### Описание:

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | Идентификатор требования        |
| formatVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [BankPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankPartyType)     |    [1]    | Отправитель                     |
| Recipient     | [CustomerPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#CustomerPartyType) |    [1]    | Получатель                      |
| Data          | [PayRequestApp](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#PayRequestApp)     |    [1]    | Данные платежного требования    |
| Digest        | [DigestType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#DigestType)        |   [0-1]   | Дайджест электронного документа |


## <a name="1C-Bank_StatusDocNotice"></a> XML-схема **извещения о состоянии электронного документа**: *1C-Bank_StatusDocNotice.xsd*

- [Файл схемы **1C-Bank_StatusDocNotice.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_StatusDocNotice.xsd)

##### Описание:

| Параметр           | Тип               | Кратность | Описание                                                                |
|--------------------|-------------------|:---------:|-------------------------------------------------------------------------|
| id                 | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | Идентификатор извещения                                                 |
| formatVersion      | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) |    [1]    | Версия формата                                                          |
| creationDate       | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Дата и время формирования                                               |
| userAgent          | [UserAgentType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#UserAgentType)     |   [0-1]   | Наименование и версия программы                                         |
| Sender             | [BankPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankPartyType)     |    [1]    | Отправитель                                                             |
| Recipient          | [CustomerPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#CustomerPartyType) |    [1]    | Получатель                                                              |
| ExtID              | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | ID исходного электронного документа, по которому возвращается состояния |
| Result             | [ResultStatusType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#ResultStatusType)  |    [1]    | Состояние электронного документа                                        |
| ExtIDStatusRequest | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |   [0-1]   | ID запроса о состоянии электронного документа, если был такой           |

## <a name="1C-Bank_StatementRequest"></a> XML-схема **запроса выписки**: *1C-Bank_StatementRequest.xsd*

- [Файл схемы **1C-Bank_StatementRequest.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_StatementRequest.xsd)

##### Описание:

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | Идентификатор запроса           |
| formatVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [BankPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankPartyType)     |    [1]    | Отправитель                     |
| Recipient     | [CustomerPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#CustomerPartyType) |    [1]    | Получатель                      |
| Data          | [Data](#1C-Bank_StatementRequest_Data)              |    [1]    | Данные запроса                  |
| Digest        | [DigestType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#DigestType)        |   [0-1]   | Дайджест электронного документа |

> ###### <a name="1C-Bank_StatementRequest_Data"></a> Data:
| Параметр      | Тип               | Кратность | Описание                                     |
|---------------|-------------------|:---------:|----------------------------------------------|
| StatementType | [StatementKindType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#StatementKindType) |    [1]    | Тип выписки                                  |
| DateFrom      | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Начало периода формирования выписки          |
| DateTo        | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Конец периода формирования выписки           |
| Account       | [AccNumType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#AccNumType)        |    [1]    | Номер счета, по которому производится запрос |
| Bank          | [BankType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankType)          |    [1]    | Банк, в котором открыт счет                  |

## <a name="1C-Bank_Statement"></a> XML-схема **выписки банка**: *1C-Bank_Statement.xsd*

- [Файл схемы **1C-Bank_Statement.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_Statement.xsd)

##### Описание:

| Параметр              | Тип               | Кратность | Описание                                        |
|-----------------------|-------------------|:---------:|-------------------------------------------------|
| id                    | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | Идентификатор выписки                           |
| formatVersion         | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) |    [1]    | Версия формата                                  |
| creationDate          | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Дата и время формирования                       |
| userAgent             | [UserAgentType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#UserAgentType)     |   [0-1]   | Наименование и версия программы                 |
| Sender                | [BankPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankPartyType)     |    [1]    | Отправитель                                     |
| Recipient             | [CustomerPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#CustomerPartyType) |    [1]    | Получатель                                      |
| Data                  | [Data](#1C-Bank_Statement_Data)              |    [1]    | Данные выписки по лиц. счету                    |
| ExtIDStatementRequest | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |   [0-1]   | ID исходного запроса на выписку, если такой был |

>###### <a name="1C-Bank_Statement_Data"></a> Data:
| Параметр       | Тип               | Кратность | Описание                                                 |
|----------------|-------------------|:---------:|----------------------------------------------------------|
| StatementType  | [StatementKindType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#StatementKindType) |    [1]    | Тип выписки                                              |
| DateFrom       | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |   [0-1]   | Начало периода выписки                                   |
| DateTo         | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Конец периода выписки                                    |
| Account        | [AccNumType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#AccNumType)        |    [1]    | Номер лиц. счета                                         |
| Bank           | [BankType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankType)          |    [1]    | Банк, в котором открыт счет                              |
| OpeningBalance | [SumType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#SumType)           |   [0-1]   | Остаток на счете на начало периода                       |
| TotalDebits    | [SumType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#SumType)           |   [0-1]   | Общая сумма документов по дебету счета                   |
| TotalCredits   | [SumType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#SumType)           |   [0-1]   | Общая сумма документов по кредиту счета                  |
| ClosingBalance | [SumType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#SumType)           |    [1]    | Общая сумма документов по кредиту счета                  |
| OperationInfo  | [OperationInfo](#1C-Bank_Statement_Data_OperationInfo)     |   [0-n]   | Информация об одной операции по лицевому счету в выписке |
| Stamp          | [Stamp](#1C-Bank_Statement_Data_Stamp)             |   [0-1]   | Данные штампа банка по выписке в целом                   |

>>###### <a name="1C-Bank_Statement_Data_OperationInfo"></a> OperationInfo:
| Параметр | Тип                 | Кратность | Описание                                            |
|----------|---------------------|:---------:|-----------------------------------------------------|
| PayDoc   | [PayDoc](#1C-Bank_Statement_Data_OperationInfo_PayDoc)              |    [1]    | Данные платежного документа                         |
| DC       | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (1)          |    [1]    | Признак дебета/кредита: <br>  1 - Операция по дебету, <br> 2 - Операция по кредиту                                |
| Date     | [date](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#date)                |    [1]    | Дата проводки документа по лиц. счету               |
| ExtID    | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)              |   [0-1]   | ID исходного платежного документа плательщика       |
| Stamp    | [OperationInfo_Stamp](#1C-Bank_Statement_Data_OperationInfo_OperationInfo_Stamp) |    [1]    | Данные штампа банка по каждому платежному документу |

>>###### <a name="1C-Bank_Statement_Data_Stamp"></a> Stamp:
| Параметр               | Тип             | Кратность | Описание        |
|------------------------|-----------------|:---------:|-----------------|
| Тип значения: [BankType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankType) |                 |           |                 |
| Branch                 | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (до 255) |    [1]    | Отделение банка |

-----

>>>###### <a name="1C-Bank_Statement_Data_OperationInfo_PayDoc"></a> PayDoc:
| Параметр               | Тип             | Кратность | Описание        |
|------------------------|-----------------|:---------:|-----------------|
| Тип значения: [BankType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankType) |                 |           |                 |
| Branch                 | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (до 255) |    [1]    | Отделение банка |


>>>###### <a name="1C-Bank_Statement_Data_OperationInfo_OperationInfo_Stamp"></a> OperationInfo_Stamp:
| Параметр               | Тип             | Кратность | Описание                            |
|------------------------|-----------------|:---------:|-------------------------------------|
| Тип значения: [BankType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankType) |                 |           |                                     |
| Branch                 | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (до 255) |    [1]    | Отделение банка                     |
| Status                 | [StatusType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#StatusType)      |    [1]    | Статус платежного документа в банке |

## <a name="1C-Bank_StatusRequest"></a> XML-схема **запроса о состоянии электронного документа**: *1C-Bank_StatusRequest.xsd*

- [Файл схемы **1C-Bank_StatusRequest.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_StatusRequest.xsd)

##### Описание:

| Параметр      | Тип               | Кратность | Описание                                                                |
|---------------|-------------------|:---------:|-------------------------------------------------------------------------|
| id            | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | Идентификатор запроса                                                   |
| formatVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) |    [1]    | Версия формата                                                          |
| creationDate  | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Дата и время формирования                                               |
| userAgent     | [UserAgentType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#UserAgentType)     |   [0-1]   | Наименование и версия программы                                         |
| Sender        | [BankPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankPartyType)     |    [1]    | Отправитель                                                             |
| Recipient     | [CustomerPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#CustomerPartyType) |    [1]    | Получатель                                                              |
| ExtID         | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | ID исходного электронного документа, статус которого требуется получить |

## <a name="1C-Bank_CancelationRequest"></a> XML-схема **запрос об отзыве электронного документа**: *1C-Bank_CancelationRequest.xsd*

- [Файл схемы **1C-Bank_CancelationRequest.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_CancelationRequest.xsd)

##### Описание:

| Параметр      | Тип               | Кратность | Описание                                                        |
|---------------|-------------------|:---------:|-----------------------------------------------------------------|
| id            | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | Идентификатор запроса                                           |
| formatVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) |    [1]    | Версия формата                                                  |
| creationDate  | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Дата и время формирования                                       |
| userAgent     | [UserAgentType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#UserAgentType)     |   [0-1]   | Наименование и версия программы                                 |
| Sender        | [BankPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankPartyType)     |    [1]    | Отправитель                                                     |
| Recipient     | [CustomerPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#CustomerPartyType) |    [1]    | Получатель                                                      |
| ExtID         | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | ID исходного электронного документа, который требуется отозвать |
| Reason        | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            |   [0-1]   | Причина, основание отзыва электронного документа                |

## <a name="1C-Bank_Settings"></a> XML-схема **настроек обмена с банком**: *1C-Bank_Settings.xsd*

- [Файл схемы **1C-Bank_Settings.xsd**](http://https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_Settings.xsd)

##### Описание:

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | [IDType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDType)            |    [1]    | Идентификатор набора данных     |
| formatVersion | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) |    [1]    | Версия формата                  |
| creationDate  | [dateTime](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#dateTime)          |    [1]    | Дата и время формирования       |
| userAgent     | [UserAgentType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#UserAgentType)     |   [0-1]   | Наименование и версия программы |
| Sender        | [BankPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#BankPartyType)     |    [1]    | Отправитель                     |
| Recipient     | [CustomerPartyType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#CustomerPartyType) |    [1]    | Получатель                      |
| Data          | [Data](#1C-Bank_Settings_Data)              |   [1-n]   | Параметры обмена                |

>###### <a name="1C-Bank_Settings_Data"></a> Data:
| Параметр          | Тип               | Кратность | Описание                                                                    |
|-------------------|-------------------|:---------:|-----------------------------------------------------------------------------|
| CustomerID        | [IDCustomerType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#IDCustomerType)    |    [1]    | Уникальный идентификатор клиента в банке                                    |
| BankServerAddress | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (от 1)     |    [1]    | Адрес ресурса банка                                                         |
| FormatVersion     | [FormatVersionType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#FormatVersionType) |    [1]    | Актуальная версия формата обмена данными                                    |
| Encoding          | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            |    [1]    | Кодировка файлов обмена. По умолчанию: "UTF-8"                              |
| Compress          | [boolean](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#boolean)           |   [0-1]   | Признак сжатия электронных документов при обмене. По умолчанию: false       |
| Logon             | [Logon](#1C-Bank_Settings_Data_Logon)             |    [1]    | Способ аутентификации на ресурсе банка                                      |
| CryptoParameters  | [CryptoParameters](#1C-Bank_Settings_Data_CryptoParameters)  |   [0-n]   | Настройки криптографии                                                      |
| Document          | [Document](#1C-Bank_Settings_Data_Document)          |   [1-n]   | Настройки по видам электронных документов, которыми возможен обмен с банком |


>>###### <a name="1C-Bank_Settings_Data_Logon"></a> Logon:
| Параметр    | Тип         | Кратность | Описание                           |
|-------------|-------------|:---------:|------------------------------------|
| Login       | [Login](#1C-Bank_Settings_Data_Logon_Login)       |    [1]    | По логину и паролю                 |
| Certificate | [Certificate](#1C-Bank_Settings_Data_Logon_Certificate) |    [1]    | По сертификату электронной подписи |


>>>###### <a name="1C-Bank_Settings_Data_Logon_Login"></a> Login:
| Параметр | Тип            | Кратность | Описание           |
|----------|----------------|:---------:|--------------------|
| User     | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (до 50) |    [1]    | Логин пользователя |

>>>###### <a name="1C-Bank_Settings_Data_Logon_Certificate"></a> Certificate:
| Параметр            | Тип            | Кратность | Описание                                     |
|---------------------|----------------|:---------:|----------------------------------------------|
| EncryptingAlgorithm | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (до 50) |    [1]    | Алгоритм шифрования, например, GOST 28147-89 |

>>###### <a name="1C-Bank_Settings_Data_CryptoParameters"></a> CryptoParameters:
| Параметр                   | Тип               | Кратность | Описание                                                                                         |
|----------------------------|-------------------|:---------:|--------------------------------------------------------------------------------------------------|
| CSPName                    | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (до 256)   |    [1]    | Имя CSP (cryptographic service provider)                                                         |
| CSPType                    | [int](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#int)               |    [1]    | Тип CSP (cryptographic service provider)                                                         |
| SignAlgorithm              | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (до 50)    |    [1]    | Алгоритм подписи, например, GOST R 34.10-2001                                                    |
| HashAlgorithm              | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (до 50)    |    [1]    | Алгоритм хэширования, например, GOST R 34.11-94                                                  |
| Encrypted                  | [Encrypted](#1C-Bank_Settings_Data_CryptoParameters_Encrypted)         |   [1-n]   | Применение шифрования данных на прикладном уровне                                                |
| BankTrustedRootCertificate | [base64Binary](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#base64Binary)      |   [0-1]   | Доверенный корневой сертификат УЦ банка                                                          |
| BankCertificate            | [base64Binary](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#base64Binary)      |   [0-1]   | Сертификат электронной подписи банка                                                             |
| CustomerSignature          | [CustomerSignature](#1C-Bank_Settings_Data_CryptoParameters_CustomerSignature) |   [1-n]   | Карточка электронных подписей клиента                                                            |
| URLAddinInfo               | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string)            |   [0-1]   | Адрес-ссылка, откуда будет загружаться файл описания внешн.модуля, если он используется в обмене |

>>>###### <a name="1C-Bank_Settings_Data_CryptoParameters_Encrypted"></a> Encrypted:
| Параметр         | Тип            | Кратность | Описание                                     |
|------------------|----------------|:---------:|----------------------------------------------|
| EncryptAlgorithm | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) (до 50) |    [1]    | Алгоритм шифрования, например, GOST 28147-89 |

>>>###### <a name="1C-Bank_Settings_Data_CryptoParameters_CustomerSignature"></a> CustomerSignature:
| Параметр        | Тип             | Кратность | Описание                          |
|-----------------|-----------------|:---------:|-----------------------------------|
| numberGroup     | [integer](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#integer)         |    [1]    | Номер группы электронных подписей |
| GroupSignatures | [GroupSignatures](#1C-Bank_Settings_Data_CryptoParameters_CustomerSignature_GroupSignatures) |   [1-n]   | Группа электронных подписей       |

>>>>###### <a name="1C-Bank_Settings_Data_CryptoParameters_CustomerSignature_GroupSignatures"></a> GroupSignatures:
| Параметр    | Тип          | Кратность | Описание                                     |
|-------------|--------------|:---------:|----------------------------------------------|
| Certificate | [base64Binary](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#base64Binary) |    [1]    | Алгоритм шифрования, например, GOST 28147-89 |

>>###### <a name="1C-Bank_Settings_Data_Document"></a> Document:
| Параметр | Тип         | Кратность | Описание                                                               |
|----------|-------------|:---------:|------------------------------------------------------------------------|
| docKind  | [DocKindType](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#DocKindType) |    [1]    | Код вида электронного документа, как он задан в описании к стандарту   |
| Signed   | [Signed](#1C-Bank_Settings_Data_Document_Signed)      |   [0-1]   | Применение электронной подписи для данного вида электронного документа |

>>>###### <a name="1C-Bank_Settings_Data_Document_Signed"></a> Signed:
| Параметр       | Тип    | Кратность | Описание                                                                               |
|----------------|--------|:---------:|----------------------------------------------------------------------------------------|
| RuleSignatures | [string](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#string) |   [1-n]   | Правило, задающее наличие электронных подписей для данного вида электронного документа |

## <a name="1C-Bank_AuthSign"></a> XML-схема **данных для аутентификации по закр.ключу**: *1C-Bank_AuthSign.xsd*

- [Файл схемы **1C-Bank_AuthSign.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_AuthSign.xsd)

## <a name="1C-Bank_Exch-Common"></a> XML-схема **описания общих типов**: *1C-Bank_Exch-Common.xsd*

- [Файл схемы **1C-Bank_Exch-Common.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_Exch-Common.xsd)

## <a name="1C-Bank_Library"></a> XML-схема **библиотеки**: *1C-Bank_Library.xsd*

- [Файл схемы **1C-Bank_Library.xsd**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/xsd-scheme/1C-Bank_Library.xsd)


