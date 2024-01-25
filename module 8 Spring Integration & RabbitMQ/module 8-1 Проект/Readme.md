<h1>Integration</h1>

<h2>Какие навыки вы приобретете, выполнив данный проект:</h2>

<ul>
	<li><code>REST API</code></li>
	<li><code>RabbitMQ</code></li>
	<li><code>Spring Email</code></li>
	<li><code>Spring scheduled</code></li>
</ul>

<h2>Введение</h2>

<p>Сегодня разработчики предпочитают создавать приложения с микросервисной архитектурой, вместо монолитной. Любая система, состоящая из множества небольших сервисов не может обойтись без коммуникаций с другими сервисами. Это может&nbsp;<code>REST API</code>&nbsp;- в таком случае сервисы напрямую отправляют друг-другу информацию. Однако, есть способы быстрее -&nbsp;<code>брокер сообщений</code>.</p>

<p><code>RabbitMQ</code>&nbsp;&mdash; это брокер сообщений, который можно использовать, чтобы позволить разным сервисам обмениваться данными через общий протокол.&nbsp;<code>RabbitMQ</code>&nbsp;может работать как очередь заданий, которая отправляет таски распределенным исполнителям.</p>

<p>Очередь сообщений (англ. &quot;message queuing&quot;) &mdash; это метод коммуникации между разными сервисами. Благодаря очередям сообщений сервисы могут оставаться полностью независимыми при выполнении своих индивидуальных задач. Сообщения обычно представляют собой небольшие запросы, ответы, обновления статуса или даже небольшую информацию. Очередь сообщений предоставляет временное место для хранения этих сообщений, позволяя приложениям отправлять и получать их по мере необходимости.</p>

<p>Сообщения идут в одном направлении, от издателя (англ. &quot;<code>publisher</code>&quot;) к брокеру и, наконец, к потребителю (англ. &quot;<code>consumer</code>&quot;):</p>

<h2>Описание:</h2>

<p>В этом проекте вам необходимо реализовать систему для оформления и оплаты заказов в одном интернет магазине.</p>

<p><img alt="" height="546" name="image.png" src="https://ucarecdn.com/57911fb3-c520-4d8e-9e16-6d0427926f4b/" width="1280" /></p>

<ul>
	<li><code>C1</code>,&nbsp;<code>C2</code>&nbsp;- это клиенты, которые взаимодействуют с сервисами по&nbsp;<code>http</code>;</li>
	<li><code>RabbitMQ</code>&nbsp;- брокер сообщений, который занимается созданием и управлением&nbsp;<code>queue</code>&nbsp;и&nbsp;<code>exchange</code>.</li>
	<li><code>Core</code>&nbsp;- это сервис, который имеет доступ к базе данных. Реализует&nbsp;<code>CRUD</code>&nbsp;для заказов.</li>
	<li><code>Mailing</code>&nbsp;- сервис для отправки писем.</li>
	<li><code>Order</code>&nbsp;- сервис, который принимает запросы по ресурсу&nbsp;<code>/order</code>. Отвечает за создание заказа на определенную сумму. При получении запроса по ресурсу&nbsp;<code>/order</code>&nbsp;отправляет сообщение в&nbsp;<code>Core</code>&nbsp;для создания заказа. После успешного создания заказа отправляет письмо с информацией.</li>
	<li><code>Payment</code>&nbsp;- сервис, который принимает запросы по ресурсу&nbsp;<code>/payment</code>. Отвечает за оплату заказа. При получении запроса по ресурсу&nbsp;<code>/payment</code>, сервис отправляет сообщение в&nbsp;<code>Core</code>&nbsp;за информацией о наличии заказа для оплаты. При отсутствии заказа, выдает ошибку. После успешной оплаты отправляет письмо с информацией о заказе.</li>
</ul>

<h2>OrderAndPay</h2>

<p>В данной системе пользователь имеет доступ только к двум ресурсам.</p>

