---


---

<h1 id="api-прямого-обмена-данными-с-банком">API прямого обмена данными с банком</h1>
<p>API обмена данными – уровень, который описывает подходы и методы осуществления операций обмена данными, он в свою очередь делится на следующие уровни:</p>
<ul>
<li>
<p>Уровень аутентификации</p>
</li>
<li>
<p>Настройка обмена</p>
</li>
<li>
<p>Транспортный уровень</p>
</li>
</ul>
<hr>
<ul>
<li>
<p><a href="#authentication">Порядок взаимодействия на уровне аутентификации</a></p>
<ul>
<li>
<p><a href="#baseAuthentication">Аутентификация по логину и паролю</a></p>
<ul>
<li>
<p><a href="#logon">Метод <strong>Logon</strong> (HTTP-метод POST)</a></p>
</li>
<li>
<p><a href="#logonOTP">Метод <strong>LogonOTP</strong> (HTTP-метод POST)</a></p>
</li>
</ul>
</li>
<li>
<p><a href="#certAuthentication">Аутентификация по закрытому ключу сертификата</a></p>
<ul>
<li><a href="#logonCert">Метод <strong>LogonCert</strong> (HTTP-метод POST)</a></li>
</ul>
</li>
<li>
<p><a href="#recommendations">Рекомендации для банковского сервиса</a></p>
</li>
</ul>
</li>
<li>
<p><a href="#installation">Настройка обмена электронными документами</a></p>
<ul>
<li>
<p><a href="#installationGet">Получение настроек обмена с банком в автоматическом режиме</a></p>
<ul>
<li><a href="#getSettings">Метод <strong>GetSettings</strong> (HTTP-метод POST)</a></li>
</ul>
</li>
<li>
<p><a href="#exampleSettings">Пример XML-файла настроек обмена с банком</a></p>
</li>
</ul>
</li>
<li>
<p><a href="#transport">Порядок взаимодействия на транспортном уровне</a></p>
<ul>
<li>
<p><a href="#transportPacket">Формирование и разбор транспортного контейнера</a></p>
</li>
<li>
<p><a href="#send">Отправка электронных документов из 1С</a></p>
<ul>
<li><a href="#sendPack">Метод <strong>SendPack</strong> (HTTP-метод POST)</a></li>
</ul>
</li>
<li>
<p><a href="#get">Получение электронных документов в 1С</a></p>
<ul>
<li>
<p><a href="#getPackList">Метод <strong>GetPackList</strong> (HTTP-метод GET)</a></p>
</li>
<li>
<p><a href="#getPack">Метод <strong>GetPack</strong> (HTTP-метод GET)</a></p>
</li>
</ul>
</li>
</ul>
</li>
<li>
<p><a href="#test">Проверка работоспособности обмена электронными документами</a></p>
<ul>
<li><a href="#zond">Пример XML-файла запроса-зонда</a></li>
</ul>
</li>
<li>
<p><a href="#synchronization">Процедура синхронизации 1С с банковским сервисом</a></p>
</li>
<li>
<p><a href="../common-section/data-security.md#security">Обеспечение безопасности данных</a></p>
<ul>
<li>
<p><a href="../common-section/data-security.md#encryption">Шифрование данных при передаче</a></p>
</li>
<li>
<p><a href="../common-section/data-security.md#crypto">Криптооперации, выполняемые на стороне Клиента и Банка</a></p>
</li>
<li>
<p><a href="../common-section/data-security.md#signature">Защита электронных документов с помощью электронной подписи</a></p>
</li>
</ul>
</li>
<li>
<p><a href="../xsd-scheme/readme.md">Схемы данных</a></p>
</li>
<li>
<p><a href="../common-section/tables.md">Классификаторы</a></p>
<ul>
<li>
<p><a href="../common-section/tables.md#packet">Таблица кодов статусов транспортных контейнеров</a></p>
</li>
<li>
<p><a href="../common-section/tables.md#ed">Таблица кодов видов электронных документов</a></p>
</li>
<li>
<p><a href="../common-section/tables.md#status">Таблица кодов статусов электронных документов</a></p>
</li>
<li>
<p><a href="../common-section/tables.md#statementType">Таблица типов выписок банка</a></p>
</li>
<li>
<p><a href="../common-section/tables.md#errors">Таблица кодов ошибок и их описание, которые может возвращать банковский сервис в «1С:Предприятие 8»</a></p>
</li>
</ul>
</li>
<li>
<p><a href="../common-section/type-tables.md">Описание типов</a></p>
</li>
</ul>
<hr>
<h1 id="a-nameauthenticationa-порядок-взаимодействия-на-уровне-аутентификации."><a></a> Порядок взаимодействия на уровне аутентификации.</h1>
<p>Аутентификация Клиента с целью создания сессии на сервисе Банка может проходить по логину и паролю + дополнительный OTP («one time password», опционально) (<a href="#logon">Logon</a>) или по закрытому ключу сертификата (<a href="#logoncert">LogonCert</a>) .<br>
Аутентификация по логину и паролю используется для получения настроек из банка, поэтому на сервисе банка данный вид аутентификации должен всегда поддерживаться. В файле настроек можно задать данный способ аутентификации как основной и единственный или указать аутентификацию по закрытому ключу сертификата при дальнейшем обмене.</p>
<h2 id="a-namebaseauthenticationa-аутентификация-по-логину-и-паролю"><a></a> Аутентификация по логину и паролю</h2>
<p>Система «1С:Предприятия 8» передает в Банк уникальный идентификатор Клиента в банковской системе (строка может содержать только ANSI-символы в соответствии с <a href="http://tools.ietf.org/html/rfc2616">RFC 2616</a> для передачи значений в HTTP-заголовке) и строку, содержащую «логин + пароль» (строка «Basic логин:пароль» в формате BASE64 в соответствии с <a href="http://tools.ietf.org/html/rfc2617">Basic access authentication</a>) (отправка производится HTTP-методом POST - метод <a href="#logon">Logon</a>).</p>
<p>Особенностью данного процесса является то, что в момент первой аутентификации уникальный идентификатор Клиента неизвестен, тогда следует передавать «0».</p>
<p>В случае использования однофакторной аутентификации на стороне Банка и успешного завершения процесса сервис возвращает идентификатор сессии («маркер») в «1С:Предприятия 8» в синхронном режиме (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>).</p>
<p><img src="../doc_imgs/Logon.png" alt="Аутентификация по логину и паролю. Logon"></p>
<p>В случае использования двухфакторной аутентификации на стороне Банка (применение OTP – «one time password») и успешного начала процесса аутентификации сервис в синхронном режиме возвращает в «1С:Предприятия 8» идентификатор сессии, пока еще неавторизованной на стороне Банка, и признак, что требуется расширенная аутентификация (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>), а также направляет OTP на мобильный телефон Клиента.</p>
<p>«1С:Предприятии 8» выводит форму с текстом, содержащим идентификатор неавторизованной сессии и, возможно, маску мобильного телефона Клиента (если Банк ее прислал), и предлагает пользователю ввести OTP.</p>
<p>Система «1С:Предприятия 8» передает в Банк уникальный идентификатор Клиента в банковской системе и введенный OTP (отправка производится HTTP-методом POST - метод LogonOTP). В случае успешного завершения процесса банковский сервис возвращает идентификатор авторизованной сессии в «1С:Предприятия 8» в синхронном режиме (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>).</p>
<p>Особенностью данного процесса является то, что в момент первой аутентификации уникальный идентификатор Клиента неизвестен, тогда следует передавать «0».</p>
<p>В дальнейшем, идентификатор авторизованной сессии будет указан в каждом HTTP-запросе для обмена данными на транспортном уровне (в заголовке SID).</p>
<p>Результатом неуспешной аутентификации должна быть ошибка, возвращаемая банковской системой в синхронном режиме (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>).</p>
<p><img src="../doc_imgs/LogonOTP.png" alt="Аутентификация по логину и паролю. LogonOTP"></p>
<h3 id="a-namelogona-метод-logon-http-метод-post"><a></a> Метод Logon (HTTP-метод POST)</h3>
<p>Аутентификация по логину и паролю, однофакторная аутентификация</p>
<p><strong>Заголовки:</strong></p>
<ul>
<li>
<p>Host: &lt;Адрес ресурса банка&gt;</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>CustomerID: &lt;Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен&gt;</p>
</li>
<li>
<p>Authorization: Basic &lt;логин:пароль&gt;</p>
</li>
<li>
<p>APIVersion: &lt;Версия API обмена данными&gt;</p>
</li>
<li>
<p>AvailableAPIVersion: &lt;Доступная версия API обмена данными&gt;</p>
</li>
</ul>
<p><strong>Тело запроса:</strong></p>
<ul>
<li>ПУСТО</li>
</ul>
<p><strong>Успешный ответ:</strong></p>
<ul>
<li>
<p>HTTP/1.1 200 OK</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>Content: &lt; XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>&gt;</p>
</li>
</ul>
<p><strong>Параметры запроса:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>Host</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Адрес ресурса банка</td>
</tr>
<tr>
<td>CustomerID</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен</td>
</tr>
<tr>
<td>Authorization</td>
<td><a href="../common-section/type-tables.md#base64Binary">base64Binary</a></td>
<td align="center">[1]</td>
<td>«логин + пароль» в соответствии с <a href="http://tools.ietf.org/html/rfc2617">Basic access authentication</a></td>
</tr>
<tr>
<td>APIVersion</td>
<td><a href="../common-section/type-tables.md#FormatVersionType">FormatVersionType</a></td>
<td align="center">[1]</td>
<td>Версия API обмена данными</td>
</tr>
<tr>
<td>AvailableAPIVersion</td>
<td><a href="../common-section/type-tables.md#FormatVersionType">FormatVersionType</a></td>
<td align="center">[0-1]</td>
<td>Максимальная доступная для текущей информационной базы версия API обмена данными</td>
</tr>
</tbody>
</table><p><strong>Параметры ответа:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>ResultBank</td>
<td><a href="../common-section/type-tables.md#ResultBank">ResultBank</a></td>
<td align="center">[1]</td>
<td>Ответ банка</td>
</tr>
</tbody>
</table><p><strong>Пример запроса</strong> аутентификации по <strong>логину и паролю</strong>:</p>
<pre class=" language-http"><code class="prism  language-http">
<span class="token request-line"><span class="token property">POST</span> https://dbogate.demobank.ru/Logon HTTP/1.1</span>
<span class="token header-name keyword">Host:</span> dbogate.demobank.ru
<span class="token header-name keyword">Accept:</span> */*
<span class="token header-name keyword">CustomerID:</span> 502036
<span class="token header-name keyword">Authorization:</span> Basic NjY5NzcxNDczMTo5MzcyMjkxMzIx
<span class="token header-name keyword">APIVersion:</span> 2.1.1
<span class="token header-name keyword">AvailableAPIVersion:</span> 2.2.1
<span class="token header-name keyword">User-Agent:</span> 1C+Enterprise/8.3
<span class="token header-name keyword">Content-Type:</span> application/xml; charset=utf-8
<span class="token header-name keyword">Content-Length:</span> 0<span class="token application/xml">

</span></code></pre>
<p><strong>Пример успешного ответа</strong> на запрос аутентификации по <strong>логину и паролю</strong>:</p>
<pre class=" language-xml"><code class="prism  language-xml">
HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 145

<span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
&lt;ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1"&gt;
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Success</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>LogonResponse</span><span class="token punctuation">&gt;</span></span>
			<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>SID</span><span class="token punctuation">&gt;</span></span>8867755b6fbb4ae296aa0ac6b179ae88<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>SID</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>LogonResponse</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Success</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ResultBank</span><span class="token punctuation">&gt;</span></span>

</code></pre>
<p><strong>Пример успешного ответа</strong> на запрос аутентификации по <strong>логину и паролю + ОТР</strong>:</p>
<pre class=" language-xml"><code class="prism  language-xml">
HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 176

<span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
&lt;ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1"&gt;
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Success</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>LogonResponse</span><span class="token punctuation">&gt;</span></span>
            <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>SID</span><span class="token punctuation">&gt;</span></span>7767755b6fbb4ae296aa0ac6b179aef9<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>SID</span><span class="token punctuation">&gt;</span></span>
            <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>ExtraAuth</span><span class="token punctuation">&gt;</span></span>
            	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>OTP</span> <span class="token attr-name">phoneMask</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>7916***6465<span class="token punctuation">"</span></span> <span class="token punctuation">/&gt;</span></span>
            <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ExtraAuth</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>LogonResponse</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Success</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ResultBank</span><span class="token punctuation">&gt;</span></span>

</code></pre>
<h3 id="a-namelogonotpa-метод-logonotp-http-метод-post"><a></a> Метод LogonOTP (HTTP-метод POST)</h3>
<p>Аутентификация по логину и паролю, двухфакторная аутентификация</p>
<p><strong>Заголовки:</strong></p>
<ul>
<li>
<p>Host: &lt;Адрес ресурса банка&gt;</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>CustomerID: &lt;Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен&gt;</p>
</li>
<li>
<p>SID: &lt;Идентификатор неавторизованной сессии&gt;</p>
</li>
<li>
<p>OTP: &lt;Пользовательский OTP&gt;</p>
</li>
<li>
<p>APIVersion: &lt;Версия API обмена данными&gt;</p>
</li>
</ul>
<p><strong>Тело запроса:</strong></p>
<ul>
<li>ПУСТО</li>
</ul>
<p><strong>Успешный ответ:</strong></p>
<ul>
<li>
<p>HTTP/1.1 200 OK</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>Content: &lt; XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>&gt;</p>
</li>
</ul>
<p><strong>Параметры запроса:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>Host</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Адрес ресурса банка</td>
</tr>
<tr>
<td>SID</td>
<td><a href="../common-section/type-tables.md#IDType">IDType</a></td>
<td align="center">[1]</td>
<td>Идентификатор неавторизованной сессии</td>
</tr>
<tr>
<td>CustomerID</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен</td>
</tr>
<tr>
<td>OTP</td>
<td><a href="../common-section/type-tables.md#int">int</a></td>
<td align="center">[1]</td>
<td>Пользовательский OTP - целые числа 6-7 знаков</td>
</tr>
<tr>
<td>APIVersion</td>
<td><a href="../common-section/type-tables.md#FormatVersionType">FormatVersionType</a></td>
<td align="center">[1]</td>
<td>Версия API обмена данными</td>
</tr>
</tbody>
</table><p><strong>Параметры ответа:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>ResultBank</td>
<td><a href="../common-section/type-tables.md#ResultBank">ResultBank</a></td>
<td align="center">[1]</td>
<td>Ответ банка</td>
</tr>
</tbody>
</table><p><strong>Пример запроса</strong> передачи <strong>OTP</strong>:</p>
<pre class=" language-http"><code class="prism  language-http">
<span class="token request-line"><span class="token property">POST</span> https://dbogate.demobank.ru/LogonOTP HTTP/1.1</span>
<span class="token header-name keyword">Host:</span> dbogate.demobank.ru
<span class="token header-name keyword">Accept:</span> */*
<span class="token header-name keyword">SID:</span> 7767755b6fbb4ae296aa0ac6b179aef9
<span class="token header-name keyword">CustomerID:</span> 502036
<span class="token header-name keyword">OTP:</span> 034494
<span class="token header-name keyword">APIVersion:</span> 2.2.1
<span class="token header-name keyword">User-Agent:</span> 1C+Enterprise/8.3
<span class="token header-name keyword">Content-Type:</span> application/xml; charset=utf-8
<span class="token header-name keyword">Content-Length:</span> 0<span class="token application/xml">

