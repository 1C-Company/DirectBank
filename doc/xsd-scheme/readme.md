# Схемы данных

+ XML-схема **транспортного контейнера**: *1C-Bank_Packet.xsd*
+ XML-схема **ответа банковского сервиса**: *1C-Bank_ResultBank.xsd*
+ XML-схема **запроса-зонда**: *1C-Bank_Probe.xsd*
+ XML-схема **извещения о состояния обработки транспортного контейнера**: *1C-Bank_StatusPacketNotice.xsd*
+ XML-схема **платежного поручения**: *1C-Bank_PayDocRu.xsd*
+ XML-схема **платежного требования**: *1C-Bank_PayRequest.xsd*
+ XML-схема **извещения о состоянии электронного документа**: *1C-Bank_StatusDocNotice.xsd*
+ XML-схема **запроса выписки**: *1C-Bank_StatementRequest.xsd*
+ XML-схема **выписки банка**: *1C-Bank_Statement.xsd*
+ XML-схема **запроса о состоянии электронного документа**: *1C-Bank_StatusRequest.xsd*
+ XML-схема **запрос об отзыве электронного документа**: *1C-Bank_CancelationRequest.xsd*
+ XML-схема **настроек обмена с банком**: *1C-Bank_Settings.xsd*
+ XML-схема **данных для аутентификации по закр.ключу**: *1C-Bank_AuthSign.xsd*
+ XML-схема **описания общих типов**: *1C-Bank_Exch-Common.xsd*
+ XML-схема **библиотеки**: *1C-Bank_Library.xsd*


## <a name="1C-Bank_Packet"></a> XML-схема транспортного контейнера: *1C-Bank_Packet.xsd*

