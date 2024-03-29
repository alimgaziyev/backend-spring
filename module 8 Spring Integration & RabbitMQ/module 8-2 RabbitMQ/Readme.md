<h1>rabbitmq-init</h1>

<p>RabbitMQ — это брокер сообщений, который можно использовать, чтобы позволить разным сервисам обмениваться данными через общий протокол. RabbitMQ может работать как очередь заданий, который отправляет таски распределенным исполнителям.</p>

<p>Очередь сообщений (англ. "message queuing") — это метод коммуникации между разными сервисами. Благодаря очередям сообщений сервисы могут оставаться полностью независимыми при выполнении своих индивидуальных задач. Сообщения обычно представляют собой небольшие запросы, ответы, обновления статуса или даже небольшую информацию. Очередь сообщений предоставляет временное место для хранения этих сообщений, позволяя приложениям отправлять и получать их по мере необходимости.</p>

<p>Сообщения идут в одном направлении, от издателя (англ. "publisher") к брокеру и, наконец, к потребителю (англ. "consumer"):</p>

<p style="text-align: center;"><img alt="" height="111" name="image.png" src="https://ucarecdn.com/5fd8194f-1d0b-4f11-a784-67e3109e8c05/" width="332"></p>

<p>Издатель публикует сообщение брокеру, который доставит данные потребителю. Если требуется ответ, он придет через некоторое время через тот же механизм, но в обратном порядке (роли потребителя и издателя поменяются местами).</p>

<p>Следующая диаграмма иллюстрирует слабую зависимость между издателем и потребителем:</p>

<p style="text-align: center;"><img alt="" height="426" name="image.png" src="https://ucarecdn.com/9040c8c7-9de7-4626-8a8a-c7052f44a8c4/" width="602"></p>

<p>Если один из сервисов не будет работать, другая часть системы будет независимо работать, и сообщения, которые должны быть отправлены между ними, будут ждать в очереди.</p>

<p>Архитектура, представленная через очередь сообщений, позволяет следующее:</p>

<ul>
	<li>Издатели или потребители могут обновляться один за другим, не влияя друг на друга.</li>
	<li>Производительность каждой стороны оставляет другую сторону незатронутой.</li>
	<li>Издатели или потребители могут выходить из строя, не влияя друг на друга.</li>
	<li>Количество реплик издателей и потребителей может независимо масштабироваться.</li>
	<li>Потребители и издатели могут быть реализованы на разных технологиях.</li>
</ul>

<h2>AMQP</h2>

<p>AMQP — это открытый стандартный протокол, который определяет, как система может обмениваться сообщениями. Протокол определяет набор правил, которым должны следовать сервисы, которые будут взаимодействовать друг с другом.</p>

<p>Ниже приведен список основных концепций AMQP:</p>

<ul>
	<li><strong>Broker</strong>. Брокер — это часть программного обеспечения, которая получает сообщения от одной службы и доставляет их другой службе.</li>
	<li><strong>Connection</strong>. Физическое сетевое (TCP) соединение между приложением (издатель/потребитель) и брокером. Когда клиент отключается или происходит системный сбой, соединение закрывается.</li>
	<li><strong>Channel</strong>. Канал — это виртуальное соединение внутри соединения. Он переиспользует соединение, без необходимости повторной авторизации и открытия нового TCP соединения. Когда сообщения публикуются или потребляются, это делается по каналу. В рамках одного соединения может быть установлено множество каналов.</li>
	<li><strong>Exchange</strong>. Данный объект отвечает за применение правил маршрутизации для сообщений, гарантируя, что сообщения достигают своего конечного пункта назначения. В какой очереди окажется сообщение, зависит от правил. Правила маршрутизации могут быть прямой (точка-точка), тематической (публикация-подписка), разветвленнойй (multicast).</li>
	<li><strong>Queue</strong>. Очередь представляет собой последовательность элементов; в данном случае сообщения. Очередь существует в брокере.</li>
	<li><strong>Binding</strong>. Привязка — это виртуальная связь между обменом и очередью в брокере. Он позволяет сообщениям перетекать из брокера в очередь.</li>
	<li><strong>Виртуальный хост, vhost</strong>. В брокере существует виртуальный хост. Это способ разделения пространств под приложения, которые используют один и тот же инстанс RabbitMQ. Пользователи, биржи, очереди и т.д. изолированы на одном конкретном виртуальном хосте. Пользователь, подключенный к определенному виртуальному хосту, не может получить доступ в другой виртуальный хост. Пользователи могут иметь разные права доступа к разным виртуальным хостам.</li>
</ul>

<h2>RabbitMQ</h2>

<p>RabbitMQ — это реализация брокера AMQP на языке Erlang. Erlang был выбран из-за встроенной поддержки создания высоконадежных и распределенных приложений. Erlang используется в телекоммуникационных коммутаторах, и известно, что общая доступность системы составляет девять девяток (это всего 32 миллисекунды простоя в год). Erlang может быть запущен в любой операционной системе.</p>

