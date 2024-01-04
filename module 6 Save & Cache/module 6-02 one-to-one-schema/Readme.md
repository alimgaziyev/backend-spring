<h1>One To One Schema</h1>

<p>Улучшите свои навыки SQL</p>

<h3>Задача</h3>

<p><code>UserProfilesDbInitializer</code> предоставляет API, которая позволяет создавать (инициализировать) таблицы в базе данных для пользователей, рабочих профилей и их связей. Каждый пользователь может иметь только один профиль. У пользователя может не быть рабочего профиля. Работа инициализатора заключается в создании правильных таблиц базы данных.</p>

<p><code>UserProfilesDbInitializer</code> содержит <em>javadoc</em>, в котором указаны требования к базе данных. <strong>Он уже содержит все необходимые Java реализация.</strong> Ваша задача — <strong>внедрить файл SQL</strong> <code>table_initialization.sql</code>.</p>

<p>Цель задачи — <strong>спроектировать таблицу базы данных в соответствии с документами и соглашением об именах и реализовать ее с помощью SQL</strong>. Это поможет вам <strong>изучить отношения 1 к 1</strong> и узнать, как создать базу данных <strong>наиболее эффективным способом</strong>.</p>

<p>Для проверки решения вы можете запустить тесты в репозитории:</p>

<pre><code>./mvnw clean test
</code></pre>

<h3>Предварительные условия ❗</h3>

<p>Предполагается, что вы знакомы с проектированием баз данных и соглашением об именах, <em>JDBC API</em> и <em>SQL</em>.</p>

<h3>С чего начать ?</h3>

<ul>
	<li>Перейдите в директорию с <a href="https://github.com/jusan-singularity/save-and-cache/tree/master/1-0-rdbms-and-sql/1-2-1-one-to-one-schema" rel="noopener noreferrer nofollow">задачей</a> в своем репозитории</li>
	<li>Откройте файл <code>table_initialization.sql</code> и выполните задание</li>
	<li>Ознакомьтесь с ссылками ниже</li>
</ul>

<h3>Материалы по теме</h3>

<ul>
	<li><a href="https://github.com/bobocode-projects/jdbc-api-tutorial/tree/master/jdbc-basics" rel="noopener noreferrer nofollow">Учебник по основам JDBC API</a></li>
</ul>