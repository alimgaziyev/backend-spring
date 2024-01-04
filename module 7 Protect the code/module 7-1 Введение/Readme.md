<p><code>Backend</code> - это программа или набор программ, которые реализуют бизнес-логику и работу с базами данных. Чаще всего, <code>backend</code> предоставляет <code>API</code>, который используют другие клиенты.</p>

<p>Например: У нас есть магазин по продаже клавиатур. Разработчик реализует бизнес-логику процессов администратора:</p>

<pre><code>findAllKeyboards();
findKeyboardByName(String name);
addKeyboard(Keyboard keyboard);
updateKeyboard(Keyboard keyboard);
deleteKeyboard(Keyboard);</code></pre>

<p>Теперь разработчику нужно открыть возможность использовать данные методы другим людям по сети интернет.</p>

<p>Разработчик должен реализовать способ взаимодействия сервиса извне. Он решает написать <code>REST API</code>.</p>

<p><code>API</code> - Application Programming Interface - это список методов, которые отрабатываются в <code>backend</code>.</p>

<p><code>REST</code> означает (<code>REpresentational State Transfer</code>) Передача репрезентативного состояния. Это стиль написания веб-сервисов, который использует различные концепции, которые уже присутствуют в протоколе <code>HTTP</code>.</p>

<p><code>REST</code> - представляет способ семантической связи с протоколом <code>HTTP</code>. Например, в протоколе <code>HTTP</code> есть метод <code>GET</code>. Логично, что данным типом метода должны пользоваться только методы для получения данных: <code>findAllKeyboard</code>, <code>findKeyboardByName</code></p>

<p><img alt="" height="319" name="image.png" src="https://ucarecdn.com/284b7372-8495-48a7-b726-2cc39fdb72b5/" width="793"></p>

<p>Повторно про <code>http</code> вы можете почитать <a href="https://stepik.org/lesson/679690/step/3?unit=678443" rel="nofollow noopener noreferrer">здесь</a>.</p>

<p>Все пути к определенному методу <code>API</code> описываются в коде. Для определения, какой из методов выполнять, используется <code>path</code> в <code>url</code>:</p>

<p> </p>

<p><img alt="" height="423" name="image.png" src="https://ucarecdn.com/60d975c5-092b-4f42-8447-6a740d97fb77/" width="805"></p>

<p>При разработке по <code>REST</code> мы можем представить наши методы в таком виде:</p>

<p>Первый параметр это <code>http</code> метод, второй <code>path</code>, третий это вызываемый метод.</p>

<pre><code>GET /keyboards findAllKeyboards();
GET /keyboards findKeyboardByName(String name);
POST /keyboards addKeyboard(Keyboard keyboard);
PUT /keyboards updateKeyboard(Keyboard keyboard);
DELETE /keyboards/1 deleteKeyboard(Keyboard);
</code></pre>

<p>Важно отметить, что с REST вам нужно думать о приложении с точки зрения ресурсов: Определите, какие ресурсы вы хотите открыть для внешнего мира и используйте глаголы, уже определенные протоколом HTTP для выполнения операций с этими ресурсами.</p>

<ul>
	<li>Формат обмена данными: здесь нет никаких ограничений. JSON — очень популярный формат, хотя можно использовать и другие, такие как XML.</li>
	<li>Транспорт: всегда HTTP. REST полностью построен на основе HTTP.</li>
	<li>Определение сервиса: не существует стандарта для этого, а REST является гибким. Это может быть недостатком в некоторых сценариях, поскольку потребляющему приложению может быть необходимо понимать форматы запросов и ответов. Однако широко используются такие языки определения веб-приложений, как WADL (Web Application Definition Language) и Swagger.</li>
</ul>

<p>Компоненты <code>HTTP</code></p>

<p><code>HTTP</code> определяет следующую структуру запроса:</p>

<ul>
	<li>строка запроса — определяет тип сообщения. К примеру <code>/users?name=AZ</code>.</li>
	<li>заголовки запроса — характеризуют тело сообщения, параметры передачи и прочие сведения. <code>X-API-TOKEN: supersecret</code>.</li>
	<li>тело сообщения (body) — необязательное. <code>JSON</code>, <code>XML</code> или <code>RAW</code>.</li>
</ul>

<p><code>HTTP</code> определяет следующую структуру ответного сообщения (response):</p>

<ul>
	<li>строка состояния (status line), включающая код состояния и сообщение о причине. <code>200</code>, <code>404</code> и другие.</li>
	<li>поля заголовка ответа (header fields). <code>SET-COOKIE: ooooo</code>.</li>
	<li>дополнительное тело сообщения (body). <code>JSON</code>, <code>XML</code>, <code>RAW</code>, изображения, видео, файлы.</li>
</ul>

<p>Дополнительный материалы для изучения:</p>

<ul>
	<li><a href="https://developer.mozilla.org/ru/docs/Web/HTTP/Overview" rel="nofollow noopener noreferrer">HTTP Overview</a></li>
	<li><a href="https://developer.mozilla.org/ru/docs/Learn/Common_questions/What_is_a_URL" rel="nofollow noopener noreferrer">URL Overview</a></li>
	<li><a href="https://www.json.org/json-en.html" rel="nofollow noopener noreferrer">JSON Overview</a></li>
</ul>