<ol>
	<li><code>POST /order</code>&nbsp;- создает заказ на определенную сумму. Принимает в&nbsp;<code>body</code>&nbsp;-&nbsp;<code>amount</code>,&nbsp;<code>email</code>.</li>
	<li><code>POST /payment</code>&nbsp;- оплачивает по карте сумму за определенный заказ. Принимает в&nbsp;<code>body</code>&nbsp;-&nbsp;<code>card_number</code>, в параметрах -&nbsp;<code>id</code>&nbsp;заказа.</li>
</ol>

<h2>Обязательные условия</h2>

<ul>
	<li>Воспользуйтесь репозиторием <a href="https://classroom.github.com/a/aFYmQDfN" rel="noopener noreferrer nofollow">GitHub Classroom</a> для этого задания.</li>
	<li>В репозитории должен быть&nbsp;<code>README.md</code>&nbsp;с описанием запуска программы и запуска тестов для каждого сервиса.</li>
	<li>Сервис&nbsp;<code>Core</code>&nbsp;должен быть подключен к базе данных и должен реализовать&nbsp;<code>CRUD</code>&nbsp;для&nbsp;<code>order</code>;</li>
	<li>Сервис&nbsp;<code>Order</code>&nbsp;должен принимать&nbsp;<code>http</code>&nbsp;запросы по ресурсу&nbsp;<code>/order</code>&nbsp;на порту 8080.</li>
	<li>Сервис&nbsp;<code>Payment</code>&nbsp;должен принимать&nbsp;<code>http</code>&nbsp;запросы по ресурсу&nbsp;<code>/payment</code>&nbsp;по порту 8081.</li>
	<li><code>Order</code>&nbsp;и&nbsp;<code>Payment</code>&nbsp;сервисы должны валидировать входные данные.</li>
	<li><code>Order</code>&nbsp;и&nbsp;<code>Payment</code>&nbsp;сервисы должны обрабатывать ошибки и выводить соответствующее сообщение и&nbsp;<code>HTTP</code>&nbsp;статус.</li>
	<li><code>Mailing</code>&nbsp;сервис должен отправлять письма. Можно использовать&nbsp;<code>gmail</code>&nbsp;аккаунт для отправки. ВАЖНО: в открытом доступе не должны быть ваши пароли или логины. Добавьте в&nbsp;<code>README</code>&nbsp;инструкцию по добавлению этих данных.</li>
	<li>При успешном создании и оплаты заказа,&nbsp;<code>Mailing</code>&nbsp;сервис должен отправить письмо человеку, оформившему заказ.</li>
	<li>Каждый сервис должен иметь&nbsp;<code>unit</code>&nbsp;тесты.</li>
	<li>Каждый сервис должен быть в отдельной папке и иметь свой&nbsp;<code>main</code>.</li>
</ul>

<pre>
<code>&gt; ls
core mailing payment order README.md
</code></pre>

<p>Полезные ссылки:</p>

<ul>
	<li><a href="https://spring.io/guides/gs/messaging-jms/" rel="nofollow noopener noreferrer">JMS</a></li>
	<li><a href="https://www.rabbitmq.com/getstarted.html" rel="nofollow noopener noreferrer">RabbitMQ tutorials</a></li>
	<li><a href="https://spring.io/guides/gs/messaging-rabbitmq/" rel="nofollow noopener noreferrer">Spring Messaging With RabbitMQ</a></li>
	<li><a href="https://www.baeldung.com/spring-email" rel="nofollow noopener noreferrer">Spring Email</a></li>
	<li><a href="https://www.baeldung.com/spring-scheduled-task" rel="nofollow noopener noreferrer">Scheduled Task With Spring</a></li>
	<li><a href="https://habr.com/ru/company/raiffeisenbank/blog/346380/" rel="nofollow noopener noreferrer">Просто о микросервисах</a></li>
</ul>
