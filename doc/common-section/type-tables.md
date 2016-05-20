#Приложение. Таблицы типов

+ @[Таблица типов **edo**](#1)
+ [Таблица **прикладных** типов](#2)
+ [Таблица типов **W3C**](#3)
+ [Таблицы **общих комплексных типов edo**](#4)
 + [Тип edo:ResultBank](#4.1)
 + [Тип edo:ErrorType](#4.2)
 + [Тип edo:SuccessResultType](#4.3)
 + [Тип edo:SendPacketResponseType](#4.4)
 + [Тип edo:GetPacketListResponseType](#4.5)
 + [Тип edo:LogonResponseType](#4.6)
 + [Тип edo:LogonCertResponseType](#4.7)
 + [Тип edo:GetSettingsResponse](#4.8)
 + [Тип edo:Packet](#4.9)
 + [Тип edo:ParticipantType](#4.10)
 + [Тип edo:BankPartyType](#4.11)
 + [Тип edo:CustomerPartyType](#4.12)
 + [Тип edo:DocumentType](#4.13)
 + [Тип edo:DigestType](#4.14)
 + [Тип edo:ResultStatusType](#4.15)
 + [Тип edo:StatusType](#4.16)
 + [Тип edo:PayDocRuApp](#4.17)
 + [Тип edo:PayRequestApp](#4.18)
 + [Тип edo:PaymentDataType](#4.19)
 + [Тип edo:CustomerDetailsType](#4.20)
 + [Тип edo:BankType](#4.21)


## <a name="1"></a> Таблица типов edo
(http://directbank.1c.ru/XMLSchema)

| Наименование              | Тип                           | Описание                                                                                                       |
|---------------------------|-------------------------------|----------------------------------------------------------------------------------------------------------------|
| IDType                    | string                        | Уникальный идентификатор                                                                                       |
| FormatVersionType         | string (до 12)                | Версия формата                                                                                                 |
| UserAgentType             | string (до 100)               | Версия ПО                                                                                                      |
| ResultBank                | edo:ResultBank                | Ответ банка                                                                                                    |
| ErrorType                 | edo:ErrorType                 | Ответ в случае возникновения ошибки                                                                            |
| SuccessResultType         | edo:SuccessResultType         | Успешный ответ банка                                                                                           |
| SendPacketResponseType    | edo:SendPacketResponseType    | Отправка пакета в банк                                                                                         |
| Packet                    | edo:Packet                    | Пакет электронных документов                                                                                   |
| GetPacketListResponseType | edo:GetPacketListResponseType | Список ID пакетов, готовых к передачи клиенту                                                                  |
| GetPacketResponse         | edo:Packet                    | Пакет электронных документов для получения клиентом                                                            |
| LogonResponseType         | edo:LogonResponseType         | Аутентификация по логину + ОТР (опционально)                                                                   |
| LogonCertResponseType     | edo:LogonCertResponseType     | Аутентификация по сертификату                                                                                  |
| GetSettingsResponseType   | edo:GetSettingsResponseType   | Получение настроек обмена в автоматическом режиме                                                              |
| ParticipantType           | edo:ParticipantType           | Одна из сторон, принимающая участие в обмене электронными документами (Участник)                               |
| BankPartyType             | edo:BankPartyType             | Отправитель                                                                                                    |
| CustomerPartyType         | edo:CustomerPartyType         | Получатель                                                                                                     |
| IDCustomerType            | string (от 1 до 50)           | Уникальный идентификатор клиента в банке                                                                       |
| DocumentType              | edo:DocumentType              | Данные электронного документа                                                                                  |
| DocKindType               | string (2)                    | Вид электронного документа, как он задан в описании к стандарту                                                |
| ContentType               | string                        | Тип контента передаваемого файла. Доступные значения:application/xmlapplication/octet-streamtext/plaintext/xml |
| AccNumType                | string (20)                   | Номер счета (расчетного, корреспондентского).  Макет: [0-9]{20}                                                |