</span></code></pre>
<p><strong>Пример успешного ответа</strong> на запрос передачи <strong>OTP</strong>:</p>
<pre class=" language-xml"><code class="prism  language-xml">
HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 145

<span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
&lt;ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1"&gt;
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Success</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>LogonResponse</span><span class="token punctuation">&gt;</span></span>
			<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>SID</span><span class="token punctuation">&gt;</span></span>8867755b6fbb4ae296aa0ac6b179ae88<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>SID</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>LogonResponse</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Success</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ResultBank</span><span class="token punctuation">&gt;</span></span>

</code></pre>
<h2 id="a-namecertauthenticationa-аутентификация-по-закрытому-ключу-сертификата"><a></a> Аутентификация по закрытому ключу сертификата</h2>
<p>Данный вид аутентификации нельзя использовать на этапе получения настроек из банка.</p>
<p>Система «1С:Предприятия 8» передает в Банк уникальный идентификатор Клиента в банковской системе (строка может содержать только ANSI-символы в соответствии с <a href="http://tools.ietf.org/html/rfc2616">RFC 2616</a> для передачи значений в HTTP-заголовке), а также  файл открытой части ключа электронной подписи Клиента, импортированный в систему «1С:Предприятие 8» на этапе настроек обмена (отправка производится HTTP-методом POST - метод <em>LogonCert,</em> передается  XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_AuthSign">XML-схеме данных для аутентификации по закр.ключу</a>).<br>
Банковский сервис по идентификатору клиента, серийному номеру сертификата и имени издателя выполняет поиск сертификата электронной подписи Клиента, если находит, то возвращает уникальный идентификатор (например, идентификатор неавторизованной сессии на стороне Банка) - «маркер» (строка в формате BASE64), зашифрованный с использованием открытого ключа электронной подписи Клиента (формат зашифрованных данных - PKCS#7). Если не находит, то возвращает ошибку аутентификации (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>).</p>
<p>Полученный маркер должен быть расшифрован на стороне «1С:Предприятия 8» с использованием закрытого ключа электронной подписи. В дальнейшем, расшифрованный уникальный идентификатор должен быть указан в каждом HTTP-запросе для обмена данными на транспортном уровне (в заголовке SID).</p>
<h3 id="a-namelogoncerta-метод-logoncert-http-метод-post"><a></a> Метод LogonCert (HTTP-метод POST)</h3>
<p><strong>Заголовки:</strong></p>
<ul>
<li>
<p>Host: &lt;Адрес ресурса банка&gt;</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>CustomerID: &lt;Уникальный идентификатор Клиента, содержащий только ANSI-символы.&gt;</p>
</li>
<li>
<p>APIVersion: &lt;Версия API обмена данными&gt;</p>
</li>
<li>
<p>AvailableAPIVersion: &lt;Доступная версия API обмена данными&gt;</p>
</li>
</ul>
<p><strong>Тело запроса:</strong></p>
<p>Content: &lt;XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_AuthSign">XML-схеме данных для аутентификации по закр.ключу</a>&gt;</p>
<p><strong>Успешный ответ:</strong></p>
<ul>
<li>
<p>HTTP/1.1 200 OK</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>Content: &lt; XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>&gt;</p>
</li>
</ul>
<p><strong>Параметры запроса:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>Host</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Адрес ресурса банка</td>
</tr>
<tr>
<td>CustomerID</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Уникальный идентификатор Клиента, содержащий только ANSI-символы.</td>
</tr>
<tr>
<td>APIVersion</td>
<td><a href="../common-section/type-tables.md#FormatVersionType">FormatVersionType</a></td>
<td align="center">[1]</td>
<td>Версия API обмена данными</td>
</tr>
<tr>
<td>AvailableAPIVersion</td>
<td><a href="../common-section/type-tables.md#FormatVersionType">FormatVersionType</a></td>
<td align="center">[0-1]</td>
<td>Максимальная доступная для текущей информационной базы версия API обмена данными</td>
</tr>
</tbody>
</table><p><strong>Параметры ответа:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>ResultBank</td>
<td><a href="../common-section/type-tables.md#ResultBank">ResultBank</a></td>
<td align="center">[1]</td>
<td>Ответ банка</td>
</tr>
</tbody>
</table><p><img src="../doc_imgs/LogonCert.png" alt="Аутентификация по закрытому ключу сертификата. LogonCert"></p>
<p><strong>Пример запроса</strong> аутентификации по <strong>закрытому ключу сертификата</strong>:</p>
<pre class=" language-http"><code class="prism  language-http">
<span class="token request-line"><span class="token property">POST</span> https://dbogate.demobank.ru/LogonCert HTTP/1.1</span>
<span class="token header-name keyword">Host:</span> dbogate.demobank.ru
<span class="token header-name keyword">Accept:</span> */*
<span class="token header-name keyword">CustomerID:</span> 502036
<span class="token header-name keyword">Authorization:</span> Basic NjY5NzcxNDczMTo5MzcyMjkxMzIx
<span class="token header-name keyword">APIVersion:</span> 2.1.1
<span class="token header-name keyword">AvailableAPIVersion:</span> 2.2.1
<span class="token header-name keyword">User-Agent:</span> 1C+Enterprise/8.3
<span class="token header-name keyword">Content-Type:</span> application/xml; charset=utf-8
<span class="token header-name keyword">Content-Length:</span> 2294<span class="token application/xml">

<span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>X509Data</span> <span class="token attr-name">xmlns</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://directbank.1c.ru/XMLSchema<span class="token punctuation">"</span></span> <span class="token attr-name"><span class="token namespace">xmlns:</span>xs</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema<span class="token punctuation">"</span></span> <span class="token attr-name"><span class="token namespace">xmlns:</span>xsi</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema-instance<span class="token punctuation">"</span></span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>64ec7428-df46-4522-a6fb-abc0fb5a643d<span class="token punctuation">"</span></span> <span class="token attr-name">formatVersion</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2.1.2<span class="token punctuation">"</span></span> <span class="token attr-name">creationDate</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2019-03-25T12:32:28<span class="token punctuation">"</span></span> <span class="token attr-name">userAgent</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>1С - БЭД: 1.6.1.4; БиблиотекаЭлектронныхДокументовДемо: 1.6.1.4<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>X509IssuerName</span><span class="token punctuation">&gt;</span></span>CRYPTO-PRO Test Center 2<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>X509IssuerName</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>X509SerialNumber</span><span class="token punctuation">&gt;</span></span>1200313235336897D366F6EE7A000000313235<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>X509SerialNumber</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>X509Certificate</span><span class="token punctuation">&gt;</span></span>MIIDDTCCArygAwIBAgITEgAxMjUzaJfTZvbuegAAADEyNTAIBgYqhQMCAgMwfzEj
MCEGCSqGSIb3DQEJARYUc3VwcG9ydEBjcnlwdG9wcm8ucnUxCzAJBgNVBAYTAlJV
MQ8wDQYDVQQHEwZNb3Njb3cxFzAVBgNVBAoTDkNSWVBUTy1QUk8gTExDMSEwHwYD
VQQDExhDUllQVE8tUFJPIFRlc3QgQ2VudGVyIDIwHhcNMTkwMTA5MDg0NDI0WhcN
MTkwNDA5MDg1NDI0WjAZMRcwFQYDVQQDDA7QoNC+0L3QsNC70LTQvjBmMB8GCCqF
AwcBAQEBMBMGByqFAwICJAAGCCqFAwcBAQICA0MABEAw/ghNB5iSXyI2fh9u+8Il
QplnMmGwJfEFcFyQi02gtYtQNdlIAHKVCBQB+xG2DVkAtVPJ862PtbXNSqX4x4qA
o4IBcDCCAWwwDgYDVR0PAQH/BAQDAgTwMBMGA1UdJQQMMAoGCCsGAQUFBwMCMB0G
A1UdDgQWBBTkj1PAQjxhYU2rl5u/bIiGNL6UvzAfBgNVHSMEGDAWgBQVMXywjRre
ZtcVnElSlxckuQF6gzBZBgNVHR8EUjBQME6gTKBKhkhodHRwOi8vdGVzdGNhLmNy
eXB0b3Byby5ydS9DZXJ0RW5yb2xsL0NSWVBUTy1QUk8lMjBUZXN0JTIwQ2VudGVy
JTIwMi5jcmwwgakGCCsGAQUFBwEBBIGcMIGZMGEGCCsGAQUFBzAChlVodHRwOi8v
dGVzdGNhLmNyeXB0b3Byby5ydS9DZXJ0RW5yb2xsL3Rlc3QtY2EtMjAxNF9DUllQ
VE8tUFJPJTIwVGVzdCUyMENlbnRlciUyMDIuY3J0MDQGCCsGAQUFBzABhihodHRw
Oi8vdGVzdGNhLmNyeXB0b3Byby5ydS9vY3NwL29jc3Auc3JmMAgGBiqFAwICAwNB
AM3KBkXlTmmTOSDA7lliffDKkc53o5DNtkP3/+hUv2BGqNaW0bs0UhNOgP3LEkLV
uTBy4kCyKkfBQ+cH/3w3IDU=<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>X509Certificate</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>X509Data</span><span class="token punctuation">&gt;</span></span>

</span></code></pre>
<p><strong>Пример успешного ответа</strong> на запрос аутентификации по <strong>закрытому ключу сертификата</strong>:</p>
<pre class=" language-xml"><code class="prism  language-xml">
HTTP/1.1 200 OK
Content-Length: 1036
Server: Microsoft-IIS/8.5
X-Powered-By: ASP.NET
Date: Mon, 25 Mar 2019 09:32:31 GMT

<span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>ResultBank</span> <span class="token attr-name">xmlns</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://directbank.1c.ru/XMLSchema<span class="token punctuation">"</span></span> <span class="token attr-name"><span class="token namespace">xmlns:</span>xs</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema<span class="token punctuation">"</span></span> <span class="token attr-name"><span class="token namespace">xmlns:</span>xsi</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema-instance<span class="token punctuation">"</span></span> <span class="token attr-name">formatVersion</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2.2.1<span class="token punctuation">"</span></span> <span class="token attr-name">userAgent</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>DirectBankService<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Success</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>LogonCertResponse</span><span class="token punctuation">&gt;</span></span>
            <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>EncryptedSID</span><span class="token punctuation">&gt;</span></span>MIIB4QYJKoZIhvcNAQcDoIIB0jCCAc4CAQAxggFwMIIBbAIBADCBljB/MSMwIQYJ
KoZIhvcNAQkBFhRzdXBwb3J0QGNyeXB0b3Byby5ydTELMAkGA1UEBhMCUlUxDzAN
BgNVBAcTBk1vc2NvdzEXMBUGA1UEChMOQ1JZUFRPLVBSTyBMTEMxITAfBgNVBAMT
GENSWVBUTy1QUk8gVGVzdCBDZW50ZXIgMgITEgAxMjUzaJfTZvbuegAAADEyNTAf
BggqhQMHAQEBATATBgcqhQMCAiQABggqhQMHAQECAgSBrDCBqTAoBCAW+Qq8GLci
fmTW/tiKOVHnVkUHGqnbqecRqALLhq6DQgQE6obJFKB9BgkqhQMHAQIFAQGgZjAf
BggqhQMHAQEBATATBgcqhQMCAiQABggqhQMHAQECAgNDAARA/aK/7zFLdZWLLdAl
5ZFLRjOjSN/f83tnb28/UU71zbYN4FL1KC/YflXoUQbPQne89OA9hPGtHo1cDT4t
ExbPGwQIqr3cc3r3JUUwVQYJKoZIhvcNAQcBMB8GBiqFAwICFTAVBAiqd9ePVONw
/AYJKoUDBwECBQEBgCcbOLtBwrqXz8dd6vNO4CsjbJFXMA/eieCj1NuFcenwX9f3
MfAQtpI=<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>EncryptedSID</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>LogonCertResponse</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Success</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ResultBank</span><span class="token punctuation">&gt;</span></span>
</code></pre>
<h2 id="a-namerecommendationsa-рекомендации-для-банковского-сервиса"><a></a> Рекомендации для банковского сервиса</h2>
<p>Время жизни авторизованной сессии на стороне банк.сервиса рекомендуется устанавливать 5 минут, а при получении новых запросов из «1С:Предприятия 8» – автоматически его продлевать.</p>
<p>В случае обнаружения 3-х подряд неверных попыток аутентификации на банковском сервисе в течение 30 секунд с одного IP-адреса рекомендуется отклонять все последующие попытки аутентификации с этого IP-адреса в течение 60 секунд.</p>
<p>В случае обнаружения 10-ти подряд неверных попыток аутентификации с одного IP-адреса в течение 120 секунд банковскому сервису рекомендуется отклонять все последующие попытки аутентификации с этого IP-адреса в течение 240 секунд, а также проинформировать клиента банка о возникшей ситуации альтернативными способами связи (SMS-оповещением и/или письмом по эл.почте).</p>
<p>При отклонении в систему «1С:Предприятие 8» банковский сервис должен в синхронном режиме возвращать соответствующую ошибку (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>).</p>
<h1 id="a-nameinstallationa-настройка-обмена-электронными-документами"><a></a> Настройка обмена электронными документами</h1>
<p>Для начала использования прямого обмена электронными документами из решений  «1С:Предприятие 8» с банковской системой Клиент должен получить параметры обмена данными из Банка – в «1С:Предприятии 8» будут автоматически созданы настройки ЭДО с Банком.</p>
<ul>
<li>Банковская система формирует файл настроек обмена с банком (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_Settings">XML-схеме настроек обмена с банком</a>), содержащий параметры обмена данными с банком.</li>
<li>Получение настроек проходит согласно протоколу, описанному в разделе <a href="#transport">«Порядок взаимодействия на транспортном уровне»</a>.</li>
<li>После получения файла настроек обмена система «1С:Предприятие 8» автоматически настроит систему согласно полученным параметрам.</li>
</ul>
<h2 id="a-nameinstallationgeta-получение-настроек-обмена-с-банком-в-автоматическом-режиме"><a></a> Получение настроек обмена с банком в автоматическом режиме</h2>
<p>Получение настроек обмена с банком в 1С выполняется в 3 этапа:</p>
<ul>
<li>
<p>аутентификация Клиента на стороне Банка (если нет ранее открытой сессии);</p>
</li>
<li>
<p>передача в банк расчетного счета Клиента, банк по этому номеру формирует файл настроек и отправляет обратно;</p>
</li>
<li>
<p>обработка ответа банка.</p>
</li>
</ul>
<h3 id="a-namegetsettingsa-метод--getsettings-http-метод-post"><a></a> Метод  GetSettings (HTTP-метод POST)</h3>
<p><strong>Заголовки:</strong></p>
<ul>
<li>
<p>Host: &lt;Адрес ресурса банка&gt;</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>CustomerID: &lt;Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен&gt;</p>
</li>
<li>
<p>Account: &lt;Номер расчетного счета Клиента, для зарплатных проектов может отсутствовать&gt;</p>
</li>
<li>
<p>Inn: &lt;Инн организации, для которой запрашиваются настройки&gt;</p>
</li>
<li>
<p>Bic: &lt;БИК Банка&gt;</p>
</li>
<li>
<p>SID: &lt;Идентификатор авторизованной сессии&gt;</p>
</li>
<li>
<p>APIVersion: &lt;Версия API обмена данными&gt;</p>
</li>
<li>
<p>AvailableAPIVersion: &lt;Доступная версия API обмена данными&gt;</p>
</li>
</ul>
<p><strong>Тело запроса:</strong></p>
<ul>
<li>ПУСТО</li>
</ul>
<p><strong>Успешный ответ:</strong></p>
<ul>
<li>
<p>HTTP/1.1 200 OK</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>Content: &lt; XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>&gt;</p>
</li>
</ul>
<p><strong>Параметры запроса:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>Host</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Адрес ресурса банка</td>
</tr>
<tr>
<td>CustomerID</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Уникальный идентификатор Клиента, содержащий только ANSI-символы. Передается 0, если CustomerID еще неизвестен</td>
</tr>
<tr>
<td>Account</td>
<td><a href="../common-section/type-tables.md#AccNumType">AccNumType</a></td>
<td align="center">[0-1]</td>
<td>Номер расчетного счета Клиента, для зарплатных проектов может отсутствовать</td>
</tr>
<tr>
<td>Inn</td>
<td><a href="../common-section/type-tables.md#string">string</a> (от 10 до 12)</td>
<td align="center">[1]</td>
<td>Инн организации, для которой запрашиваются настройки</td>
</tr>
<tr>
<td>Bic</td>
<td><a href="../common-section/type-tables.md#string">string</a> (9)</td>
<td align="center">[1]</td>
<td>БИК Банка</td>
</tr>
<tr>
<td>SID</td>
<td><a href="../common-section/type-tables.md#IDType">IDType</a></td>
<td align="center">[1]</td>
<td>Идентификатор авторизованной сессии</td>
</tr>
<tr>
<td>APIVersion</td>
<td><a href="../common-section/type-tables.md#FormatVersionType">FormatVersionType</a></td>
<td align="center">[1]</td>
<td>Версия API обмена данными</td>
</tr>
<tr>
<td>AvailableAPIVersion</td>
<td><a href="../common-section/type-tables.md#FormatVersionType">FormatVersionType</a></td>
<td align="center">[0-1]</td>
<td>Максимальная доступная для текущей информационной базы версия API обмена данными</td>
</tr>
</tbody>
</table><p><strong>Параметры ответа:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>ResultBank</td>
<td><a href="../common-section/type-tables.md#ResultBank">ResultBank</a></td>
<td align="center">[1]</td>
<td>Ответ банка</td>
</tr>
</tbody>
</table><p><strong>Описание:</strong></p>
<p>При запросе настроек обмена в автоматическом режиме система «1С:Предприятие 8» передает в Банк номер расчетного счета Клиента (отправка производится HTTP-методом POST - метод <a href="#getSettings">GetSettings</a>). Банковский сервис по номеру расчетному счета клиента формирует файл настроек (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_Settings">XML-схеме настроек обмена с банком</a> и в синхронном режиме возвращается в «1С:Предприятие 8» (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>).</p>
<p><img src="../doc_imgs/GetSettings.png" alt="Получение настроек обмена с банком в автоматическом режиме. GetSettings"></p>
<p>Результатом неуспешного запроса настроек обмена будет ошибка, возвращаемая банковской системой также в синхронном режиме (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>).</p>
<p>Особенностью данного процесса является то, что на момент первого запроса настроек обмена с банком в системе «1С:Предприятие 8» неизвестен уникальный идентификатор Клиента в банковской системе.<br>
В этом случае следует использовать «0» в качестве значения этого реквизита.</p>
<p><strong>Пример запроса</strong> получения настроек с банком:</p>
<pre class=" language-http"><code class="prism  language-http">
<span class="token request-line"><span class="token property">POST</span> https://dbogate.demobank.ru/GetSettings HTTP/1.1</span>
<span class="token header-name keyword">Host:</span> dbogate.demobank.ru
<span class="token header-name keyword">Accept:</span> */*
<span class="token header-name keyword">CustomerID:</span> 0
<span class="token header-name keyword">SID:</span> 8867755b6fbb4ae296aa0ac6b179ae88
<span class="token header-name keyword">Inn:</span> 761700021132
<span class="token header-name keyword">Bic:</span> 044525888
<span class="token header-name keyword">Account:</span> 40802810200000099888
<span class="token header-name keyword">APIVersion:</span> 2.1.1
<span class="token header-name keyword">AvailableAPIVersion:</span> 2.2.1
<span class="token header-name keyword">User-Agent:</span> 1C+Enterprise/8.3
<span class="token header-name keyword">Content-Type:</span> application/xml; charset=utf-8
<span class="token header-name keyword">Content-Length:</span> 0<span class="token application/xml">

</span></code></pre>
<p><strong>Пример успешного ответа</strong> на запрос получения настроек с банком:</p>
<pre class=" language-xml"><code class="prism  language-xml">
HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 2145

<span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
&lt;ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1"&gt;
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Success</span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>GetSettingsResponse</span> <span class="token attr-name">creationDate</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2015-02-19T11:21:02<span class="token punctuation">"</span></span> <span class="token attr-name">formatVersion</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2.2.1<span class="token punctuation">"</span></span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>502036<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
			<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Data</span> <span class="token attr-name">dockind</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>06<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPFNldHRpbmdzIHhtbG5zPSJo
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
IC8+CiAgICA8L0RhdGE+CjwvU2V0dGluZ3M+<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Data</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>GetSettingsResponse</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Success</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ResultBank</span><span class="token punctuation">&gt;</span></span>

</code></pre>
<h3 id="a-nameexamplesettingsa--пример-xml-файла-настроек-обмена-с-банком"><a></a>  Пример XML-файла настроек обмена с банком:</h3>
<ul>
<li><a href="../application-layer/Settings.xml">XML-файл <strong>настроек обмена с банком</strong></a></li>
</ul>
<pre class=" language-xml"><code class="prism  language-xml"><span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Settings</span> <span class="token attr-name">xmlns</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://directbank.1c.ru/XMLSchema<span class="token punctuation">"</span></span>
    <span class="token attr-name"><span class="token namespace">xmlns:</span>xsi</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema-instance<span class="token punctuation">"</span></span>
    <span class="token attr-name"><span class="token namespace">xmlns:</span>xsd</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema<span class="token punctuation">"</span></span>
    <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>EFD857B5-7FA8-4195-8666-2CCADBC3C8DE<span class="token punctuation">"</span></span>
    <span class="token attr-name">formatVersion</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2.2.1<span class="token punctuation">"</span></span>
    <span class="token attr-name">creationDate</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2016-04-22T09:38:51<span class="token punctuation">"</span></span> 
    <span class="token attr-name">userAgent</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>DemoBankService<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Sender</span> <span class="token attr-name">bic</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>044525888<span class="token punctuation">"</span></span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>ДЕМО-БАНК<span class="token punctuation">"</span></span> <span class="token punctuation">/&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Recipient</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2806<span class="token punctuation">"</span></span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>Торговый дом Комплексный<span class="token punctuation">"</span></span> <span class="token attr-name">inn</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>7705260699<span class="token punctuation">"</span></span> <span class="token attr-name">kpp</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>770501001<span class="token punctuation">"</span></span> <span class="token punctuation">/&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Data</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>CustomerID</span><span class="token punctuation">&gt;</span></span>2806<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>CustomerID</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>BankServerAddress</span><span class="token punctuation">&gt;</span></span>https://dbogate.demobank.ru/<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>BankServerAddress</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>FormatVersion</span><span class="token punctuation">&gt;</span></span>2.2.1<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>FormatVersion</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Encoding</span><span class="token punctuation">&gt;</span></span>UTF-8<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Encoding</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Logon</span><span class="token punctuation">&gt;</span></span>
            <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Login</span><span class="token punctuation">&gt;</span></span>
                <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>User</span><span class="token punctuation">&gt;</span></span>user_login<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>User</span><span class="token punctuation">&gt;</span></span>
            <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Login</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Logon</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Document</span> <span class="token attr-name">docKind</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>03<span class="token punctuation">"</span></span> <span class="token punctuation">/&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Document</span> <span class="token attr-name">docKind</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>05<span class="token punctuation">"</span></span> <span class="token punctuation">/&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Document</span> <span class="token attr-name">docKind</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>10<span class="token punctuation">"</span></span> <span class="token punctuation">/&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Document</span> <span class="token attr-name">docKind</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>11<span class="token punctuation">"</span></span> <span class="token punctuation">/&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Document</span> <span class="token attr-name">docKind</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>14<span class="token punctuation">"</span></span> <span class="token punctuation">/&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Document</span> <span class="token attr-name">docKind</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>30<span class="token punctuation">"</span></span> <span class="token punctuation">/&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Data</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Settings</span><span class="token punctuation">&gt;</span></span>
</code></pre>
<h1 id="a-name3a-порядок-взаимодействия-на-транспортном-уровне"><a></a> Порядок взаимодействия на транспортном уровне</h1>
<p>Данный уровень позволяет отправлять и получать электронные документы между системой «1С:Предприятие 8» Клиента и Банком по согласованным между сторонами обмена настройкам, используя шифрованный канал (протокол TLS 1.0/1.2) (подробнее см. раздел <a href="../common-section/data-security.md#security">«Обеспечение безопасности данных»</a>).</p>
<p>Инициатором сеанса обмена всегда выступает система «1С:Предприятие 8».</p>
<p>Для отправки и получения всех электронных документов используется единый адрес ресурса банка вида: <code>https://&lt;host&gt;:&lt;port&gt;</code>.</p>
<p>Для передачи электронных документов между участниками обмена используется «транспортный контейнер» - XML-файл, сформированный по определенному формату (<a href="../xsd-scheme/readme.md#1C-Bank_Packet">XML-схема транспортного контейнера</a>).</p>
<p>Отправителем и получателем транспортного контейнера могут быть как Клиент (Организация), работающий на системе «1С:Предприятие 8», так и Банк (роли участников обмена зависят от конкретной бизнес-операции).</p>
<h2 id="a-nametransportpacketa-формирование-и-разбор-транспортного-контейнера"><a></a> Формирование и разбор транспортного контейнера</h2>
<p>Формирование и разбор транспортного контейнер зависит от настроек обмена с конкретным банковским сервисом и может быть представлен в виде схем:</p>
<p><img src="../doc_imgs/shipping_container_write_and_read_rules.png" alt="Формирование и разбор транспортного контейнера"></p>
<h2 id="a-namesenda-отправка-электронных-документов-из-1с"><a></a> Отправка электронных документов из 1С</h2>
<p>Отправка электронных документов из 1С выполняется в 3 этапа:</p>
<ul>
<li>
<p>формирование транспортного контейнера, содержащего электронные документы;</p>
</li>
<li>
<p>аутентификация Клиента на стороне Банка (если нет ранее открытой сессии);</p>
</li>
<li>
<p>отправка транспортного контейнера в Банк и обработка ответа;</p>
</li>
</ul>
<p><img src="../doc_imgs/Sending_packages_of_electronic_documents_to_the_bank.png" alt="Отправка электронных документов из 1С в банковский сервис"></p>
<p>При отправке электронных документов из 1С будут последовательно вызваны следующие методы банковского сервиса:</p>
<ul>
<li>Аутентификация:</li>
<li>Для аутентификации по логину и паролю, только <a href="#logon">Logon</a>;</li>
<li>Для аутентификации по логину и паролю с двухфакторной авторизацией — <a href="#logon">Logon</a>, а затем — <a href="#logonOTP">LogonOTP</a>.</li>
<li>Отправка транспортного контейнера с данными электронных документов из 1С — <a href="#sendPack">SendPack</a>.</li>
</ul>
<h3 id="a-namesendpacka-метод-sendpack-http-метод-post"><a></a> Метод SendPack (HTTP-метод POST)</h3>
<p><strong>Заголовки:</strong></p>
<ul>
<li>
<p>Host: &lt;Адрес ресурса банка&gt;</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>CustomerID: &lt;Уникальный идентификатор Клиента, содержащий только ANSI-символы&gt;</p>
</li>
<li>
<p>SID: &lt;Идентификатор авторизованной сессии&gt;</p>
</li>
<li>
<p>APIVersion: &lt;Версия API обмена данными&gt;</p>
</li>
</ul>
<p><strong>Тело запроса:</strong></p>
<ul>
<li>&lt; XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_Packet">XML-схеме транспортного контейнера</a>&gt;</li>
</ul>
<p><strong>Успешный ответ:</strong></p>
<ul>
<li>HTTP/1.1 200 OK</li>
<li>Content-Type: application/xml; charset=utf-8</li>
<li>Content: &lt; XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>&gt;</li>
</ul>
<p><strong>Параметры запроса:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>Host</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Адрес ресурса банка</td>
</tr>
<tr>
<td>CustomerID</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Уникальный идентификатор Клиента, содержащий только ANSI-символы</td>
</tr>
<tr>
<td>SID</td>
<td><a href="../common-section/type-tables.md#IDType">IDType</a></td>
<td align="center">[1]</td>
<td>Идентификатор авторизованной сессии</td>
</tr>
<tr>
<td>APIVersion</td>
<td><a href="../common-section/type-tables.md#FormatVersionType">FormatVersionType</a></td>
<td align="center">[1]</td>
<td>Версия API обмена данными</td>
</tr>
<tr>
<td>Packet</td>
<td><a href="../common-section/type-tables.md#Packet">Packet</a></td>
<td align="center">[1]</td>
<td>Транспортный контейнер с данными электронных документов</td>
</tr>
</tbody>
</table><p><strong>Параметры ответа:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>ResultBank</td>
<td><a href="../common-section/type-tables.md#ResultBank">ResultBank</a></td>
<td align="center">[1]</td>
<td>Ответ банка</td>
</tr>
</tbody>
</table><p><strong>Описание:</strong></p>
<ul>
<li>
<p>В «1С:Предприятии 8» формируется транспортный контейнер (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_Packet">XML-схеме транспортного контейнера</a>) с одним или несколькими электронными документами.</p>
</li>
<li>
<p>Если настройки обмена между Клиентом и Банком предусматривают сжатие данных, то электронные документы перед помещением в транспортный контейнер сжимаются в формате zip-архива, который описан в открытой спецификации, доступной по адресу <a href="http://www.pkware.com/documents/casestudies/APPNOTE.TXT">http://www.pkware.com/documents/casestudies/APPNOTE.TXT</a>.</p>
</li>
<li>
<p>Далее проходит аутентификация на стороне Банка согласно протоколу, описанному в разделе «<a href="#authentication">Порядок взаимодействия на уровне аутентификации</a>».</p>
</li>
<li>
<p>Если аутентификация пройдена успешно, транспортный контейнер передается в Банк (отправка на ресурс Банка производится HTTP-методом POST – метод SendPack), в синхронном режиме банковская система возвращает либо ошибку получения, либо уникальный идентификатор на стороне Банка, который будет однозначно соответствовать полученному транспортному контейнеру (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>).</p>
</li>
<li>
<p>Банковская система может проводить контроль формата и разбор транспортного контейнера:</p>
</li>
<li>
<p>либо в синхронном режиме (сразу в момент получения);</p>
</li>
<li>
<p>либо в асинхронном режиме (ставит входящие трасп.контейнеры в очередь на разбор).</p>
</li>
<li>
<p>Для такого варианта уникальный идентификатор возвращается сразу, после того, как транспортный контейнер успешно был поставлен в очередь на разбор, не дожидаясь самого разбора.</p>
</li>
<li>
<p>Далее запускается процесс разбора очереди входящих транспортных контейнеров банковской системой, результатом которого будет ответ о состоянии обработки (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_StatusPacketNotice">XML-схеме извещения о состояния обработки транспортного контейнера</a>).</p>
</li>
<li>
<p>Подготовленный ответ Банка упаковывается в транспортный контейнер (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_Packet">XML-схеме транспортного контейнера</a>) и ожидает на стороне Банка очередного запроса на наличие подготовленных к передаче транспортных контейнеров из «1С:Предприятие 8».</p>
</li>
<li>
<p>Параллельно с процессом отправки ответа о состоянии обработки транспортного контейнера банковская система выполняет обработку каждого электронного документа, входящего в него.</p>
</li>
</ul>
<p><img src="../doc_imgs/SendPack.png" alt="Отправка электронных документов из 1С. SendPack"></p>
<p><strong>Пример отправки</strong> транспортного контейнера:</p>
<pre class=" language-xml"><code class="prism  language-xml">
POST https://dbogate.demobank.ru/SendPack HTTP/1.1
Host: dbogate.demobank.ru
Accept: */*
SID: 8867755b6fbb4ae296aa0ac6b179ae88
CustomerID: 502036
APIVersion: 2.2.1
User-Agent: 1C+Enterprise/8.3
Content-Type: application/xml; charset=utf-8
Content-Length: 5239

<span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Packet</span> <span class="token attr-name">xmlns</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://directbank.1c.ru/XMLSchema<span class="token punctuation">"</span></span>
	<span class="token attr-name"><span class="token namespace">xmlns:</span>xs</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema<span class="token punctuation">"</span></span>
    <span class="token attr-name"><span class="token namespace">xmlns:</span>xsi</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema-instance<span class="token punctuation">"</span></span>
    <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>9bad1c35-b7ff-11e4-9a88-0003ffb697db<span class="token punctuation">"</span></span>
    <span class="token attr-name">formatVersion</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2.2.1<span class="token punctuation">"</span></span>
    <span class="token attr-name">creationDate</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2016-04-19T11:12:31<span class="token punctuation">"</span></span>
    <span class="token attr-name">userAgent</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>1С - БЭД: 1.4<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Sender</span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Customer</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>502036<span class="token punctuation">"</span></span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>ИП Петрович и Ко<span class="token punctuation">"</span></span> <span class="token attr-name">inn</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>761700021132<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Sender</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Recipient</span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Bank</span> <span class="token attr-name">bic</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>044525888<span class="token punctuation">"</span></span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>ДЕМО-БАНК<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Recipient</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Document</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>a64225eb-9737-4d80-bd9d-1ffe5fdb63b1<span class="token punctuation">"</span></span> <span class="token attr-name">dockind</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>10<span class="token punctuation">"</span></span> <span class="token attr-name">formatVersion</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2.2.1<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Data</span><span class="token punctuation">&gt;</span></span>
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
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Data</span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Signature</span> <span class="token attr-name">x509IssuerName</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>Удостоверяющий Центр Банка<span class="token punctuation">"</span></span> <span class="token attr-name">x509SerialNumber</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>022C03015B03010F022FE2<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>SignedData</span><span class="token punctuation">&gt;</span></span>
MIIGbQYJKoZIhvcNAQcCoIIGXjCCBloCAQExEDAOBgorBgEEAa1ZAQIBBQAwCwYJKoZIhvcNAQcBoIIEwDCCBLwwggRdoAMCAQICCwFsAwFbAwEPAh/mMA4GCisGAQQBrVkBAwIFADCB7jELMAkGA1UEBhMCUlUxFTATBgNVBAgeDAQcBD4EQQQ6BDIEMDEVMBMGA1UEBx4MBBwEPgRBBDoEMgQwMTUwMwYDVQQKHiwEHgQQBB4AIAQRBDAEPQQ6ACAAIgQkBBoAIAQeBEIEOgRABEsEQgQ4BDUAIjFfMF0GA1UEAx5WBCMENAQ+BEEEQgQ+BDIENQRABE8ETgRJBDgEOQAgBCYENQQ9BEIEQAAgBB4EEAQeACAEEQQwBD0EOgAgACIEJAQaACAEHgRCBDoEQARLBEIEOAQ1ACIxGTAXBgkqhkiG9w0BCQEWCnBraUBvZmMucnUwHhcNMTQxMDA2MDcxMDMyWhcNMTUxMjEwMDcxMDMyWjCB2jELMAkGA1UEBhMCUlUxFTATBgNVBAgeDAQcBB4EIQQaBBIEEDEpMCcGA1UECh4gBBgEHwAgBB8ENQRCBEAEPgQyBDgERwAgBDgAIAQaBD4xDzANBgNVBAseBgBEAEIATzFFMEMGA1UEDB48BBgEPQQ0BDgEMgQ4BDQEQwQwBDsETAQ9BEsEOQAgBD8EQAQ1BDQEPwRABDgEPQQ4BDwEMARCBDUEOwRMMTEwLwYDVQQDHigEHwQ1BEIEQAQ+BDIAIAQfBDUEQgRAACAEHwQ1BEIEQAQ+BDIEOARHMF4wFgYKKwYBBAGtWQEGAgYIKoZIzj0DAQcDRAAEQQSvMegoDmW20Br8eWAZipeFbWfUR7J7d/pdCiO8pMw2lfHX1Vjet7cTaiG0vQhwmD+TGIOh+FgRHBkMZXNVDl1Do4IB6TCCAeUwHQYDVR0OBBYEFC8QX0Ex2lQqSBZDVujpCceXCG/wMIIBJAYDVR0jBIIBGzCCAReAFEnfU+U9thXTPfDAbd2Z2TnrZfZZoYH0pIHxMIHuMQswCQYDVQQGEwJSVTEVMBMGA1UECB4MBBwEPgRBBDoEMgQwMRUwEwYDVQQHHgwEHAQ+/EhM5EcGNPvvvYXrX14rtH0Q7J7yOAV1ROmMxggFwMIIBbAIBATCB/jCB7jELMAkGA1UEBhMCUlUxRJBDgEOQAgBCYENQQ9BEIEQAAgBB4EEAQeACAEEQQwBD0EOgAgACIEJAQaACAEHgRCBDoEQARLBEIEOAQ1ACIxGTAXBgkqhkiG9w0BCQEWCnBraUBvZmMucnUCCwFsAwFbAwEPAh/mMA4GCisGAQQBrVkBAgEFADAMBgorBgEEAa1ZAQYCBEgwRgIhALJ4SDHfRVBq9egxlJiAC+tGHRNU7vg4AIUA8iS9qFmOAiEA5rdyEyuYZ5H46JjDNVJexcYmgCuDNpiU15rskCKDuVc=
        	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>SignedData</span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Signature</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Document</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Packet</span><span class="token punctuation">&gt;</span></span>

</code></pre>
<p><strong>Пример успешного ответа</strong> на запрос отправки транспортного контейнера:</p>
<pre class=" language-xml"><code class="prism  language-xml">
HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 145

<span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
&lt;ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1"&gt;
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Success</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>SendPacketResponse</span><span class="token punctuation">&gt;</span></span>
			<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>ID</span><span class="token punctuation">&gt;</span></span>50214584626<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ID</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>SendPacketResponse</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Success</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ResultBank</span><span class="token punctuation">&gt;</span></span>

</code></pre>
<h3 id="a-namegeta-получение-электронных-документов-в-1с"><a></a> Получение электронных документов в 1С</h3>
<p>Получение электронных документов в 1С выполняется в 3 этапа:</p>
<ul>
<li>
<p>аутентификация Клиента на стороне Банка (если нет ранее открытой сессии);</p>
</li>
<li>
<p>запрос у Банка списка подготовленных к передаче транспортных контейнеров, содержащих электронные документы для Клиента;</p>
</li>
<li>
<p>запрос у Банка транспортного контейнера по его уникальному идентификатору и разбор в 1С.</p>
</li>
</ul>
<p><img src="../doc_imgs/Receive_packages_of_electronic_documents_from_the_bank.png" alt="Получение электронных документов из банковского сервиса в 1С"></p>
<p>При получении электронных документов из банковского сервиса в 1С будут последовательно вызваны следующие методы:</p>
<ul>
<li>Аутентификация:</li>
<li>Для аутентификации по логину и паролю, только <a href="#logon">Logon</a>;</li>
<li>Для аутентификации по логину и паролю с двухфакторной авторизацией — <a href="#logon">Logon</a>, а затем — <a href="#logonOTP">LogonOTP</a>.</li>
<li>Запрос на получение списка GUID транспортных контейнеров, готовых к отправке клиенту банком, — <a href="#getPackList">GetPackList</a>.</li>
<li>В цикле запрос на получение транспортного контейнера по GUID из ранее полученного списка <a href="#getPack">GetPack</a></li>
</ul>
<p>Если после получения и разбора всех контейнеров из списка, 1С не находит нужный контейнер, процесс будет начат заново. Это происходит при ожидании получения выписки банка.</p>
<h3 id="a-namegetpacklista-метод-getpacklist-http-метод-get"><a></a> Метод GetPackList (HTTP-метод GET)</h3>
<p><strong>Заголовки:</strong></p>
<ul>
<li>
<p>Host: &lt;Адрес ресурса банка&gt;</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>CustomerID: &lt;Уникальный идентификатор Клиента, содержащий только ANSI-символы&gt;</p>
</li>
<li>
<p>SID: &lt;Идентификатор авторизованной сессии&gt;</p>
</li>
<li>
<p>APIVersion: &lt;Версия API обмена данными&gt;</p>
</li>
</ul>
<p><strong>Адрес запроса:</strong></p>
<ul>
<li>https://&lt;Адрес ресурса банка&gt;/GetPackList?date=&lt;Отметка времени&gt;</li>
</ul>
<p>(Отметка времени в формате «dd.MM.yyyy HH:mm:ss», где dd – день числом, MM – месяц числом, yyyy – год числом, HH – часы в формате 24, mm – минуты, ss – секунды.)</p>
<p><strong>Успешный ответ:</strong></p>
<ul>
<li>HTTP/1.1 200 OK</li>
<li>Content-Type: application/xml; charset=utf-8</li>
<li>Content: &lt; XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>&gt;</li>
</ul>
<p><strong>Параметры запроса:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>Host</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Адрес ресурса банка</td>
</tr>
<tr>
<td>CustomerID</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Уникальный идентификатор Клиента, содержащий только ANSI-символы</td>
</tr>
<tr>
<td>SID</td>
<td><a href="../common-section/type-tables.md#IDType">IDType</a></td>
<td align="center">[1]</td>
<td>Идентификатор авторизованной сессии</td>
</tr>
<tr>
<td>APIVersion</td>
<td><a href="../common-section/type-tables.md#FormatVersionType">FormatVersionType</a></td>
<td align="center">[1]</td>
<td>Версия API обмена данными</td>
</tr>
</tbody>
</table><p><strong>Параметры ответа:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>ResultBank</td>
<td><a href="../common-section/type-tables.md#ResultBank">ResultBank</a></td>
<td align="center">[1]</td>
<td>Ответ банка</td>
</tr>
</tbody>
</table><p><strong>Важно:</strong></p>
<ul>
<li>Обратите внимание на параметр <em><strong>TimeStampLastPacket</strong></em> в ответе, это метка времени, на которую вернули всю актуальную информацию.</li>
</ul>
<p><strong>Описание:</strong></p>
<ul>
<li>
<p>Проходит аутентификация на стороне Банка согласно протоколу, описанному в разделе «<a href="#authentication">Порядок взаимодействия на уровне аутентификации</a>».</p>
</li>
<li>
<p>Если аутентификация пройдена успешно, система «1С:Предприятия 8» запрашивает Банк на наличие подготовленных транспортных контейнеров для Клиента (запрос на ресурс Банка производится HTTP-методом GET – метод GetPackList). В синхронном режиме банковская система возвращает список уникальных идентификаторов транспортных контейнеров, готовых к передаче со стороны Банка и отметку времени, равную дате и времени формирования (передаются значения даты и времени сервера Банка) самого последнего транспортного контейнера, входящего в этот список (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>).</p>
</li>
<li>
<p>Если в запросе из «1С:Предприятия 8» о готовых к передаче транспортных контейнерах указать «Отметку времени» (указывается значение даты и времени сервера Банка с точностью до секунды), то именно с этого момента времени будет формироваться список уникальных идентификаторов (в формате GUID), подготовленных к передаче в хронологическом порядке.</p>
<p>Для того, чтобы получить список GUID всех когда-либо сформированных транспортных контейнеров на банковской стороне, параметр «Отметка времени» в запросе не передается.</p>
</li>
</ul>
<p><strong>Пример запроса</strong> получения списка уникальных идентификаторов транспортных контейнеров:</p>
<pre class=" language-http"><code class="prism  language-http">
<span class="token request-line"><span class="token property">GET</span> https://dbogate.demobank.ru/GetPackList?date=16.02.2015%2011<span class="token attr-name">:25</span><span class="token attr-name">:32</span> HTTP/1.1</span>
<span class="token header-name keyword">Host:</span> dbogate.demobank.ru
<span class="token header-name keyword">Accept:</span> */*
<span class="token header-name keyword">SID:</span> 8867755b6fbb4ae296aa0ac6b179ae88
<span class="token header-name keyword">CustomerID:</span> 502036
<span class="token header-name keyword">APIVersion:</span> 2.2.1
<span class="token header-name keyword">User-Agent:</span> 1C+Enterprise/8.3

</code></pre>
<p><strong>Пример успешного ответа</strong> на запрос получения списка уникальных идентификаторов:</p>
<pre class=" language-xml"><code class="prism  language-xml">
HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 145


<span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
&lt;ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1"&gt;
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Success</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>GetPacketListResponse</span> <span class="token attr-name">TimeStampLastPacket</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2015-02-19T11:15:42<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
			<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>PacketID</span><span class="token punctuation">&gt;</span></span>50214585876<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>PacketID</span><span class="token punctuation">&gt;</span></span>
        <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>GetPacketListResponse</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Success</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ResultBank</span><span class="token punctuation">&gt;</span></span>

</code></pre>
<p>Система «1С:Предприятие 8» после получения списка GUID готовых к передаче транспортных контейнеров запрашивает конкретный транспортный контейнер по его GUID (запрос на ресурс Банка производится HTTP-методом GET – метод <a href="#getPack">GetPack</a>). В синхронном режиме банковская система возвращает либо ошибку, либо транспортный контейнер (XML-файл, соответствующий XML-схеме ответа банк.сервиса).</p>
<p>Далее происходит разбор содержимого транспортного контейнера уже в системе «1С:Предприятие 8».</p>
<h3 id="a-namegetpacka-метод-getpack-http-метод-get"><a></a> Метод GetPack (HTTP-метод GET)</h3>
<p><strong>Заголовки:</strong></p>
<ul>
<li>
<p>Host: &lt;Адрес ресурса банка&gt;</p>
</li>
<li>
<p>Content-Type: application/xml; charset=utf-8</p>
</li>
<li>
<p>CustomerID: &lt;Уникальный идентификатор Клиента, содержащий только ANSI-символы&gt;</p>
</li>
<li>
<p>SID: &lt;Идентификатор авторизованной сессии&gt;</p>
</li>
<li>
<p>APIVersion: &lt;Версия API обмена данными&gt;</p>
</li>
</ul>
<p><strong>Адрес запроса:</strong></p>
<ul>
<li>https://&lt;Адрес ресурса банка&gt;/GetPack?id=&lt; GUID транспортного контейнера&gt;</li>
</ul>
<p><strong>Тело запроса:</strong></p>
<ul>
<li>ПУСТО</li>
</ul>
<p><strong>Успешный ответ:</strong></p>
<ul>
<li>HTTP/1.1 200 OK</li>
<li>Content-Type: application/xml; charset=utf-8</li>
<li>Content: &lt; XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_ResultBank">XML-схеме ответа банк.сервиса</a>&gt;</li>
</ul>
<p><strong>Параметры запроса:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>Host</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Адрес ресурса банка</td>
</tr>
<tr>
<td>CustomerID</td>
<td><a href="../common-section/type-tables.md#string">string</a></td>
<td align="center">[1]</td>
<td>Уникальный идентификатор Клиента, содержащий только ANSI-символы</td>
</tr>
<tr>
<td>SID</td>
<td><a href="../common-section/type-tables.md#IDType">IDType</a></td>
<td align="center">[1]</td>
<td>Идентификатор авторизованной сессии</td>
</tr>
<tr>
<td>APIVersion</td>
<td><a href="../common-section/type-tables.md#FormatVersionType">FormatVersionType</a></td>
<td align="center">[1]</td>
<td>Версия API обмена данными</td>
</tr>
</tbody>
</table><p><strong>Параметры ответа:</strong></p>

<table>
<thead>
<tr>
<th>Параметр</th>
<th>Тип</th>
<th align="center">Кратность</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>ResultBank</td>
<td><a href="../common-section/type-tables.md#ResultBank">ResultBank</a></td>
<td align="center">[1]</td>
<td>Ответ банка</td>
</tr>
</tbody>
</table><p><strong>Пример запроса</strong> получения транспортного контейнера:</p>
<pre class=" language-http"><code class="prism  language-http">
<span class="token request-line"><span class="token property">GET</span> https://dbogate.demobank.ru/GetPack?id=50214585876 HTTP/1.1</span>
<span class="token header-name keyword">Host:</span> dbogate.demobank.ru
<span class="token header-name keyword">Accept:</span> */*
<span class="token header-name keyword">SID:</span> 8867755b6fbb4ae296aa0ac6b179ae88
<span class="token header-name keyword">CustomerID:</span> 502036
<span class="token header-name keyword">APIVersion:</span> 2.2.1
<span class="token header-name keyword">User-Agent:</span> 1C+Enterprise/8.3

</code></pre>
<p><strong>Пример успешного ответа</strong> на запрос получения транспортного контейнера:</p>
<pre class=" language-xml"><code class="prism  language-xml">
HTTP/1.1 200 OK
Content-Type: application/xml;charset=UTF-8
Content-Length: 2502

<span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
&lt;ResultBank xmlns ="http://directbank.1c.ru/XMLSchema" formatVersion="2.2.1"&gt;
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Success</span><span class="token punctuation">&gt;</span></span>
		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>GetPacketResponse</span> <span class="token attr-name">userAgent</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>Back Office<span class="token punctuation">"</span></span>
        <span class="token attr-name">creationDate</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2015-02-19T11:15:42<span class="token punctuation">"</span></span>
        <span class="token attr-name">formatVersion</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2.2.1<span class="token punctuation">"</span></span>
        <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>0ef6778b-4a2c-717c-e053-248c410af4aa<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
			<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Sender</span><span class="token punctuation">&gt;</span></span>
				<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Bank</span> <span class="token attr-name">bic</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>044525888<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
			<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Sender</span><span class="token punctuation">&gt;</span></span>
        	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Recipient</span><span class="token punctuation">&gt;</span></span>
				<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Customer</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>502036<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
	    	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Recipient</span><span class="token punctuation">&gt;</span></span>
        	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Document</span> <span class="token attr-name">notifyRequired</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>false<span class="token punctuation">"</span></span>
        	<span class="token attr-name">signResponse</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>false<span class="token punctuation">"</span></span>
        	<span class="token attr-name">encrypted</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>false<span class="token punctuation">"</span></span>
        	<span class="token attr-name">compressed</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>false<span class="token punctuation">"</span></span>
        	<span class="token attr-name">testOnly</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>false<span class="token punctuation">"</span></span>
        	<span class="token attr-name">formatVersion</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2.2.1<span class="token punctuation">"</span></span>
        	<span class="token attr-name">dockind</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>01<span class="token punctuation">"</span></span>
        	<span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>0f6d6032-31b0-8337-e053-248c410a1832<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
        		&lt;Data contentType="text/xml"fileName=""&gt;
PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPFN0YXR1c1BhY2tldE5vdGlj
ZSB4bWxucz0iaHR0cDovL2RpcmVjdGJhbmsuMWMucnUvWE1MU2NoZW1hIiBpZD0iMGY2ZDYwMzIt
MzFiMC04MzM3LWUwNTMtMjQ4YzQxMGExODMyIiBmb3JtYXRWZXJzaW9uPSIyLjIuMSIgY3JlYXRp
b25EYXRlPSIyMDE1LTAyLTE5VDExOjAzOjQ4Ij4KICA8U2VuZGVyPgogICAgPEJhbmsgYmljPSIw
NDQ1MjU5ODUiLz4KICA8L1NlbmRlcj4KICA8UmVjaXBpZW50PgogICAgPEN1c3RvbWVyIGlkPSI1
MDIwMzYyNzEyMSIvPgogIDwvUmVjaXBpZW50PgogIDxJRFJlc3VsdFN1Y2Nlc3NSZXNwb25zZT41
MDIxNDU4MDgzODwvSURSZXN1bHRTdWNjZXNzUmVzcG9uc2U+CiAgPFJlc3VsdD4KICAgIDxTdGF0
dXM+CiAgICAgIDxDb2RlPjAxPC9Db2RlPgogICAgICA8TmFtZT7Qn9GA0LjQvdGP0YI8L05hbWU+
CiAgICA8L1N0YXR1cz4KICA8L1Jlc3VsdD4KPC9TdGF0dXNQYWNrZXROb3RpY2U+Cg==
         		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Data</span><span class="token punctuation">&gt;</span></span>
         		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Signature</span> <span class="token attr-name">x509SerialNumber</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>076C03010BBF0109029965<span class="token punctuation">"</span></span> <span class="token attr-name">x509IssuerName</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>CN=Удостоверяющий Центр Банка<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
            		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>SignedData</span><span class="token punctuation">&gt;</span></span>
MIIIGAYJKoZIhvcNAQcCoIIICTCCCAUCAQExDjAMBgorBgEEAa1ZAQIBMAsGCSqGSIb3DQEHAaCCBOkwggTlMIIEh6ADAgECAgsBbAMBC78BCQKZZTAOBgorBgEEAa1ZAQMCBQAwgc4xCzAJBgNVBAYTAlJVMRUwEwYDVQQIHgwEHAQ+BEEEOgQyBDAxFTATBgNVBAceDAQcBD4EQQQ6BDIEMDEpMCcGA1UECh4gBB0EHgQcBB4EIQAtBBEEEAQdBBoAIAAoBB4EEAQeACkxSTBHBgNVBAMeQAQjBDQEPgRBBEIEPgQyBDUEQARPBE4ESQQ4BDkAIAQmBDUEPQRCBEAAIAQdBB4EHAQeBCEALQQRBBAEHQQaBDAxGzAZBgkqhkiG9w0BCQEWDHBraUBub21vcy5ydTAeFw0xMzExMTQwNzMyNThaFw0xNTAxMTgwNzMyNThaMIIBITELMAkGA1UEBhMCUlUxFTATBgNVBAgeDAQcBD4EQQQ6BDIEMDEVMBMGA1UEBx4MBBwEPgRBBDoEMgQwMS0wKwYDVQQKHiQAIgQdBB4EHAQeBCEALQQRBBAEHQQaACIAIAAoBB4EEAQeACkxDzANBgNVBAseBgBEAEIATzEbMBkGA1UEDB4SBB8EQAQ1BDcEOAQ0BDUEPQRCMWkwZwYDVQQDHmAEIAQ+BDwEMAQ1BDIAIAQUBDwEOARCBEAEOAQ5ACAEFwQwBDoENQRABDgENQQyBDgERwAgACgEQgQ1BEEEQgQ+BDIESwQ5ACAENAQ7BE8AIAAxBEEALQBnAGEAdABlACkxHDAaBgkqhkiG9w0BCQEWDXRlc3RAbm9tb3MucnUwXjAWBgorBgEEAa1ZAQYCBggqhkjOPQMBBwNEAARBBBoll+XCHC/ID+sPpsUVDoRJU/HDcmYuGNPMVdXPRL07BSieQTeK4xA6dBOTLR1Z8ucCMk88DDKcsMpVoq1absejggHrMIIB5zAdBgNVHQ4EFgQUiBzxHvsiSOqenCuS8clloCsNaPAwgewGA1UdIwSB5DCB4aGB1KSB0TCBzjELMAkGA1UEBhMCUlUxFTATBgNVBAgeDAQcBD4EQQQ6BDIEMDEVMBMGA1UEBx4MBBwEPgRBBDoEMgQwMSkwJwYDVQQKHiAEHQQeBBwEHgQhAC0EEQQQBB0EGgAgACgEHgQQBB4AKTFJMEcGA1UEAx5ABCMENAQ+BEEEQgQ+BDIENQRABE8ETgRJBDgEOQAgBCYENQQ9BEIEQAAgBB0EHgQcBB4EIQAtBBEEEAQdBBoEMDEbMBkGCSqGSIb3DQEJARYMcGtpQG5vbW9zLnJ1gggBbAEBAQkBATALBgNVHQ8EBAMCAvwwUgYDVR0fBEswSTBHoEWgQ4ZBaHR0cDovL3d3dy5ub21vcy5ydS9mLzEvY29ycG9yYXRlL3JlbW90ZS9jYS1yZWdsYW1lbnQvQ0FfMjAxMC5jcmwwdgYDVR0gBG8wbTBrBgorBgEEAYKcBQEBMF0wPgYIKwYBBQUHAgEWMmh0dHA6Ly93d3cubm9tb3MucnUvY29ycG9yYXRlL3JlbW90ZS9jYS1yZWdsYW1lbnQvMBsGCCsGAQUFBwICMA8aDcTBziBCUy1DbGllbnQwDgYKKwYBBAGtWQEDAgUAA0gAMEUCIDuMdg2vSXPo2m2OhyOfD/j9W3j++cq54aLhgHGX7bPnAiEAtuUXHPpSyGNVmb5EWlwXVKGg5suhKYMNyK43/tOkXr0xggL0MIIC8AIBATCB3jCBzjELMAkGA1UEBhMCUlUxFTATBgNVBAgeDAQcBD4EQQQ6BDIEMDEVMBMGA1UEBx4MBBwEPgRBBDoEMgQwMSkwJwYDVQQKHiAEHQQeBBwEHgQhAC0EEQQQBB0EGgAgACgEHgQQBB4AKTFJMEcGA1UEAx5ABCMENAQ+BEEEQgQ+BDIENQRABE8ETgRJBDgEOQAgBCYENQQ9BEIEQAAgBB0EHgQcBB4EIQAtBBEEEAQdBBoEMDEbMBkGCSqGSIb3DQEJARYMcGtpQG5vbW9zLnJ1AgsBbAMBC78BCQKZZTAMBgorBgEEAa1ZAQIBoIIBoTAYBgkqhkiG9w0BCQMxCwYJKoZIhvcNAQcBMBwGCSqGSIb3DQEJBTEPFw0xNTAyMTkwODA1MTNaMC8GCSqGSIb3DQEJBDEiBCDwWSYDkO3so1xbyB9h+5Mg52gP3ijqZJf1ahFRkZY6yjCCATQGCyqGSIb3DQEJEAIvMYIBIzCCAR8wggEbMIIBFzAMBgorBgEEAa1ZAQIBBCAgbg9HriqIAf1RGVxkrmpXXGeP7FhgC+mpECPlbRVmgTCB5DCB1KSB0TCBzjELMAkGA1UEBhMCUlUxFTATBgNVBAgeDAQcBD4EQQQ6BDIEMDEVMBMGA1UEBx4MBBwEPgRBBDoEMgQwMSkwJwYDVQQKHiAEHQQeBBwEHgQhAC0EEQQQBB0EGgAgACgEHgQQBB4AKTFJMEcGA1UEAx5ABCMENAQ+BEEEQgQ+BDIENQRABE8ETgRJBDgEOQAgBCYENQQ9BEIEQAAgBB0EHgQcBB4EIQAtBBEEEAQdBBoEMDEbMBkGCSqGSIb3DQEJARYMcGtpQG5vbW9zLnJ1AgsBbAMBC78BCQKZZTAOBgorBgEEAa1ZAQYCBQAERzBFAiAytZ9jF+7AJVoiFF7DNkQX5r/zzUvKAPRLDLzGxdONPAIhAKrwnkXPd4K4vOGy64JLkLlGJZbCJagYYuV57EYI3mvq
            		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>SignedData</span><span class="token punctuation">&gt;</span></span>
         		<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Signature</span><span class="token punctuation">&gt;</span></span>
        	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Document</span><span class="token punctuation">&gt;</span></span>
    	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>GetPacketResponse</span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Success</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ResultBank</span><span class="token punctuation">&gt;</span></span>

</code></pre>
<h1 id="a-nametesta-проверка-работоспособности-обмена-электронными-документами."><a></a> Проверка работоспособности обмена электронными документами.</h1>
<ul>
<li>На этапе отладки или профилактических работ часто возникает потребность проверить доступность банковского сервиса, не приступая к штатному обмену электронными документами. Для таких целей предназначен специальный вид электронного документа.</li>
<li>По команде в «1С:Предприятии 8» формируется электронный документ «Запрос-зонд» (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_Probe">XML-схеме запроса-зонда</a>).</li>
<li>Если используется электронная подпись на стороне «1С:Предприятия 8» (см. раздел <a href="../common-section/data-security.md#security">«Обеспечение безопасности данных»</a>), то система предложит пользователю подписать электронный документ.</li>
<li>Электронный документ «Запрос-зонд» с электронной подписью (если используется, см. раздел <a href="../common-section/data-security.md#security">«Обеспечение безопасности данных»</a>) помещаются в транспортный контейнер (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_Packet">XML-схеме транспортного контейнера</a>), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне).</li>
<li>Далее передача данных в Банк проходит согласно протоколу, описанному в разделе «<a href="#transport">Порядок взаимодействия на транспортном уровне</a>».</li>
<li>Если отправка прошла успешно, то система «1С:Предприятие 8» изменит статус электронного документа «Запрос-зонд» на «Отправлен».</li>
<li>После получения из Банка ответа по результатам обработки транспортного контейнера система «1С:Предприятие 8» назначит электронному документу запроса статус «Доставлен».</li>
<li>По результатам контроля и обработки запроса-зонда на стороне Банка всегда формируется электронный документ «Извещение о состоянии электронного документа» (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_StatusDocNotice">XML-схеме извещения о состоянии электронного документа</a>), который содержит:</li>
<li>либо ошибку обработки запроса (<a href="../common-section/type-tables.md#ErrorType">Error</a>) с соответствующим описанием исключительной ситуации;</li>
<li>либо (<a href="../common-section/type-tables.md#StatusType">Status</a>) код «01», означающий что проверка успешно завершена.</li>
<li>Если используется электронная подпись (см. раздел <a href="../common-section/data-security.md#security">«Обеспечение безопасности данных»</a>), то электронный документ «Извещение о состоянии электронного документа» подписывается.</li>
<li>Банковская система формирует транспортный контейнер (XML-файл, соответствующий <a href="../xsd-scheme/readme.md#1C-Bank_Packet">XML-схеме транспортного контейнера</a>), согласно настройкам обмена между Клиентом и Банком (в частности, применение сжатия данных на прикладном уровне) и ставит в очередь на передачу в «1С:Предприятие 8».</li>
<li>Далее получение данных из Банка проходит согласно протоколу, описанному в разделе <a href="#transport">«Порядок взаимодействия на транспортном уровне»</a>.</li>
<li>Статус запроса-зонда система «1С:Предприятие 8» сообщит пользователю после успешного разбора входящего транспортного контейнера из Банка.</li>
</ul>
<h4 id="a-namezonda-пример-xml-файла-запроса-зонда"><a></a> Пример XML-файла запроса-зонда</h4>
<ul>
<li><a href="../application-layer/Probe.xml">XML-файл <strong>запроса-зонда</strong></a></li>
</ul>
<pre class=" language-xml"><code class="prism  language-xml"><span class="token prolog">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Probe</span> <span class="token attr-name">xmlns</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://directbank.1c.ru/XMLSchema<span class="token punctuation">"</span></span>
	<span class="token attr-name"><span class="token namespace">xmlns:</span>xs</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema<span class="token punctuation">"</span></span>
    <span class="token attr-name"><span class="token namespace">xmlns:</span>xsi</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>http://www.w3.org/2001/XMLSchema-instance<span class="token punctuation">"</span></span>
	<span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>eafa28b5-6600-424c-b0d3-3785274d570d<span class="token punctuation">"</span></span>
	<span class="token attr-name">formatVersion</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2.2.1<span class="token punctuation">"</span></span>
	<span class="token attr-name">creationDate</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>2016-04-22T09:33:57<span class="token punctuation">"</span></span>
	<span class="token attr-name">userAgent</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>1С - БЭД: 1.4.1.1; БиблиотекаЭлектронныхДокументов: 1.4.1.1<span class="token punctuation">"</span></span><span class="token punctuation">&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Sender</span> <span class="token attr-name">id</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>id:42;s:9999<span class="token punctuation">"</span></span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>Торговый дом Комплексный<span class="token punctuation">"</span></span> <span class="token attr-name">inn</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>7705260699<span class="token punctuation">"</span></span> <span class="token attr-name">kpp</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>770501001<span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
	<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>Recipient</span> <span class="token attr-name">bic</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>044525888<span class="token punctuation">"</span></span> <span class="token attr-name">name</span><span class="token attr-value"><span class="token punctuation">=</span><span class="token punctuation">"</span>ДЕМО-БАНК <span class="token punctuation">"</span></span><span class="token punctuation">/&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>Probe</span><span class="token punctuation">&gt;</span></span>
</code></pre>
<h1 id="a-name5a-процедура-синхронизации-1с-с-банковским-сервисом"><a></a> Процедура синхронизации 1С с банковским сервисом</h1>
<p>При синхронизации 1С с банковским сервисом последовательно выполняются процедуры отправки всех подготовленных в 1С транспортных контейнеров с данными электронных документов в банк и получения всех транспортных контейнеров документов из банка.</p>
<p><a href="#send">Процедура отправки в банк</a> выполняется в цикле. Для каждого подготовленного к отправке транспортного контейнера будут вызваны следующие методы:</p>
<ul>
<li>Аутентификация:</li>
<li>Для аутентификации по логину и паролю, только <a href="#logon">Logon</a>;</li>
<li>Для аутентификации по логину и паролю с двухфакторной авторизацией — <a href="#logon">Logon</a>, а затем — <a href="#logonOTP">LogonOTP</a>.</li>
<li>Отправка транспортного контейнера с данными электронных документов из 1С — <a href="#sendPack">SendPack</a>.</li>
</ul>
<p>При <a href="#get">получении электронных документов из банковского сервиса в 1С</a> будут последовательно вызваны следующие методы:</p>
<ul>
<li>Аутентификация:</li>
<li>Для аутентификации по логину и паролю, только <a href="#logon">Logon</a>;</li>
<li>Для аутентификации по логину и паролю с двухфакторной авторизацией — <a href="#logon">Logon</a>, а затем — <a href="#logonOTP">LogonOTP</a>.</li>
<li>Запрос на получение списка GUID транспортных контейнеров, готовых к отправке клиенту банком — <a href="#getPackList">GetPackList</a>.</li>
<li>Для каждого GUID из ранее полученного списка запрос на получение транспортного контейнера — <a href="#getPack">GetPack</a>.</li>
</ul>