<p>Для надежности данных RabbitMQ полагается на Mnesia, встроенную базу данных Erlang, который хранит в памяти/файле. Mnesia хранит информацию о пользователях, очередях, привязках и так далее. Индекс очереди хранит позиции сообщения и информацию о том, было ли сообщение доставлено или нет.</p>

<p>Для кластеризации в основном полагается на возможности кластеризации Erlang. RabbitMQ можно настроить на одном автономном экземпляре или в виде кластера на нескольких серверах.</p>

<p>Брокеры RabbitMQ можно соединять вместе с помощью различных методов, таких как федерация, для формирования обмена сообщениями с интеллектуальной маршрутизацией сообщений между брокерами и возможностью охвата нескольких дата-центров.</p>

<h2>RabbitMQ в реальной жизни</h2>

<p>Наиболее распространенный вариант использования RabbitMQ — это издатель и очередь с потребителем. Это можно представить, как канал, где одно приложение помещает сообщения в канал, а другое приложение читает сообщения из этого канала. Сообщения доставляются в порядке поступления.</p>

<p>Очереди сообщений часто используются между микросервисами.</p>

<p>Архитектурный стиль микросервисов делит приложение на небольшие сервисы, а целое приложение представляет собой сумму этих микросервисов. Сервисы не связаны напрямую друг с другом. Вместо этого они используют очереди сообщений. Одна служба асинхронно отправляет сообщения в очередь, и эти сообщения доставляются в нужное место назначения, когда потребитель готов.</p>

<h2>События и таски</h2>

<p>События — это уведомления, сообщающие сервисам, когда что-то произошло. Сервис может подписываться на события другого сервиса и реагировать, создавая и обрабатывая таски. Типичный вариант использования — это когда RabbitMQ действует как очередь задач, обрабатывающая долгие и сложные операции.</p>

<p>Примеры:</p>

<ul>
	<li>
	<p>Представьте социальную сеть как Instagram. Каждый раз, когда кто-то публикует новый пост, подписчики должны быть уведомлены о новом посте. Миллионы людей могут одновременно публиковать новый пост. С помощью очередей сообщений можно поставить задачу в очередь для каждого события по мере поступления. Когда консьюмер получает запрос, он извлекает список подписчиков отправителя и уведомляет каждого.</p>
	</li>
	<li>
	<p>Другой пример, инструмент рассылки по электронной почте, который рассылает тысячи писем тысячам пользователей. Может быть такое, что многие пользователи запускают массовые сообщения одновременно. Инструмент рассылки новостей по электронной почте должен быть в состоянии обрабатывать такой объем сообщений. Каждое письмо может быть добавлено в очередь для консьюмера с информацией что отправлять и кому. Каждое электронное письмо обрабатывается одно за другим, пока все электронные письма не будут отправлены.</p>
	</li>
</ul>

<h2>Преимущества очереди сообщений</h2>

<p>Коммуникация между различными службами играет важную роль в распределенных системах. Существует много примеров использования очереди сообщений, поэтому выделим некоторые функции и преимущества очереди сообщений в микросервисных архитектурах:</p>

<ul>
	<li><strong>Упрощение разработки и поддержки</strong>: разделение приложения на микросервисы дает разработчикам свободу писать код для конкретного сервиса на любом языке. Такой код проще поддерживать и обновлять;</li>
	<li><strong>Изоляция неисправности</strong>: неисправность может быть локализована и, таким образом, не повлияет на другие сервисы.</li>
	<li><strong>Повышение продуктивности</strong>: разные разработчики могут работать над разными микросервисами одновременно. Каждую службу можно протестировать отдельно, чтобы определить готовность всей системы.</li>
	<li><strong>Улучшенная масштабируемость</strong>: микросервисы также позволяют легко масштабировать по необходимости. Можно добавить больше потребителей, если очередь сообщений растет. Добавление новых компонентов только к одной службе легко сделать без изменения какой-либо другой службы.</li>
	<li><strong>Простота понимания</strong>: поскольку каждый модуль в микросервисной архитектуре представляет собой единую функциональность, ознакомиться с соответствующими деталями задачи несложно.</li>
</ul>





<h1>rabbitmq-setup</h1>

<h2>Установка RabbitMQ</h2>

<p>Установку RabbitMQ нужно производить из офф.репозитория разработчика RabbitMQ.</p>

<p>Инструкции по установке доступны по <a href="https://www.rabbitmq.com/download.html" rel="nofollow noopener noreferrer">ссылке</a>.</p>

<p>Также, для локального тестирования можно запустить в <a href="https://hub.docker.com/_/rabbitmq" rel="nofollow noopener noreferrer">контейнере</a>.</p>

