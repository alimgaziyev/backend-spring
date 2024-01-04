<h1>Many To Many Schema</h1>

<p>Улучшите свои навыки проектирования базы данных и навыки SQL</p>

<h3>Задача</h3>

<p><code>WallStreetDbInitializer</code>&nbsp;предоставляет API, который позволяет создавать (инициализировать) таблицы базы данных для хранения информации о брокере, продажах и их отношениях. Грубо говоря, каждая группа продаж состоит из нескольких брокеров, и каждый брокер может быть связан более чем с одной группой продаж. Работа инициализатора заключается в создании правильных таблиц базы данных.</p>

<p><code>WallStreetDbInitializer</code>&nbsp;содержит&nbsp;<em>javadoc</em>, в котором указаны требования к базе данных.&nbsp;<strong>Он уже содержит все необходимые Java реализации.</strong>&nbsp;Ваша задача &mdash;&nbsp;<strong>внедрить файл SQL</strong>&nbsp;<code>table_initialization.sql</code>.</p>

<p>Цель задачи &mdash;&nbsp;<strong>спроектировать таблицу базы данных в соответствии с документами и соглашением об именах и реализовать ее с помощью SQL</strong>.</p>

<p>Для проверки решения вы можете запустить тесты в репозитории:</p>

<pre>
<code>./mvnv clean test
</code></pre>

<h3>Предварительные условия !</h3>

<p>Предполагается, что вы знакомы с проектированием баз данных и соглашением об именах,&nbsp;<em>JDBC API</em>&nbsp;и&nbsp;<em>SQL</em>.</p>

<h3>С чего начать ?</h3>

<ul>
	<li>Перейдите в директорию с&nbsp;<a href="https://github.com/jusan-singularity/save-and-cache/tree/master/1-0-rdbms-and-sql/1-2-3-many-to-many-schema" rel="noopener noreferrer nofollow">задачей</a>&nbsp;в своем репозитории</li>
	<li>Откройте файл&nbsp;<code>table_initialization.sql</code>&nbsp;и выполните задание</li>
	<li>Ознакомьтесь с ссылками ниже</li>
</ul>

<h3>Материалы по теме</h3>

<ul>
	<li><a href="https://github.com/bobocode-projects/jdbc-api-tutorial/tree/master/jdbc-basics" rel="noopener noreferrer nofollow">Учебник по основам JDBC API</a></li>
</ul>
