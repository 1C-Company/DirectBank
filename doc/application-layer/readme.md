# ВНИМАНИЕ! Это не финальная версия, раздел может быть дополнен.

# Прикладной уровень взаимодействия
Данный уровень позволяет обмениваться любыми исходящими и входящими электронными документами, которые возникают в процесс взаимодействия Клиента, работающего в системе «1С:Предприятие 8», и Банка.

+ [Настройка обмена электронными документами](#1)
 + [Получение настроек обмена с банком в автоматическом режиме](#1.1)
 + [Загрузка настроек обмена с банком из файла](#1.2)
 + [Пример XML-файла настроек обмена с банком](#1.3)
+ [Порядок обмена электронными документами](#2)
 + [Проверка работоспособности обмена электронными документами](#2.1)
   + [Пример XML-файла запроса-зонда](#2.1.1)
 + [Отправка платежного документа и изменение статусов](#2.2)
   + [Пример XML-файла платежного поручения](#2.2.1)
    + [Пример XML-файла платежного требования](#2.2.2)
 + [Получение выписки банка и изменение статусов](#2.3)
   + [Пример XML-файла запроса выписки](#2.3.1)
   + [Пример XML-файла выписки банка](#2.3.2)
 + [Получение состояния электронного документа](#2.4)
   + [Пример XML-файла извещения о состоянии электронного документа](#2.4.1)
 + [Запрос состояния электронного документа](#2.5)
   + [Пример XML-файла запроса состояния электронного документа](#2.5.1)
 + [Запрос об отзыве электронного документа](#2.6)
   + [Пример XML-файла запроса состояния электронного документа](#2.6.1)
+ [Обеспечение безопасности данных](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)
 + [Шифрование данных при передаче](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Шифрование-данных-при-передаче)
 + [Криптооперации, выполняемые на стороне Клиента и Банка](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Криптооперации-выполняемые-на-стороне-Клиента-и-Банка)
 + [Защита электронных документов с помощью электронной подписи](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Защита-электронных-документов-с-помощью-электронной-подписи)
+ [Схемы данных](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md)
+ [Таблицы кодов](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md)
 + [Таблица кодов статусов транспортных контейнеров](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md#-Таблица-кодов-статусов-транспортных-контейнеров)
 + [Таблица кодов видов электронных документов](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md#-Таблица-кодов-видов-электронных-документов)
 + [Таблица кодов статусов электронных документов](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md#-Таблица-кодов-статусов-электронных-документов)
 + [Таблица типов выписок банка](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md#-Таблица-типов-выписок-банка)
 + [Таблица кодов ошибок и их описание, которые может возвращать банковский сервис в «1С:Предприятие 8»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/tables.md#-Таблица-кодов-ошибок-и-их-описание-которые-может-возвращать-банковский-сервис-в-1СПредприятие-8)
+ [Таблицы типов](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md)

## <a name="1"></a> Настройка обмена электронными документами.
Для начала использования прямого обмена электронными документами из решений  «1С:Предприятие 8» с банковской системой Клиент должен получить параметры обмена данными из Банка – в «1С:Предприятии 8» будут автоматически созданы настройки ЭДО с Банком.

Порядок действий Клиента по орг.части и способ получения настроек обмена определяет сам Банк, например, надо ли в личном кабинете Клиента включать опцию использования нового сервиса и сохранять файл настроек на диск или же заключить доп.соглашение с Банком по обслуживанию через новый сервис и получить настройки в автомат.режиме.

### <a name="1.1"></a>  Получение настроек обмена с банком в автоматическом режиме.

При запросе настроек обмена в автоматическом режиме система «1С:Предприятие 8» передает в Банк номер расчетного счета Клиента (отправка производится HTTP-методом POST - метод [GetSettings](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2.2)). Банковский сервис по номеру расчетному счета клиента формирует файл настроек (XML-файл, соответствующий [XML-схеме настроек обмена с банком](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Settings)) и в синхронном режиме возвращается в «1С:Предприятие 8» (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)).

![](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/Setting up a direct exchange.png)

Результатом неуспешного запроса настроек обмена будет ошибка, возвращаемая банковской системой также в синхронном режиме (XML-файл, соответствующий [XML-схеме ответа банк.сервиса](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_ResultBank)).

Особенностью данного процесса является то, что на момент первого запроса настроек обмена с банком в системе «1С:Предприятие 8» неизвестен уникальный идентификатор Клиента в банковской системе. В этом случае следует использовать «0» в качестве значения этого реквизита.

### <a name="1.2"></a> Загрузка настроек обмена с банком из файла.
Файл настроек обмена (XML-файл, соответствующий [XML-схеме настроек обмена с банком](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Settings)) предварительно получается Клиентом из банковской системы по каналам связи, которые определяет сам Банк. После получения файла настроек Клиент указывает его в помощнике создания настроек и система «1С:Предприятие 8» создает настройку согласно данным файла.

![](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/Load settings from a file.png)

### <a name="1.3"></a>  Пример XML-файла настроек обмена с банком:

- [XML-файл **настроек обмена с банком**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/Settings.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Settings xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
	id="EFD857B5-7FA8-4195-8666-2CCADBC3C8DE"
    formatVersion="2.1.1"
	creationDate="2016-04-22T09:38:51" 
    userAgent="DemoBankService">
	<Sender bic="044525888" name="ДЕМО-БАНК" />
	<Recipient id="2806" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001" />
	<Data>
		<CustomerID>2806</CustomerID>
		<BankServerAddress>https://dbogate.demobank.ru/</BankServerAddress>
		<FormatVersion>2.1.1</FormatVersion>
		<Encoding>UTF-8</Encoding>
		<Logon>
			<Login>
				<User>user_login</User>
			</Login>
		</Logon>
		<Document docKind="03" />
		<Document docKind="05" />
		<Document docKind="06" />
		<Document docKind="10" />
		<Document docKind="14" />
	</Data>
</Settings>
```

- - -

## <a name="2"></a> Порядок обмена электронными документами.
Отправителем и получателем электронного документа могут быть как Клиент (Организация), работающий на системе «1С:Предприятие 8», так и Банк (роли участников обмена зависят от конкретной бизнес-операции). При этом инициатором обмена всегда выступает система «1С:Предприятие 8».

Общий принцип работы на прикладном уровне может быть представлен в виде схемы:

![](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/Procedure for electronic document exchange.png)

### <a name="2.1"></a> Проверка работоспособности обмена электронными документами.
- На этапе отладки или профилактических работ часто возникает потребность проверить доступность банковского сервиса, не приступая к штатному обмену электронными документами. Для таких целей предназначен специальный вид электронного документа.
- По команде в «1С:Предприятии 8» формируется электронный документ «Запрос-зонд» (XML-файл, соответствующий [XML-схеме запроса-зонда](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Probe)).
- Если используется электронная подпись на стороне «1С:Предприятия 8» (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то система предложит пользователю подписать электронный документ.
- Электронный документ «Запрос-зонд» с электронной подписью (если используется, см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)) помещаются в транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне).
- Далее передача данных в Банк проходит согласно протоколу, описанному в разделе «[Порядок взаимодействия на транспортном уровне](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2)».
- Если отправка прошла успешно, то система «1С:Предприятие 8» изменит статус электронного документа запрос на «Отправлен».
- После получения из Банка ответа по результатам обработки транспортного контейнера система «1С:Предприятие 8» назначит электронному документу запроса статус «Доставлен».
- По результатам контроля и обработки запроса-зонда на стороне Банка всегда формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), который содержит:
 - либо ошибку обработки запроса ([Error](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#ErrorType)) с соответствующим описанием исключительной ситуации;
 - либо ([Status](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/type-tables.md#StatusType)) код «01», означающий что проверка успешно завершена.
- Если используется электронная подпись (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то электронный документ извещения подписывается.
- Банковская система формирует транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне) и ставит в очередь на передачу в «1С:Предприятие 8».
- Далее получение данных из Банка проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- Статус запроса-зонда система «1С:Предприятие 8» сообщит пользователю после успешного разбора входящего транспортного контейнера из Банка.

#### <a name="2.1.1"></a> Пример XML-файла запроса-зонда

- [XML-файл **запроса-зонда**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/Probe.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Probe xmlns="http://directbank.1c.ru/XMLSchema" 
	xmlns:xs="http://www.w3.org/2001/XMLSchema" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	id="eafa28b5-6600-424c-b0d3-3785274d570d" 
	formatVersion="2.1.1" 
	creationDate="2016-04-22T09:33:57" 
	userAgent="1С - БЭД: 1.3.5.10; БиблиотекаЭлектронныхДокументов: 1.3.5.10">
	<Sender id="id:42;s:9999" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001"/>
	<Recipient bic="044525888" name="ДЕМО-БАНК "/>
</Probe>
```

### <a name="2.2"></a> Отправка платежного документа и изменение статусов.

- В «1С:Предприятии 8» формируется электронный документ «Платежное поручение» (XML-файл с бизнес данными, соответствующий [XML-схеме платежного поручения](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_PayDocRu)) или «Платежное требование» (XML-файл с бизнес данными, соответствующий [XML-схеме платежного требования](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_PayRequest)).
- Если используется электронная подпись на стороне «1С:Предприятия 8» (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то система предложит пользователю подписать электронный документ.
- Электронный документ с электронной подписью (если используется, см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)) помещаются в транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне).
- Далее передача данных в Банк проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- При этом происходит изменение статусов:
 - Если отправка прошла успешно, то система «1С:Предприятие 8» изменит статус электронному документу на «Отправлен».
 - После получения из Банка ответа по результатам обработки транспортного контейнера система «1С:Предприятие 8» назначит электронному документу статус «Доставлен».
 - По результатам контроля и первичной обработки электронного документа на стороне Банка формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusPacketNotice)), содержащий либо ошибку обработки, либо текущий статус электронного документа, и готовится к отправке.
 - После получения информации о статусе электронного документа или ошибки обработки на стороне Банка система «1С:Предприятие 8» назначит соответствующий статус электронному документу.
 - Статус «Подтвержден» электронному документу «1С:Предприятие 8» назначит только после получения отметки об исполнении на стороне Банка в электронном документ «Выписка банка».

#### <a name="2.2.1"></a> Пример XML-файла платежного поручения:

- [XML-файл **платежного поручения**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/PayDocRu.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<PayDocRu xmlns="http://directbank.1c.ru/XMLSchema" 
	xmlns:xs="http://www.w3.org/2001/XMLSchema" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    id="05688096-0806-4ef0-af4c-572f75dbaf7c" 
    formatVersion="2.1.1" 
    creationDate="2016-04-22T09:38:51" 
    userAgent="1С - БЭД: 1.3.5.10; БиблиотекаЭлектронныхДокументов: 1.3.5.10">
    <Sender id="id:42;s:9999" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001"/>
    <Recipient bic="044525888" name="ДЕМО-БАНК"/>
	<Data>
        <DocNo>14</DocNo>
        <DocDate>2016-04-22</DocDate>
        <Sum>15</Sum>
        <Payer>
            <Name>Торговый дом "Комплексный"</Name>
            <INN>7705260699</INN>
            <KPP>770501001</KPP>
            <Account>40702810813123123222</Account>
            <Bank>
                <BIC>044525888</BIC>
                <Name>ДЕМО-БАНК</Name>
                <CorrespAcc>30101810500000000219</CorrespAcc>
            </Bank>
        </Payer>
        <Payee>
            <Name>ООО "Канцтовары"</Name>
            <INN>7704596181</INN>
            <KPP>770401001</KPP>
            <Account>40702810401200000035</Account>
            <Bank>
                <BIC>044525999</BIC>
                <Name>ДЕМО-БАНК2</Name>
                <CorrespAcc>30101810200000000593</CorrespAcc>
            </Bank>
        </Payee>
        <TransitionKind>01</TransitionKind>
        <Priority>3</Priority>
        <Purpose>за товар</Purpose>
	</Data>
</PayDocRu>
```

#### <a name="2.2.2"></a> Пример XML-файла платежного требования:

- [XML-файл **платежного требования**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/PayRequest.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<PayRequest xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    id="a4d4c2ac-26dd-43ae-a98b-cb4cebdbd1db" 
    formatVersion="2.1.1" 
    creationDate="2016-04-22T09:50:09" 
    userAgent="1С - БЭД: 1.3.5.10; БиблиотекаЭлектронныхДокументов: 1.3.5.10">
	<Sender id="id:42;s:9999" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001"/>
	<Recipient bic="044525888" name="ДЕМО-БАНК"/>
	<Data>
		<DocNo>15</DocNo>
		<DocDate>2016-04-22</DocDate>
		<Sum>15</Sum>
		<Payer>
			<Name>ООО "Канцтовары"</Name>
			<INN>7704596181</INN>
			<KPP>770401001</KPP>
			<Account>40702810401200000035</Account>
			<Bank>
				<BIC>044525999</BIC>
				<Name>ДЕМО-БАНК2</Name>
				<City>Г. МОСКВА</City>
				<CorrespAcc>30101810200000000593</CorrespAcc>
			</Bank>
		</Payer>
		<Payee>
			<Name>Торговый дом "Комплексный"</Name>
			<INN>7705260699</INN>
			<KPP>770501001</KPP>
			<Account>40702810813123123222</Account>
			<Bank>
				<BIC>044525888</BIC>
				<Name>ДЕМО-БАНК</Name>
				<City>Г. МОСКВА</City>
				<CorrespAcc>30101810500000000219</CorrespAcc>
			</Bank>
		</Payee>
		<PaymentKind>Электронно</PaymentKind>
		<TransitionKind>02</TransitionKind>
		<Priority>3</Priority>
		<Purpose>за товар</Purpose>
		<PaymentCondition>1</PaymentCondition>
	</Data>
</PayRequest>
```

### <a name="2.3"></a> Получение выписки банка и изменение статусов.

- В «1С:Предприятии 8» формируется электронный документ «Запрос выписки банка» (XML-файл, соответствующий [XML-схеме запроса на выписку](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatementRequest)).
- Если используется электронная подпись на стороне «1С:Предприятия 8» (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то система предложит пользователю подписать электронный документ.
- Электронный документ «Запрос выписки банка» и электронная подпись (если используется) помещаются в транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне).
- Далее передача данных в Банк проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- При этом происходит изменение статусов:
 - Если отправка прошла успешно, то система «1С:Предприятие 8» изменит статус электронному документу на «Отправлен».
 - После получения из Банка ответа по результатам обработки транспортного контейнера система «1С:Предприятие 8» назначит электронному документу статус «Доставлен».
 - По результатам контроля и первичной обработки электронного документа на стороне Банка формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), содержащий либо ошибку обработки, либо текущий статус электронного документа, и готовится к отправке. После получения информации о статусе электронного документа или ошибки обработки на стороне Банка система «1С:Предприятие 8» назначит соответствующий статус электронному документу.
- По мере готовности электронного документа «Выписка банка» (XML-файл, соответствующий [XML-схеме выписка банка](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Statement)) и формированию электронной подписи Банка (если используется) банковская система формирует транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне) и ставит в очередь на передачу в «1С:Предприятие 8».
- Далее получение данных из Банка проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- Статус, что электронный документ «Выписка банка» получен, система «1С:Предприятие 8» назначит после успешного разбора входящего транспортного контейнера из Банка.

#### <a name="2.3.1"></a> Пример XML-файла запроса выписки

- [XML-файл **запроса выписки**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/StatementRequest.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<StatementRequest xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    id="da06dc8f-afbe-4172-89c8-0d4492c2dd25"
    formatVersion="2.1.1"
    creationDate="2016-04-22T09:52:27"
    userAgent="1С - БЭД: 1.3.5.10; БиблиотекаЭлектронныхДокументов: 1.3.5.10">
	<Sender id="id:42;s:9999" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001"/>
	<Recipient bic="044525888" name="ДЕМО-БАНК"/>
	<Data>
		<StatementType>0</StatementType>
		<DateFrom>2016-04-21T00:00:00</DateFrom>
		<DateTo>2016-04-21T23:59:59</DateTo>
		<Account>40702810500000000001</Account>
		<Bank>
			<BIC>044525888</BIC>
			<Name>ДЕМО-БАНК</Name>
		</Bank>
	</Data>
</StatementRequest>
```

#### <a name="2.3.2"></a> Пример XML-файла выписки банка

- [XML-файл **выписки банка**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/Statement.xml)

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Statement xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
	id="f7cbc6af-33dd-4c37-b67d-7400e1c327ad"
	formatVersion="2.1.1"
	creationDate="2016-05-13T13:51:21.829"
	userAgent="DemoBankService">
	<Sender name="ДЕМО-БАНК" bic="044525888"/>
	<Recipient id="c8d57b00-530d-4a2e-86c1-aa9d5e4fafbd" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001"/>
	<Data>
		<StatementType>0</StatementType>
		<DateFrom>2016-05-04T00:00:00.000</DateFrom>
		<DateTo>2016-05-13T11:51:13.000</DateTo>
		<Account>40702810500000000001</Account>
		<Bank>
			<BIC>044525888</BIC>
			<Name>ДЕМО-БАНК"</Name>
			<City>МОСКВА</City>
			<CorrespAcc>30101810500000000219</CorrespAcc>
		</Bank>
		<OpeningBalance>139280.91</OpeningBalance>
		<ClosingBalance>88970.02</ClosingBalance>
		<OperationInfo>
			<PayDoc id="768" docKind="10">
				<PayDocRu>
					<DocNo>768</DocNo>
					<DocDate>2016-05-04</DocDate>
					<Sum>14</Sum>
					<Payer>
						<Name>Торговый дом Комплексный</Name>
						<INN>7705260699</INN>
						<KPP>770501001</KPP>
						<Account>40702810500000000001</Account>
						<Bank>
							<BIC>044525888</BIC>
							<Name>ДЕМО-БАНК</Name>
							<City>МОСКВА</City>
							<CorrespAcc>30101810500000000219</CorrespAcc>
						</Bank>
					</Payer>
					<Payee>
						<Name>Индивидуальный предприниматель Иванов Иван Иванович</Name>
						<INN>0</INN>
						<KPP>0</KPP>
						<Account>40802810300020007955</Account>
						<Bank>
							<BIC>046577413</BIC>
							<Name>ФИЛИАЛ ДЕМО-БАНК БТВ</Name>
							<City>ЕКАТЕРИНБУРГ</City>
							<CorrespAcc>30101810965770000413</CorrespAcc>
						</Bank>
					</Payee>
					<Priority>5</Priority>
					<Purpose>За транспортные услуги по счету № Ек2Тюм003221 от 18 Апреля 2016г. Сумма 2 999-00руб без НДС</Purpose>
				</PayDocRu>
			</PayDoc>
			<DC>1</DC>
			<Date>2016-05-04</Date>
			<Stamp>
				<BIC>044525888</BIC>
				<Name>ДЕМО-БАНК</Name>
				<City>МОСКВА</City>
				<CorrespAcc>30101810500000000219</CorrespAcc>
				<Status>
					<Code>02</Code>
					<Name>Исполнен</Name>
					<MoreInfo>Платежный документ исполнен банком</MoreInfo>
				</Status>
			</Stamp>
		</OperationInfo>
		<Stamp>
			<BIC>044525888</BIC>
			<Name>ДЕМО-БАНК</Name>
			<City>МОСКВА</City>
			<CorrespAcc>30101810500000000219</CorrespAcc>
		</Stamp>
	</Data>
	<ExtIDStatementRequest>39f9553d-67b1-4314-a2b1-8bddc99e0f42</ExtIDStatementRequest>
</Statement>
```

### <a name="2.4"></a> Получение состояния электронного документа
На любом из этапов работы с электронным документом на стороне Банка может возникнуть ошибка обработки, о которой надо проинформировать Клиента.
- Банковская система формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), содержащий ошибку обработки.
- Если используется электронная подпись (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то электронный документ подписывается.
- Затем создает транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)) и ставит в очередь на передачу в «1С:Предприятие 8».
- Далее получение данных из Банка проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- После получения информации об ошибки обработки на стороне Банка система «1С:Предприятие 8» назначит соответствующий статус электронному документу с пояснением причины отказа в обработке Банком.

#### <a name="2.4.1"></a> Пример XML-файла извещения о состоянии электронного документа

- [XML-файл **извещения о состоянии электронного документа**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/StatusDocNotice.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<StatusDocNotice xmlns="http://directbank.1c.ru/XMLSchema"
	id="204e36e4-6910-416d-99b2-e8109e518744"
	formatVersion="2.1.1"
    creationDate="2016-04-22T09:34:46+03:00"
    userAgent=" DemoBankService">
	<Sender>
		<Bank bic="044525888"/>
	</Sender>
	<Recipient>
		<Customer id="id:42;s:9999"/>
	</Recipient>
	<ExtID>eafa28b5-6600-424c-b0d3-3785274d570d</ExtID>
    <Result>
        <Status>
            <Code>01</Code>
            <Name>Подписан</Name>
        </Status>
    </Result>
</StatusDocNotice>
```
### <a name="2.5"></a> Запрос состояния электронного документа
На любом из этапов работы с электронным документом на стороне Клиента может возникнуть потребность узнать актуальное состояние электронного документа в Банке.
- По команде в «1С:Предприятии 8» формируется электронный документ «Запрос о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме запроса о состоянии](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusRequest)).
- Если используется электронная подпись на стороне «1С:Предприятия 8» (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то система предложит пользователю подписать электронный документ.
- Электронный документ с электронной подписью (если используется, см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)) помещаются в транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне).
- Далее передача данных в Банк проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- Если отправка прошла успешно, то система «1С:Предприятие 8» изменит статус электронного документа запрос на «Отправлен».
- После получения из Банка ответа по результатам обработки транспортного контейнера система «1С:Предприятие 8» назначит электронному документу запроса статус «Доставлен».
- Если по результатам контроля и первичной обработки электронного документа на стороне Банка выявляется ошибка, то формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), содержащий ошибку обработки запроса, и готовится к отправке. После получения ошибки обработки из Банка система «1С:Предприятие 8» назначит соответствующий статус запросу.
- Если запрос корректный, то на стороне Банка выполняется запрос на получение актуального статуса исходного электронного документа, затем формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), содержащий 2 идентификатора (ИД исходного электронного документа и ИД запроса), а также, либо ошибку обработки исходного электронного документа, либо его текущий статус.
- Если используется электронная подпись (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то электронный документ извещения подписывается.
- Банковская система формирует транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне) и ставит в очередь на передачу в «1С:Предприятие 8».
- Далее получение данных из Банка проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- Текущий статус исходного электронного документа и статус запроса система «1С:Предприятие 8» назначит после успешного разбора входящего транспортного контейнера из Банка.

#### <a name="2.5.1"></a> Пример XML-файла запроса состояния электронного документа

- [XML-файл **запроса состояния электронного документа**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/StatusRequest.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<StatusRequest xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    id="0fa540c3-1aaa-4e06-8883-2c936fef05a5"
    formatVersion="2.1.1"
    creationDate="2016-04-22T10:09:11"
    userAgent="1С - БЭД: 1.3.5.10; БиблиотекаЭлектронныхДокументов: 1.3.5.10">
	<Sender id="id:42;s:9999" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001"/>
	<Recipient bic="044525888" name="ДЕМО-БАНК"/>
	<ExtID>05688096-0806-4ef0-af4c-572f75dbaf7c</ExtID>
</StatusRequest>
```
### <a name="2.6"></a> Запрос об отзыве электронного документа

На стороне Клиента может возникнуть потребность направить запрос об отзыве электронного документа, ранее переданного в Банк.
- По команде в «1С:Предприятии 8» формируется электронный документ «Запрос об отзыве электронного документа» (XML-файл, соответствующий [XML-схеме запроса об отзыве](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_CancelationRequest)), пользователь может указать причину отзыва документа.
- Если используется электронная подпись на стороне «1С:Предприятия 8» (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то система предложит пользователю подписать электронный документ.
- Электронный документ «Запрос об отзыве электронного документа» помещается в транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне).
- Далее передача данных в Банк проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- Если отправка прошла успешно, то система «1С:Предприятие 8» изменит статус электронного документа запрос на «Отправлен».
- После получения из Банка ответа по результатам обработки транспортного контейнера система «1С:Предприятие 8» назначит электронному документу запроса статус «Доставлен».
- Если по результатам контроля и первичной обработки электронного документа на стороне Банка выявляется ошибка, то формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), содержащий ошибку обработки запроса, и готовится к отправке. После получения ошибки обработки из Банка система «1С:Предприятие 8» назначит соответствующий статус запросу.
- Если запрос корректный, то на стороне Банка определяется, возможен ли отзыв исходного электронного документа и если возможен, то электронный документ аннулируется, если нет, то готовиться описание ошибки выполнения запроса об отзыве. Затем формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), содержащий 2 идентификатора (ИД исходного электронного документа и ИД запроса), а также, либо ошибку обработки запроса, либо тек. статус исходного электронного документа - «Аннулирован».
- Если используется электронная подпись (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то электронный документ извещения подписывается.
- Банковская система формирует транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне) и ставит в очередь на передачу в «1С:Предприятие 8».
- Далее получение данных из Банка проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- Текущий статус исходного электронного документа и статус запроса система «1С:Предприятие 8» назначит после успешного разбора входящего транспортного контейнера из Банка.

#### <a name="2.6.1"></a> Пример XML-файла запроса состояния электронного документа

- [XML-файл **запроса состояния электронного документа**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/CancelationRequest.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CancelationRequest xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    id="e8caa148-82a3-446d-a10f-ff82ad514b7e"
    formatVersion="2.1.1"
    creationDate="2016-04-22T10:11:25"
    userAgent="1С - БЭД: 1.3.5.10; БиблиотекаЭлектронныхДокументов: 1.3.5.10">
    <Sender id="id:42;s:9999" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001"/>
    <Recipient bic="044525888" name="ДЕМО-БАНК"/>
    <ExtID>05688096-0806-4ef0-af4c-572f75dbaf7c</ExtID>
    <Reason>Описание причины отзыва</Reason>
</CancelationRequest>
```