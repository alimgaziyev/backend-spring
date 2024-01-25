<h1>rabbitmq-install</h1>

<h3>Полезное</h3>

<ul>
	<li><a href="https://www.rabbitmq.com/download.html" rel="nofollow noopener noreferrer">Download RabbitMQ</a></li>
</ul>

<h3>Задание</h3>

<ol>
	<li>Установить и запустить на удаленном сервере stepik наиболее актуальную версию <code>RabbitMQ</code>.</li>
	<li>Активировать плагин <code>rabbitmq_management</code></li>
</ol>




<h1>rabbitmq-users</h1>

<h3>Задание</h3>

<p>На сервере установлен RabbitMQ. В этом задании нужно создать пользователей и вхосты.</p>

<ol>
	<li>Создать пользователя <code>administrator</code> с паролем <code>Lhc9kkVV</code>.</li>
	<li>Создать пользователя <code>app-developer</code> с паролем <code>9cDtdMTj</code>.</li>
	<li>Поменять пароль пользователю <code>guest</code> на <code>GmBtuC5X</code></li>
	<li>Создать vhost <code>application</code> и дать полные права пользователям <code>administrator</code> и <code>app-developer</code>.</li>
</ol>





<h1>rabbitmq-uri</h1>

<h3>Полезное</h3>

<ul>
	<li><a href="https://www.rabbitmq.com/uri-spec.html" rel="nofollow noopener noreferrer">RabbitMQ URI Specification</a></li>
</ul>

<h3>Задание</h3>

<ol>
	<li>Для успешного прохождения данного задания найдите и отправьте RabbitMQ URI для подключения под пользователем <code>app-developer</code> из прошлого задания на vhost <code>application</code>. Хост использовать <code>localhost</code>.</li>
</ol>




<h1>rabbitmq-cluster</h1>

<h3>Полезное</h3>

<ul>
	<li><a href="https://www.rabbitmq.com/clustering.html" rel="nofollow noopener noreferrer">RabbitMQ Clustering</a></li>
</ul>

<h3>Задание</h3>

<ol>
	<li>
	<p>Пройти по ссылке задания в GitHub Classroom</p>

	<p>Задание выполняется в GitHub Classroom. После выполнения пришлите свою ссылку на рецензию ревьюеру в Stepik (<a href="https://classroom.github.com/a/C4kjnm4l" rel="noopener noreferrer nofollow">GitHub Classroom</a>).</p>
	</li>
	<li>Создайте скрипт <code>setup.sh</code>, который:
	<ul>
		<li>создаст кластер RabbitMQ из 2 контейнеров</li>
		<li>каждый RabbitMQ контейнер использует образ <code>rabbitmq:3.9-management</code>.</li>
		<li>назовите каждый контейнер <code>rabbit-1</code>, <code>rabbit-2</code></li>
		<li><code>rabbit-1</code> имеет порты <code>5672:5672</code>, <code>15672:15672</code>.</li>
		<li><code>rabbit-2</code> имеет порты <code>5673:5672</code>, <code>15673:15672</code>.</li>
		<li>объедините <code>rabbit-2</code> в один кластер с <code>rabbit-1</code></li>
	</ul>
	</li>
	<li>Запушить все необходимые файлы в репо <code>jusan-rabbitmq</code>.</li>
</ol>