<h2>Установка админ панели Management (Web UI)</h2>

<p>RabbitMQ по умолчанию не устанавливает менеджмент консоль. Менеджмент консоль позволяет легко заглянуть в работающий экземпляр RabbitMQ и управлять ею. RabbitMQ поставляется вместе с CLI-приложением - <code>rabbitmq-plugins</code> для установки и удаления дополнительных плагинов. Для того чтобы активировать mangement консоль:</p>

<pre><code>sudo rabbitmq-plugins enable rabbitmq_management</code></pre>

<p>Откройте страницу админ панели <a href="http://localhost:15672" rel="nofollow noopener noreferrer">localhost:15672</a> - вы должны увидеть следующее:</p>

<p style="text-align: center;"><img alt="" height="306" name="image.png" src="https://ucarecdn.com/e82508c2-dd84-493e-b000-bb392dc4d164/" width="545"></p>

<h2>Управление пользователями</h2>

<p>Одним из встроенных CLI-приложений RabbitMQ является <code>rabbitmqctl</code>, который представляет собой инструмент для управления инстансами RabbitMQ и используется для настройки всех аспектов брокера. Для того чтобы настроить администратора в RabbitMQ, введите следующее:</p>

<pre><code>$ sudo rabbitmqctl add_user admin password123
Adding user "admin" ...
$ sudo rabbitmqctl set_user_tags admin administrator
Setting tags for user "admin" to [administrator] ...</code></pre>

<p>По умолчанию в RabbitMQ создается пользователь <code>guest</code> с паролем <code>guest</code>. Измените этот пароль на другой, как показано ниже:</p>

<pre><code>sudo rabbitmqctl change_password guest guest123</code></pre>

<p>Откройте страницу админ панели <a href="http://localhost:15672" rel="nofollow noopener noreferrer">localhost:15672</a> и залогиньтесь как <code>admin</code>.</p>

<p>В данный момент пользователь <code>admin</code> не имеет доступ к очередям сообщений на любом виртуальном хосте. Необходимо создать еще одного пользователя в целях разработки, чтобы этот пользователь мог подключаться к RabbitMQ.</p>

<p>Создадим пользователя <code>developer</code> с паролем <code>passwordDeveloper</code>:</p>

<pre><code>$ sudo rabbitmqctl add_user developer passwordDeveloper
Adding user "developer" ...</code></pre>

<p>Как обсуждалось ранее, RabbitMQ поддерживает понятие <code>vhosts</code>, где разные пользователи могут иметь разные права доступа.</p>

<p>Создадим виртуальный хост <code>dev-vhost</code>:</p>

<pre><code>$ sudo rabbitmqctl add_vhost dev-vhost
Adding vhost "dev-vhost" ...</code></pre>

<h2> </h2>

<h2>Настройка виртуальных хостов</h2>

<p>RabbitMQ имеет <code>vhost</code> по умолчанию, который называется <code>/</code>. У пользователя <code>guest</code> есть полные права на этот vhost. Хотя это удобно для быстрых тестов, рекомендуется создать отдельный виртуальный хост, чтобы запустить его с нуля без непредвиденных проблем.</p>

<p>На текущий момент пользователи <code>admin</code> и <code>developer</code> не имеют права делать что-либо на <code>dev-vhost</code>. Чтобы исправить это, предоставим полные права на него следующим образом:</p>

<pre><code>$ sudo rabbitmqctl set_permissions -p dev-vhost admin ".\*" ".\*" ".\*"
Setting permissions for user "admin" in vhost "dev-vhost" ...</code></pre>

<pre><code>$ sudo rabbitmqctl set_permissions -p dev-vhost developer ".\*" ".\*" ".\*"
Setting permissions for user "developer" in vhost "dev-vhost" ...</code></pre>

<p>Объясним, что только что было сделано: большая часть команды проста, но часть <code>.\*</code>, <code>.\*</code>, <code>.\*</code> выглядит немного загадочно, так что проанализируем ее.</p>

<p>Это триплет разрешений для виртуального хоста, который предоставляет разрешения на</p>

<ul>
	<li>настройку,</li>
	<li>запись</li>
	<li>чтение</li>
</ul>

<p>для конкретного пользователя и виртуального хоста.</p>



<h1>rabbitmq-uri</h1>

<h3>Полезное</h3>

<ul>
	<li><a href="https://www.rabbitmq.com/uri-spec.html" rel="nofollow noopener noreferrer">RabbitMQ URI Specification</a></li>
</ul>

<h3>Задание</h3>

<ol>
	<li>Для успешного прохождения данного задания найдите и отправьте RabbitMQ URI для подключения под пользователем <code>app-developer</code> из прошлого задания на vhost <code>application</code>. Хост использовать <code>localhost</code>.</li>
</ol>