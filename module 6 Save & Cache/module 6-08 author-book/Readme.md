<h1>Author Book</h1>

<p>Совершенствуйте свои навыки сопоставления отношений <em>JPA Many-To-Many</em></p>

<h3>Задача</h3>

<p>Отношения между <code>Автором</code> (Author) и <code>Книгой</code> (Book) — многие ко многим. В базе данных это отношение представлено в виде двух таблиц, которые связаны специальной <strong>таблицей ссылок</strong>. Ваша задача состоит в том, чтобы <strong>обеспечить сопоставление для обеих сущностей, следуя примечаниям в разделе <code>todo</code>.</strong></p>

<p>Для проверки решения вы можете запустить тесты в репозитории:</p>

<pre><code>./mvnv clean test
</code></pre>

<h3>Предварительные условия</h3>

<p>Предполагается, что вы знакомы со стратегиями сопоставления <em>JPA</em> и <em>Hibernate ORM</em>.</p>

<h3>С чего начать</h3>

<ul>
	<li>Перейдите в директорию с <a href="https://github.com/jusan-singularity/save-and-cache/tree/master/3-0-jpa-and-hibernate/3-1-3-author-book" rel="noopener noreferrer nofollow">задачей</a> в своем репозитории</li>
	<li>Просто начните реализовывать раздел <strong>todo</strong>, проверьте свои изменения, запустив тесты</li>
	<li>Ознакомьтесь со ссылками ниже или загуглите интересующий материал</li>
</ul>

<h3>Материалы по теме ℹ️</h3>

<ul>
	<li><a href="https://github.com/boy4uck/jpa-hibernate-tutorial/tree/master/jpa-hibernate-basics" rel="noopener noreferrer nofollow">Материалы по основам JPA и Hibernate</a></li>
	<li><a href="http://docs.jboss.org/hibernate/orm/5.3/userguide/html_single/Hibernate_User_Guide.html#associations-many-to-many" rel="nofollow noopener noreferrer">@ManyToMany</a></li>
	<li><a href="https://vladmihalcea.com/how-to-implement-equals-and-hashcode-using-the-jpa-entity-identifier/" rel="nofollow noopener noreferrer">Как реализовать equals и hashCode с использованием идентификатора объекта JPA</a></li>
	<li><a href="https://vladmihalcea.com/the-best-way-to-use-the-manytomany-annotation-with-jpa-and-hibernate/" rel="nofollow noopener noreferrer">The best way to use the @ManyToMany annotation with JPA and Hibernate</a></li>
</ul>