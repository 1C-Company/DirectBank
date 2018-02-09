# Прикладной уровень взаимодействия
Данный уровень позволяет обмениваться любыми исходящими и входящими электронными документами, которые возникают в процесс взаимодействия Клиента, работающего в системе «1С:Предприятие 8», и Банка.

+ [Порядок обмена электронными документами](#1)
+ [Отправка платежного документа и изменение статусов](#4)
  + [Пример XML-файла платежного поручения](#4.1)
   + [Пример XML-файла платежного требования](#4.2)
+ [Получение выписки банка и изменение статусов](#5)
  + [Пример XML-файла запроса выписки](#5.1)
   + [Пример XML-файла выписки банка](#5.2)
+ [Получение состояния электронного документа](#6)
  + [Пример XML-файла извещения о состоянии электронного документа](#6.1)
+ [Запрос состояния электронного документа](#7)
  + [Пример XML-файла запроса состояния электронного документа](#7.1)
+ [Запрос об отзыве электронного документа](#8)
  + [Пример XML-файла запроса состояния электронного документа](#8.1)
+ [Описание формата документа Поручение на перевод валюты (ISO 20022).](#pain)
+ [Описание формата документа Выписки банка в валюте (ISO20022)](#camp)


## <a name="1"></a> Порядок обмена электронными документами.
Отправителем и получателем электронного документа могут быть как Клиент (Организация), работающий в системе «1С:Предприятие 8», так и Банк (роли участников обмена зависят от конкретной бизнес-операции). При этом инициатором обмена всегда выступает система «1С:Предприятие 8».

Общий принцип работы на прикладном уровне может быть представлен в виде схемы:

![](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/doc_imgs/Procedure_for_electronic_document_exchange.png)

### <a name="4"></a> Отправка платежного документа и изменение статусов.

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

#### <a name="4.1"></a> Пример XML-файла платежного поручения:

- [XML-файл **платежного поручения**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/PayDocRu.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<PayDocRu xmlns="http://directbank.1c.ru/XMLSchema" 
	xmlns:xs="http://www.w3.org/2001/XMLSchema" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    id="05688096-0806-4ef0-af4c-572f75dbaf7c" 
    formatVersion="2.1.2" 
    creationDate="2016-04-22T09:38:51" 
    userAgent="1С - БЭД: 1.3.10.10; БиблиотекаЭлектронныхДокументов: 1.3.10.10">
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

#### <a name="4.2"></a> Пример XML-файла платежного требования:

- [XML-файл **платежного требования**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/PayRequest.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<PayRequest xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    id="a4d4c2ac-26dd-43ae-a98b-cb4cebdbd1db" 
    formatVersion="2.1.2" 
    creationDate="2016-04-22T09:50:09" 
    userAgent="1С - БЭД: 1.3.10.10; БиблиотекаЭлектронныхДокументов: 1.3.10.10">
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

### <a name="5"></a> Получение выписки банка и изменение статусов.

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

#### <a name="5.1"></a> Пример XML-файла запроса выписки

- [XML-файл **запроса выписки**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/StatementRequest.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<StatementRequest xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    id="da06dc8f-afbe-4172-89c8-0d4492c2dd25"
    formatVersion="2.1.2"
    creationDate="2016-04-22T09:52:27"
    userAgent="1С - БЭД: 1.3.10.10; БиблиотекаЭлектронныхДокументов: 1.3.10.10">
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

#### <a name="5.2"></a> Пример XML-файла выписки банка

- [XML-файл **выписки банка**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/Statement.xml)

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Statement xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
	id="f7cbc6af-33dd-4c37-b67d-7400e1c327ad"
	formatVersion="2.1.2"
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

### <a name="6"></a> Получение состояния электронного документа
На любом из этапов работы с электронным документом на стороне Банка может возникнуть ошибка обработки, о которой надо проинформировать Клиента.
- Банковская система формирует электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), содержащий ошибку обработки.
- Если используется электронная подпись (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то электронный документ подписывается.
- Затем создает транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)) и ставит в очередь на передачу в «1С:Предприятие 8».
- Далее получение данных из Банка проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- После получения информации об ошибке обработки на стороне Банка, система «1С:Предприятие 8» назначит соответствующий статус электронному документу с пояснением причины отказа в обработке Банком.

#### <a name="6.1"></a> Пример XML-файла извещения о состоянии электронного документа

- [XML-файл **извещения о состоянии электронного документа**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/StatusDocNotice.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<StatusDocNotice xmlns="http://directbank.1c.ru/XMLSchema"
	id="204e36e4-6910-416d-99b2-e8109e518744"
	formatVersion="2.1.2"
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
### <a name="7"></a> Запрос состояния электронного документа
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

#### <a name="7.1"></a> Пример XML-файла запроса состояния электронного документа

- [XML-файл **запроса состояния электронного документа**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/StatusRequest.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<StatusRequest xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    id="0fa540c3-1aaa-4e06-8883-2c936fef05a5"
    formatVersion="2.1.2"
    creationDate="2016-04-22T10:09:11"
    userAgent="1С - БЭД: 1.3.10.10; БиблиотекаЭлектронныхДокументов: 1.3.10.10">
	<Sender id="id:42;s:9999" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001"/>
	<Recipient bic="044525888" name="ДЕМО-БАНК"/>
	<ExtID>05688096-0806-4ef0-af4c-572f75dbaf7c</ExtID>
</StatusRequest>
```
### <a name="8"></a> Запрос об отзыве электронного документа

На стороне Клиента может возникнуть потребность направить запрос об отзыве электронного документа, ранее переданного в Банк.
- По команде в «1С:Предприятии 8» формируется электронный документ «Запрос об отзыве электронного документа» (XML-файл, соответствующий [XML-схеме запроса об отзыве](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_CancelationRequest)), пользователь может указать причину отзыва документа.
- Если используется электронная подпись на стороне «1С:Предприятия 8» (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то система предложит пользователю подписать электронный документ.
- Электронный документ «Запрос об отзыве электронного документа» помещается в транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне).
- Далее передача данных в Банк проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- Если отправка прошла успешно, то система «1С:Предприятие 8» изменит статус электронного документа запрос на «Отправлен».
- После получения из Банка ответа по результатам обработки транспортного контейнера система «1С:Предприятие 8» назначит электронному документу запроса статус «Доставлен».
- Если по результатам контроля и первичной обработки электронного документа на стороне Банка выявляется ошибка, то формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), содержащий ошибку обработки запроса, и готовится к отправке. После получения ошибки обработки из Банка система «1С:Предприятие 8» назначит соответствующий статус запросу.
- Если запрос корректный, то на стороне Банка определяется, возможен ли отзыв исходного электронного документа и если возможен, то электронный документ аннулируется, если нет, то готовится описание ошибки выполнения запроса об отзыве. Затем формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий [XML-схеме извещения о состоянии электронного документа](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_StatusDocNotice)), содержащий 2 идентификатора (ИД исходного электронного документа и ИД запроса), а также, либо ошибку обработки запроса, либо тек. статус исходного электронного документа - «Аннулирован».
- Если используется электронная подпись (см. раздел [«Обеспечение безопасности данных»](https://github.com/1C-Company/DirectBank/blob/master/doc/common-section/data-security.md#-Обеспечение-безопасности-данных)), то электронный документ извещения подписывается.
- Банковская система формирует транспортный контейнер (XML-файл, соответствующий [XML-схеме транспортного контейнера](https://github.com/1C-Company/DirectBank/blob/master/doc/xsd-scheme/readme.md#1C-Bank_Packet)), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне) и ставит в очередь на передачу в «1С:Предприятие 8».
- Далее получение данных из Банка проходит согласно протоколу, описанному в разделе [«Порядок взаимодействия на транспортном уровне»](https://github.com/1C-Company/DirectBank/blob/master/doc/transport-api/readme.md#2).
- Текущий статус исходного электронного документа и статус запроса система «1С:Предприятие 8» назначит после успешного разбора входящего транспортного контейнера из Банка.

#### <a name="8.1"></a> Пример XML-файла запроса состояния электронного документа

- [XML-файл **запроса состояния электронного документа**](https://raw.githubusercontent.com/1C-Company/DirectBank/master/doc/application-layer/CancelationRequest.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CancelationRequest xmlns="http://directbank.1c.ru/XMLSchema"
	xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    id="e8caa148-82a3-446d-a10f-ff82ad514b7e"
    formatVersion="2.1.2"
    creationDate="2016-04-22T10:11:25"
    userAgent="1С - БЭД: 1.3.10.10; БиблиотекаЭлектронныхДокументов: 1.3.10.10">
    <Sender id="id:42;s:9999" name="Торговый дом Комплексный" inn="7705260699" kpp="770501001"/>
    <Recipient bic="044525888" name="ДЕМО-БАНК"/>
    <ExtID>05688096-0806-4ef0-af4c-572f75dbaf7c</ExtID>
    <Reason>Описание причины отзыва</Reason>
</CancelationRequest>
```

## <a name="pain"></a> Описание формата документа Поручение на перевод валюты (ISO 20022).

|Элемент | Описание элемента | Тип | Мн.|
|---------|-------------------|-----|----|
| CstmrCdtTrfInitn &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;|        | CustomerCreditTransferInitiationV03   | [1] |
| &emsp;	GrpHdr |   Секция: Реквизиты сообщения     | GroupHeader32   | [1] |
| &emsp;&emsp;	MsgId | Уникальный идентификатор сообщения.| Max35Text | [1] |
| &emsp;&emsp;	CreDtTm | Дата и время формирования сообщения. | ISODateTime | [1] |
| &emsp;&emsp;	NbOfTxs | Общее число поручений в сообщении. <br> Всегда содержит значение 1.| Max15NumericText | [1] |
| &emsp;&emsp;	InitgPty | Секция: Иницииатор(отправитель) сообщения. <br>Всегда пустое.| PartyIdentification32 | [1] |
| &emsp;	PmtInf | Секция: Реквизиты распоряжения по дебету (списания). <br> Всегда состоит из одного элемента. | PaymentInstructionInformation3 | [1..n] |
| &emsp;&emsp;	PmtInfId | Уникальный идентификатор распоряжения.<br> Такое же значение должна содержать операция в выписке в поле BkToCstmrStmt.Stmt.Ntry.NtryDtls.TxDtls.Refs.MsgId.  | Max35Text | [1] |
| &emsp;&emsp; PmtMtd | Всегда содержит значение "TRF"  | PaymentMethod3Code | [1] |
| &emsp;&emsp; ReqdExctnDt | Дата исполнения. | ISODate | [1] |
| &emsp;&emsp; Dbtr | Секция: Плательщик | PartyIdentification32 | [1] |
| &emsp;&emsp;&emsp; Nm | Наименование| Max140Text | [0..1] |
| &emsp;&emsp;&emsp; PstlAdr | Секция: Адрес | PostalAddress6 | [0..1] |
| &emsp;&emsp;&emsp;&emsp; PstCd | Индекс | Max16Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp; TwnNm | Город | Max35Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp; Ctry | Код страны | CountryCode | [0..1] |
| &emsp;&emsp;&emsp;&emsp; AdrLine | Адрес. <br> Может содержать только один элемент.| Max70Text | [0..n] |
| &emsp;&emsp;&emsp; Id | Секция: идентификация участника платежа | Party6Choice | [0..1] |
| &emsp;&emsp;&emsp;&emsp; OrgId |Секция: идентификация компании  | OrganisationIdentification4 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; Othr | Секция: иной вид идентификации <br>Может содержать только один элемент | GenericOrganisationIdentification1 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Id | ИНН  | Max35Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; SchmeNm | Секция: идентификация вида кода | OrganisationIdentificationSchemeName1Choice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Cd | Код типа данных в поле Id <br> Всегда содержит значение "TXID" | ExternalOrganisationIdentification1Code | [1] |
| &emsp;&emsp;&emsp; CtryOfRes | Код страны регистрации | CountryCode | [0..1] |
| &emsp;&emsp;&emsp; CtctDtls | Секция: контактная информация| ContactDetails2 | [0..1] |
| &emsp;&emsp;&emsp;&emsp; Nm | ФИО | Max140Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp; PhneNb | Телефон | PhoneNumber | [0..1] |
| &emsp;&emsp;&emsp;&emsp; EmailAdr | Email адрес | Max2048Text | [0..1] |
| &emsp;&emsp; DbtrAcct | Секция: Счет плательщика | CashAccount16 | [1] |
| &emsp;&emsp;&emsp; Id |Секция: идентификатор счета  | AccountIdentification4Choice | [1] |
| &emsp;&emsp;&emsp;&emsp; Othr | Секция: другой тип счета | GenericAccountIdentification1 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; Id | Cчет | Max34Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; SchmeNm | Секция: тип счета| AccountSchemeName1Choice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Cd | Вид счета. <br>Всегда содержит значение "BBAN" | ExternalAccountIdentification1Code | [1] |
| &emsp;&emsp; DbtrAgt | Секция: Банк плательщика | BranchAndFinancialInstitutionIdentification4 | [1] |
| &emsp;&emsp;&emsp; FinInstnId | Секция: идентификация банка | FinancialInstitutionIdentification7 | [1] |
| &emsp;&emsp;&emsp;&emsp; BIC | SWIFT | BICIdentifier | [0..1] |
| &emsp;&emsp;&emsp;&emsp; Nm | Наименование | Max140Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp; PstlAdr | Секция: Адрес | PostalAddress6 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; TwnNm | Город | Max35Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; AdrLine | Адрес. <br> Может содержать только один элемент.| Max70Text | [0..n] |
| &emsp;&emsp; ChrgBr | С кого списывается комиссия. <br> Возможные значения: "CRED", "DEBT", "SHAR" | ChargeBearerType1Code | [0..1] |
| &emsp;&emsp; ChrgsAcct | Секция: счет комиссии. | CashAccount16 | [0..1] |
| &emsp;&emsp;&emsp; Id | Секция: идентификатор счета | AccountIdentification4Choice | [1] |
| &emsp;&emsp;&emsp;&emsp; Othr | Секция: другой тип счета | GenericAccountIdentification1 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; Id | Счет | Max34Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; SchmeNm | | AccountSchemeName1Choice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Cd | Всегда содержит значение "BBAN" | ExternalAccountIdentification1Code | [1] |
| &emsp;&emsp; CdtTrfTxInf | Секция: Реквизиты по кредиту (зачисления). <br> Всегда содержит один элемент. | CreditTransferTransactionInformation10 | [0..n] |
| &emsp;&emsp;&emsp; PmtId | Секция: идентификаторы платежа | PaymentIdentification1 | [1]|
| &emsp;&emsp;&emsp;&emsp; InstrId | Уникальный номер поручения | Max35Text | [0..1]|
| &emsp;&emsp;&emsp;&emsp; EndToEndId | Номер платежного поручения | Max35Text | [1]|
| &emsp;&emsp;&emsp; PmtTpInf | Секция: тип платежа | PaymentTypeInformation19 | [0..1]|
| &emsp;&emsp;&emsp; Amt | Секция: сумма платежа | AmountType3Choice | [1]|
| &emsp;&emsp;&emsp;&emsp; InstdAmt | Сумма | ActiveOrHistoricCurrencyAndAmount | [1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; @Ccy | Валюта перевода | ActiveOrHistoricCurrencyCode | [1]|
| &emsp;&emsp;&emsp; XchgRateInf | Секция:курс конверсии | ExchangeRateInformation1 | [0..1]|
| &emsp;&emsp;&emsp;&emsp; XchgRate | Курс | BaseOneRate | [0..1]|
| &emsp;&emsp;&emsp;&emsp; RateTp |Тип курса. <br> Всегда имеет значение "SPOT" | ExchangeRateType1Code | [0..1]|
| &emsp;&emsp;&emsp; IntrmyAgt1 | Секция: Банк посредник| BranchAndFinancialInstitutionIdentification4 | [0..1]|
| &emsp;&emsp;&emsp;&emsp; FinInstnId | Секция: идентификация банка | FinancialInstitutionIdentification7 | [1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; BIC | SWIFT | BICIdentifier | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; Nm | Наименование банка | Max140Text | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; PstlAdr | Секция: Адрес | PostalAddress6 | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Ctry | Код страны | CountryCode | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; AdrLine | Адрес. <br> Может содержать только один элемент | Max70Text | [0..n]|
| &emsp;&emsp;&emsp; CdtrAgt | Секция: Банк получателя | BranchAndFinancialInstitutionIdentification4 | [0..1]|
| &emsp;&emsp;&emsp;&emsp; FinInstnId | Секция: идентификация банка | FinancialInstitutionIdentification7 | [1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; BIC | SWIFT | BICIdentifier | [1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; ClrSysMmbId | Секция: идентификация участника клиринга | ClearingSystemMemberIdentification2 | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; MmbId | БИК | Max35Text | [1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; Nm | Наименование | Max140Text | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; PstlAdr | Секция: Адрес | PostalAddress6 | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Ctry | Код страны | CountryCode | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; AdrLine | Адрес банка. <br> Может содержать только один элемент. | Max70Text | [0..n]|
| &emsp;&emsp;&emsp; CdtrAgtAcct | Счет банк посредника получателя | CashAccount16 | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; Id | Секция: идентификация счета | Max34Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Othr | Секция: не IBAN счет | Max34Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Id | Счет | Max34Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; SchmeNm | | AccountSchemeName1Choice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Cd | Всегда содержит значение "BBAN" | ExternalAccountIdentification1Code | [1] |
| &emsp;&emsp;&emsp; Cdtr | Секция: Получатель | PartyIdentification32 | [0..1]|
| &emsp;&emsp;&emsp;&emsp; Nm | Наименование | Max140Text | [0..1]|
| &emsp;&emsp;&emsp;&emsp; PstlAdr | Секция: адрес | PostalAddress6 | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; Ctry | Код страны | CountryCode | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; PstCd | Индекс | Max16Text | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; TwnNm | Город | Max35Text | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; AdrLine | Неструктурированный адрес. <br> Может содержать только один элемент. | Max70Text | [0..n]|
| &emsp;&emsp;&emsp;&emsp; Id | Идентификатор получателя  | Party6Choice | [0..1]|
| &emsp;&emsp;&emsp;&emsp;&emsp; OrgId |  | OrganisationIdentification4 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Othr | Может содержать только один элемент | GenericOrganisationIdentification1 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Id | ИНН получателя  | Max35Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; SchmeNm |  | OrganisationIdentificationSchemeName1Choice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Cd | Всегда содержит значение "TXID" | ExternalOrganisationIdentification1Code | [1] |
| &emsp;&emsp;&emsp;&emsp; CtryOfRes | Код страны получателя | CountryCode | [0..1] |
| &emsp;&emsp;&emsp;&emsp; CtctDtls | Контактная информация организации получателя | ContactDetails2 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; Nm | ФИО | Max140Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; PhneNb | Телефон | PhoneNumber | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; EmailAdr | Email адрес | Max2048Text | [0..1] |
| &emsp;&emsp;&emsp; CdtrAcct | Секция: Счет получателя | CashAccount16 | [1] |
| &emsp;&emsp;&emsp;&emsp; Id | Счет | AccountIdentification4Choice | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; Othr | Секция: другой тип счета | GenericAccountIdentification1 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Id | Номер счета | Max34Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; SchmeNm | | AccountSchemeName1Choice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Cd | Всегда содержит значение "BBAN" | ExternalAccountIdentification1Code | [1] |
| &emsp;&emsp;&emsp; RgltryRptg | Секция: информация для регулятора | RegulatoryReporting3 | [0..n] |
| &emsp;&emsp;&emsp;&emsp; Dtls |  | StructuredRegulatoryReporting3 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp; Tp | Всегда содержит значение "VO"  | Max35Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; Cd | Код вида операции | Max10Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; Amt | Сумма | ActiveOrHistoricCurrencyAndAmount | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; @Ccy | Код валюты  | ActiveOrHistoricCurrencyCode | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp; Inf | Номер паспорта сделки | Max35Text | [0..n] |
| &emsp;&emsp;&emsp; RmtInf | Секция: Информация о платеже | RemittanceInformation5 | [0..1] |
| &emsp;&emsp;&emsp;&emsp; Ustrd | Назначение платежа | Max140Text | [0..n] |
| &emsp;&emsp;&emsp;&emsp; Strd | Секция: Структурированная информация о платеже | StructuredRemittanceInformation7 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp; RfrdDocInf | Секция: Информация о связанных документах | ReferredDocumentInformation3 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Tp |  Секция: Тип связанного документа| ReferredDocumentType2 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; CdOrPrtry |  | ReferredDocumentType1Choice | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Prtry | Тип документа. <br> Всегда имеет значение "POD" | Max35Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; Nb | Номер документа	 | Max35Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; RltdDt | Дата документа	 | ISODate | [0..1] |

## <a name="camp"></a> Описание формата документа Выписка банка в валюте (ISO 20022).

|Элемент | Описание элемента | Тип | Мн.|
|---------|-------------------|-----|----|
| BkToCstmrStmt &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;|        | BankToCustomerStatementV02   | [1] |
| &emsp;	GrpHdr |   Секция: Заголовок     | GroupHeader42   | [1] |
| &emsp;&emsp;	MsgId | Системный идентификатор сообщения.| Max35Text | [1] |
| &emsp;&emsp;	CreDtTm | Дата и время создания сообщения системой. | ISODateTime | [1] |
| &emsp;&emsp;	AddtlInf | Идентификатор запроса выписки. | Max500Text | [0..1] |
| &emsp;	Stmt | Секция: Выписка по счету | AccountStatement2 | [1..n] |
| &emsp;&emsp;	Id | Системный идентификатор выписки. | Max35Text | [1] |
| &emsp;&emsp;	CreDtTm | Дата / время создания выписки. | ISODateTime | [1] |
| &emsp;&emsp;	FrToDt | Секция: Период выписки | DateTimePeriodDetails | [0..1] |
| &emsp;&emsp;&emsp;	FrDtTm | Дата начала периода | ISODateTime | [1] |
| &emsp;&emsp;&emsp;	ToDtTm | Дата конца периода | ISODateTime | [1] |
| &emsp;&emsp;	Acct | Секция: Реквизиты счета, владелеца, обслуживающего банка | CashAccount20 | [1] |
| &emsp;&emsp;&emsp;	Id | Секция: Идентификация организации | AccountIdentification4Choice | [1] |
| &emsp;&emsp;&emsp;&emsp;	IBAN | IBAN | IBAN2007Identifier | [1] |
| &emsp;&emsp;&emsp;&emsp;	Othr | Секция: другие идентификаторы| GenericAccountIdentification1 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;	Id | Номер счета | Max34Text | [1] |
| &emsp;&emsp;&emsp;	Nm | Наименование организации | Max70Text | [0..1] |
| &emsp;&emsp;&emsp;	Ownr | Секция: владелец счета | PartyIdentification32 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;	Nm | Наименование владельца счета| Max140Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;	Id | Секция: Идентификация владельца счета | Party6Choice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;	OrgId | Секция: Идентификация организации  | OrganisationIdentification4 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Othr | Секция: Другие реквизиты. <br>Должен содержать только один элемент | GenericOrganisationIdentification1 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Id | ИНН / КИО владельца счета | Max35Text | [1] |
| &emsp;&emsp;&emsp;	Svcr | Секция: Обслуживающий банк | BranchAndFinancialInstitutionIdentification4 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;	FinInstnId | | FinancialInstitutionIdentification7 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;	BIC | SWIFT | BICIdentifier | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;	ClrSysMmbId |  | ClearingSystemMemberIdentification2 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	MmbId | БИК банка/отделения обслуживающего счет | Max35Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;	Nm | Наименование банка/отделения обслуживающего счет | Max140Text | [0..1] |
| &emsp;&emsp;	Bal | Секция: Информация о балансах | CashBalance3 | [1..n] |
| &emsp;&emsp;&emsp;	Tp |  | BalanceType12 | [1] |
| &emsp;&emsp;&emsp;&emsp;	CdOrPrtry |  | BalanceType5Choice | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;	Cd | Тип баланса. <br> "OPBD" - начальный остаток <br> "CLBD" - конечный остаток| BalanceType12Code | [1] |
| &emsp;&emsp;&emsp;	Amt | Сумма остатка| ActiveOrHistoricCurrencyAndAmount | [1] |
| &emsp;&emsp;&emsp;&emsp;	@Ccy | Код валюты | ActiveOrHistoricCurrencyCode | [1] |
| &emsp;&emsp;	TxsSummry | Секция: Итоговая информация о транзакциях | TotalTransactions2 | [0..1] |
| &emsp;&emsp;&emsp;	TtlCdtNtries | Секция: Итоговая информация о транзакциях по кредиту | NumberAndSumOfTransactions1 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;	Sum | Обороты по кредиту | DecimalNumber | [0..1] |
| &emsp;&emsp;&emsp;	TtlDbtNtries | Секция: Итоговая информация о транзакциях по дебиту| NumberAndSumOfTransactions1 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;	Sum | Обороты по дебету | DecimalNumber | [0..1] |
| &emsp;&emsp;	Ntry | Секция: Информация о транзакции (если были) | ReportEntry2 | [0..n] |
| &emsp;&emsp;&emsp;	Amt | Сумма операции| ActiveOrHistoricCurrencyAndAmount | [1] |
| &emsp;&emsp;&emsp;&emsp;	@Ccy | Код валюты | ActiveOrHistoricCurrencyCode | [1] |
| &emsp;&emsp;&emsp;	CdtDbtInd | Индикатор Дебет ('DBIT') /Кредит ('CRDT') | CreditDebitCode | [1] |
| &emsp;&emsp;&emsp;	Sts | Статус операции Исполняется ('PDNG') / Исполнено ('BOOK') | EntryStatus2Code | [1] |
| &emsp;&emsp;&emsp;	BookgDt | Секция: дата отражения по выписке | DateAndDateTimeChoice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;	Dt | Дата | ISODate | [1] |
| &emsp;&emsp;&emsp;&emsp;	DtTm | Дата и время| ISODateTime | [1] |
| &emsp;&emsp;&emsp;	ValDt | Секция: Дата валютирования | DateAndDateTimeChoice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;	Dt | Дата | ISODate | [1] |
| &emsp;&emsp;&emsp;&emsp;	DtTm | Дата и время| ISODateTime | [1] |
| &emsp;&emsp;&emsp;	AcctSvcrRef| Идентификатор транзакции. | Max35Text | [0..1] |
| &emsp;&emsp;&emsp;	BkTxCd| Секция: Информация о типе транзакции | BankTransactionCodeStructure4 | [1] |
| &emsp;&emsp;&emsp;	NtryDtls| Секция: Детали строки выписки. <br> Должен содержать только 1 элемент| EntryDetails1 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;	TxDtls| Секция: Детали транзакции. <br> Должен содержать только 1 элемент| EntryTransaction2 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;	Refs|Секция: Референсы | TransactionReferences2 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	MsgId| Идентификатор исходного документа Поручение на перевод валюты| Max35Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	EndToEndId| Номер документа | Max35Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;	RltdPties| Секция: Реквизиты участников платежа | TransactionParty2 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Dbtr| Секция: Реквизиты плательщика | PartyIdentification32 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Nm| Наименование плательщика| Max140Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Id| Секция: Идентификация плательщика| Party6Choice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	OrgId|Секция: Идентификация организации | OrganisationIdentification4 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Othr| Секция: Другие реквизиты <br>Должен содержать только 1 элемент | GenericOrganisationIdentification1 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Id| ИНН / КИО плательщика | Max35Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	CtryOfRes| Код страны| CountryCode | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	DbtrAcct| Секция: счет плательщика | CashAccount16 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Id| | AccountIdentification4Choice | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Othr| Секция: Другие реквизиты| GenericAccountIdentification1 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Id| Номер счета плательщика| Max34Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Cdtr| Секция: Реквизиты получателя | PartyIdentification32 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Nm| Наименование получателя| Max140Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Id| Секция: Идентификация получателя | Party6Choice | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	OrgId| Секция: Идентификация организации| OrganisationIdentification4 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Othr| Секция: Другие реквизиты. <br>Должен содержать только 1 элемент | GenericOrganisationIdentification1 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Id| ИНН / КИО получателя | Max35Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	CtryOfRes| Код страны| CountryCode | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	CdtrAcct| Секция: Cчет получателя | CashAccount16 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Id| | AccountIdentification4Choice | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Othr| | GenericAccountIdentification1 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Id| Номер счета получателя| Max34Text | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;	RltdAgts| Секция: Реквизиты банков | TransactionAgents2 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	DbtrAgt| Секция: Реквизиты банка плательщика  | BranchAndFinancialInstitutionIdentification4 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	FinInstnId|  | FinancialInstitutionIdentification7 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	BIC| SWIFT банка плательшика | BICIdentifier | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Nm| Наименование банка плательщика| Max140Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	PstlAdr| Адрес | PostalAddress6 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Ctry| Код страны | CountryCode | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	CdtrAgt| Секция: Реквизиты банка получателя | BranchAndFinancialInstitutionIdentification4 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	FinInstnId|  | FinancialInstitutionIdentification7 | [1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	BIC| SWIFT банка получателя| BICIdentifier | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Nm| Наименование банка получателя | Max140Text | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	PstlAdr| Адрес | PostalAddress6 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Ctry| Код страны | CountryCode | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;	RmtInf| Секция: Детали документа  | RemittanceInformation5 | [0..1] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Ustrd| Назначение платежа, может быть более одного поля | Max140Text | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	Strd| Секция: Структурные детали  | StructuredRemittanceInformation7 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	RfrdDocInf|  | ReferredDocumentInformation3 | [0..n] |
| &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;	RltdDt| Дата документа | ISODate | [0..1] |

