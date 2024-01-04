<h1>Employee Profile</h1>

<p>Совершенствуйте свои навыки <code>map</code>инга отношений <em>JPA One-To-One</em></p>

<h3>Задача</h3>

<p><code>Employee.java</code> и <code>EmployeeProfile.java</code> являются связанными объектами <code>JPA</code>. Отношения между этими двумя сущностями <strong>один к одному</strong>. У каждого сотрудника может быть только один профиль, при этом каждый профиль всегда связан с одним сотрудником. <strong>Профиль не может существовать отдельно без сотрудника, в то время как сотрудник может существовать без связанного с ним профиля.</strong></p>

<p>Ваша задача состоит в том, чтобы <strong>внедрить однонаправленное сопоставление</strong> между этими сущностями, используя <strong>производный идентификатор</strong> (общий первичный ключ для обоих объектов).</p>

<p>Для проверки решения вы можете запустить тесты в репозитории:</p>

<pre><code>./mvnv clean test
</code></pre>

<h3>Предварительные условия</h3>

<p>Предполагается, что вы знакомы со стратегиями сопоставления <em>JPA</em> и <em>Hibernate ORM</em>.</p>

<h3>С чего начать</h3>

<ul>
	<li>Перейдите в директорию с <a href="https://github.com/jusan-singularity/save-and-cache/tree/master/3-0-jpa-and-hibernate/3-1-1-employee-profile" rel="noopener noreferrer nofollow">задачей</a> в своем репозитории</li>
	<li>Просто начните реализовывать раздел <strong>todo</strong>, проверьте свои изменения, запустив тесты</li>
	<li>Ознакомьтесь с ссылками ниже или загуглите интересующий материал</li>
</ul>

<h3>Материалы по теме ℹ️</h3>

<ul>
	<li><a href="https://github.com/boy4uck/jpa-hibernate-tutorial/tree/master/jpa-hibernate-basics" rel="noopener noreferrer nofollow">Материалы по основам JPA и Hibernate</a></li>
	<li><a href="http://docs.jboss.org/hibernate/orm/5.3/userguide/html_single/Hibernate_User_Guide.html#identifiers-derived" rel="nofollow noopener noreferrer">Производные идентификаторы</a></li>
	<li><a href="https://vladmihalcea.com/the-best-way-to-map-a-onetoone-relationship-with-jpa-and-hibernate/" rel="nofollow noopener noreferrer">Лучший способ сопоставить отношения @OneToOne с JPA и Hibernate</a></li>
</ul>