[Скачать](http://) файл схемы 1C-Bank_Packet.xsd

## <a name="1C-Bank_ResultBank"></a> XML-схема **ответа банковского сервиса**: *1C-Bank_ResultBank.xsd*

[Скачать](http://) файл схемы 1C-Bank_ResultBank.xsd

## <a name="1C-Bank_Probe"></a> XML-схема **запроса-зонда**: *1C-Bank_Probe.xsd*

[Скачать](http://) файл схемы 1C-Bank_Probe.xsd

##### Описание:

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | IDType            |    [1]    | Идентификатор запроса           |
| formatVersion | FormatVersionType |    [1]    | Версия формата                  |
| creationDate  | dateTime          |    [1]    | Дата и время формирования       |
| userAgent     | UserAgentType     |   [0-1]   | Наименование и версия программы |
| Sender        | BankPartyType     |    [1]    | Отправитель                     |
| Recipient     | CustomerPartyType |    [1]    | Получатель                      |
| Digest        | DigestType        |   [0-1]   | Дайджест электронного документа |

## <a name="1C-Bank_StatusPacketNotice"></a> XML-схема **извещения о состояния обработки транспортного контейнера**: *1C-Bank_StatusPacketNotice.xsd*

[Скачать](http://) файл схемы 1C-Bank_StatusPacketNotice.xsd

## <a name="1C-Bank_PayDocRu"></a> XML-схема **платежного поручения**: *1C-Bank_PayDocRu.xsd*

[Скачать](http://) файл схемы 1C-Bank_PayDocRu.xsd

##### Описание:

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | IDType            |    [1]    | Идентификатор платежа           |
| formatVersion | FormatVersionType |    [1]    | Версия формата                  |
| creationDate  | dateTime          |    [1]    | Дата и время формирования       |
| userAgent     | UserAgentType     |   [0-1]   | Наименование и версия программы |
| Sender        | BankPartyType     |    [1]    | Отправитель                     |
| Recipient     | CustomerPartyType |    [1]    | Получатель                      |
| Data          | PayDocRuApp       |    [1]    | Данные платежного поручения     |
| Digest        | DigestType        |   [0-1]   | Дайджест электронного документа |


## <a name="1C-Bank_PayRequest"></a> XML-схема **платежного требования**: *1C-Bank_PayRequest.xsd*

[Скачать](http://) файл схемы 1C-Bank_PayRequest.xsd

##### Описание:

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | IDType            |    [1]    | Идентификатор требования        |
| formatVersion | FormatVersionType |    [1]    | Версия формата                  |
| creationDate  | dateTime          |    [1]    | Дата и время формирования       |
| userAgent     | UserAgentType     |   [0-1]   | Наименование и версия программы |
| Sender        | BankPartyType     |    [1]    | Отправитель                     |
| Recipient     | CustomerPartyType |    [1]    | Получатель                      |
| Data          | PayRequestApp     |    [1]    | Данные платежного требования    |
| Digest        | DigestType        |   [0-1]   | Дайджест электронного документа |


## <a name="1C-Bank_StatusDocNotice"></a> XML-схема **извещения о состоянии электронного документа**: *1C-Bank_StatusDocNotice.xsd*

[Скачать](http://) файл схемы 1C-Bank_StatusDocNotice.xsd

##### Описание:

| Параметр           | Тип               | Кратность | Описание                                                                |
|--------------------|-------------------|:---------:|-------------------------------------------------------------------------|
| id                 | IDType            |    [1]    | Идентификатор извещения                                                 |
| formatVersion      | FormatVersionType |    [1]    | Версия формата                                                          |
| creationDate       | dateTime          |    [1]    | Дата и время формирования                                               |
| userAgent          | UserAgentType     |   [0-1]   | Наименование и версия программы                                         |
| Sender             | BankPartyType     |    [1]    | Отправитель                                                             |
| Recipient          | CustomerPartyType |    [1]    | Получатель                                                              |
| ExtID              | IDType            |    [1]    | ID исходного электронного документа, по которому возвращается состояния |
| Result             | ResultStatusType  |    [1]    | Состояние электронного документа                                        |
| ExtIDStatusRequest | IDType            |   [0-1]   | ID запроса о состоянии электронного документа, если был такой           |

## <a name="1C-Bank_StatementRequest"></a> XML-схема **запроса выписки**: *1C-Bank_StatementRequest.xsd*

[Скачать](http://) файл схемы 1C-Bank_StatementRequest.xsd

##### Описание:

| Параметр      | Тип               | Кратность | Описание                        |
|---------------|-------------------|:---------:|---------------------------------|
| id            | IDType            |    [1]    | Идентификатор запроса           |
| formatVersion | FormatVersionType |    [1]    | Версия формата                  |
| creationDate  | dateTime          |    [1]    | Дата и время формирования       |
| userAgent     | UserAgentType     |   [0-1]   | Наименование и версия программы |
| Sender        | BankPartyType     |    [1]    | Отправитель                     |
| Recipient     | CustomerPartyType |    [1]    | Получатель                      |
| Data          | Data              |    [1]    | Данные запроса                  |
| Digest        | DigestType        |   [0-1]   | Дайджест электронного документа |

###### <a name="1C-Bank_StatementRequest_Data"></a> Data:

| Параметр      | Тип               | Кратность | Описание                                     |
|---------------|-------------------|:---------:|----------------------------------------------|
| StatementType | StatementKindType |    [1]    | Тип выписки                                  |
| DateFrom      | dateTime          |    [1]    | Начало периода формирования выписки          |
| DateTo        | dateTime          |    [1]    | Конец периода формирования выписки           |
| Account       | AccNumType        |    [1]    | Номер счета, по которому производится запрос |
| Bank          | BankType          |    [1]    | Банк, в котором открыт счет                  |

## <a name="1C-Bank_Statement"></a> XML-схема **выписки банка**: *1C-Bank_Statement.xsd*

[Скачать](http://) файл схемы 1C-Bank_Statement.xsd

##### Описание:

| Параметр              | Тип               | Кратность | Описание                                        |
|-----------------------|-------------------|:---------:|-------------------------------------------------|
| id                    | IDType            |    [1]    | Идентификатор выписки                           |
| formatVersion         | FormatVersionType |    [1]    | Версия формата                                  |
| creationDate          | dateTime          |    [1]    | Дата и время формирования                       |
| userAgent             | UserAgentType     |   [0-1]   | Наименование и версия программы                 |
| Sender                | BankPartyType     |    [1]    | Отправитель                                     |
| Recipient             | CustomerPartyType |    [1]    | Получатель                                      |
| Data                  | Data              |    [1]    | Данные выписки по лиц. счету                    |
| ExtIDStatementRequest | IDType            |   [0-1]   | ID исходного запроса на выписку, если такой был |

###### <a name="1C-Bank_Statement_Data"></a> Data:
| Параметр       | Тип               | Кратность | Описание                                                 |
|----------------|-------------------|:---------:|----------------------------------------------------------|
| StatementType  | StatementKindType |    [1]    | Тип выписки                                              |
| DateFrom       | dateTime          |   [0-1]   | Начало периода выписки                                   |
| DateTo         | dateTime          |    [1]    | Конец периода выписки                                    |
| Account        | AccNumType        |    [1]    | Номер лиц. счета                                         |
| Bank           | BankType          |    [1]    | Банк, в котором открыт счет                              |
| OpeningBalance | SumType           |   [0-1]   | Остаток на счете на начало периода                       |
| TotalDebits    | SumType           |   [0-1]   | Общая сумма документов по дебету счета                   |
| TotalCredits   | SumType           |   [0-1]   | Общая сумма документов по кредиту счета                  |
| ClosingBalance | SumType           |    [1]    | Общая сумма документов по кредиту счета                  |
| OperationInfo  | OperationInfo     |   [0-n]   | Информация об одной операции по лицевому счету в выписке |
| Stamp          | Stamp             |   [0-1]   | Данные штампа банка по выписке в целом                   |

-----

###### <a name="1C-Bank_Statement_Data_OperationInfo"></a> OperationInfo:
| Параметр | Тип                 | Кратность | Описание                                            |
|----------|---------------------|:---------:|-----------------------------------------------------|
| PayDoc   | PayDoc              |    [1]    | Данные платежного документа                         |
| DC       | string (1)          |    [1]    | Признак дебета/кредита: <br>  1 - Операция по дебету, <br> 2 - Операция по кредиту                                |
| Date     | date                |    [1]    | Дата проводки документа по лиц. счету               |
| ExtID    | IDType              |   [0-1]   | ID исходного платежного документа плательщика       |
| Stamp    | OperationInfo_Stamp |    [1]    | Данные штампа банка по каждому платежному документу |

###### <a name="1C-Bank_Statement_Data_Stamp"></a> Stamp:
| Параметр               | Тип             | Кратность | Описание        |
|------------------------|-----------------|:---------:|-----------------|
| Тип значения: BankType |                 |           |                 |
| Branch                 | string (до 255) |    [1]    | Отделение банка |

-----

###### <a name="1C-Bank_Statement_Data_OperationInfo_PayDoc"></a> PayDoc:
| Параметр               | Тип             | Кратность | Описание        |
|------------------------|-----------------|:---------:|-----------------|
| Тип значения: BankType |                 |           |                 |
| Branch                 | string (до 255) |    [1]    | Отделение банка |


###### <a name="1C-Bank_Statement_Data_OperationInfo_OperationInfo_Stamp"></a> OperationInfo_Stamp:

| Параметр               | Тип             | Кратность | Описание                            |
|------------------------|-----------------|:---------:|-------------------------------------|
| Тип значения: BankType |                 |           |                                     |
| Branch                 | string (до 255) |    [1]    | Отделение банка                     |
| Status                 | StatusType      |    [1]    | Статус платежного документа в банке |

## <a name="1C-Bank_StatusRequest"></a> XML-схема **запроса о состоянии электронного документа**: *1C-Bank_StatusRequest.xsd*

[Скачать](http://) файл схемы 1C-Bank_StatusRequest.xsd

##### Описание:

| Параметр      | Тип               | Кратность | Описание                                                                |
|---------------|-------------------|:---------:|-------------------------------------------------------------------------|
| id            | IDType            |    [1]    | Идентификатор запроса                                                   |
| formatVersion | FormatVersionType |    [1]    | Версия формата                                                          |
| creationDate  | dateTime          |    [1]    | Дата и время формирования                                               |
| userAgent     | UserAgentType     |   [0-1]   | Наименование и версия программы                                         |
| Sender        | BankPartyType     |    [1]    | Отправитель                                                             |
| Recipient     | CustomerPartyType |    [1]    | Получатель                                                              |
| ExtID         | IDType            |    [1]    | ID исходного электронного документа, статус которого требуется получить |

## <a name="1C-Bank_CancelationRequest"></a> XML-схема **запрос об отзыве электронного документа**: *1C-Bank_CancelationRequest.xsd*

[Скачать](http://) файл схемы 1C-Bank_CancelationRequest.xsd

## <a name="1C-Bank_Settings"></a> XML-схема **настроек обмена с банком**: *1C-Bank_Settings.xsd*

[Скачать](http://) файл схемы 1C-Bank_Settings.xsd

## <a name="1C-Bank_AuthSign"></a> XML-схема **данных для аутентификации по закр.ключу**: *1C-Bank_AuthSign.xsd*

[Скачать](http://) файл схемы 1C-Bank_AuthSign.xsd

## <a name="1C-Bank_Exch-Common"></a> XML-схема **описания общих типов**: *1C-Bank_Exch-Common.xsd*

[Скачать](http://) файл схемы 1C-Bank_Exch-Common.xsd

## <a name="1C-Bank_Library"></a> XML-схема **библиотеки**: *1C-Bank_Library.xsd*

[Скачать](http://) файл схемы 1C-Bank_Library.xsd


