<p>В данной главе мы познакомимся с основами синтаксиса <code>sql</code> на примере работы с базой данных <code>sqlite</code>.</p>

<p>Она хорошо подходит для целей обучения благодаря тому, что для ее запуска достаточно утилиты <code>sqlite3</code>. Все данные будут храниться в одном файле.</p>

<p>Давайте воспользуемся данной утилитой и создадим нашу первую таблицу:</p>

<pre><code>$ sqlite3 store.db
SQLite version 3.36.0 2021-06-18 18:58:49
Enter ".help" for usage hints.
sqlite&gt;</code></pre>

<p><code>sqlite3</code> - это утилита для выполнения <code>sql</code> команд. <code>store.db</code> - это база данных, которая представлена в виде одного файла.</p>

<p>Запустив программу, мы видим сообщение приветствия с информацией о версии. <code>sqlite</code> ожидает наших действий. Мы находимся в среде для выполнения <code>sql</code> команд.</p>

<h2>CREATE</h2>

<p>Давайте создадим первую таблицу:</p>

<pre><code>sqlite&gt; create table users (id int, name text);
sqlite&gt; .tables
users</code></pre>

<p>При создании, мы всегда указываем название колонки и ее тип. В различных базах данных могут присутствовать разные типы, но примитивные типы есть во всех реализациях.</p>

<p>Давайте рассмотрим на примере <code>sqlite</code>.</p>

<p>Если мы хотим хранить строки мы можем использовать <code>TEXT</code>. В зависимости от максимальной длины, мы можем использовать <code>varchar()</code>.</p>

<p>Если мы хотим хранить числовые данные мы можем использовать <code>INTEGER</code>. Для хранения чисел с плавающей точкой можно использовать <code>REAL</code>.</p>

<h2>INSERT</h2>

<pre><code>INSERT INTO users VALUES (1,'John');</code></pre>

<p>Для добавления используется ключевые слова <code>INSERT INTO</code>, за которым следует название таблицы и дальше значения.</p>

<pre><code>INSERT INTO users (id,name) VALUES (2,'Dee');</code></pre>

<p>Мы можем указывать только значения которые нужно добавить, значение которые не присутствуют будут равны <code>null</code>.</p>

<h2>SELECT</h2>

<pre><code>SELECT * FROM users;</code></pre>

<p>Для получения данных из таблицы используется ключевое слово <code>SELECT</code> за которым следуют необходимые названия столбцов и далее ключевое слово <code>FROM</code> за которым следует название таблицы.</p>

<p><code>*</code> - означает получить все столбцы из таблицы.</p>

<pre><code>SELECT name FROM users;</code></pre>

<h3>WHERE</h3>

<p>Мы можем указывать фильтры для запроса в которых могут использоваться операции сравнения:</p>

<pre><code>SELECT * FROM users WHERE id &gt; 1;</code></pre>

<p>Данный запрос вернет список <code>users</code>, где поле <code>id</code> больше 1.</p>

<p>Фильтр <code>WHERE</code> может иметь более одного условия.</p>

<pre><code>SELECT * FROM users WHERE id &gt;= 1 and id &lt;= 10;</code></pre>

<pre><code>SELECT * FROM users where id &gt;= 1 or name = 'John';</code></pre>

<p>Операции <code>and</code> и <code>or</code> работают так же как и в программировании. При <code>and</code> все условия должны быть <code>true</code>. При <code>or</code> достаточно одного условия, результат которого будет <code>true</code>.</p>

<h3>ORDER</h3>

<p>Мы можем задавать поля для сортировки и может указывать лимиты выводимого результата.</p>

<pre><code>SELECT * FROM users ORDER BY id LIMIT 2;</code></pre>

<h2>UPDATE</h2>

<pre><code>UPDATE users SET name = 'Helena' WHERE id = 1;</code></pre>

<p>При обновлении данных в таблице используется ключевое слово <code>UPDATE</code> за которым следует название таблицы, операция которую нужно выполнить <code>name = 'Helena'</code> и условие при котором нужно обновить поле в ряду.</p>

<pre><code>UPDATE users SET name = 'Boom', id = 222;</code></pre>

<p>Обязательны только название таблицы и <code>SET</code>.</p>

<h2>DELETE</h2>

<pre><code>DELETE FROM users WHERE id = 1;</code></pre>

<p>При удалении данных используется ключевое слово <code>DELETE</code> за которым следует название таблицы. Можно использовать фильтры.</p>

<p>Для удаления таблицы можно использовать команду <code>drop &lt;table_name&gt;</code>.</p>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://www.sqlite.org/datatype3.html" rel="nofollow noopener noreferrer">Sqlite Data Types</a></li>
	<li><a href="https://www.postgresql.org/docs/current/datatype.html" rel="nofollow noopener noreferrer">Postgres Data Types</a></li>
	<li><a href="https://proglib.io/p/sql-for-20-minutes" rel="nofollow noopener noreferrer">SQL for 20 minutes</a></li>
	<li><a href="https://www.postgresql.org/docs/current/sql-select.html" rel="nofollow noopener noreferrer">Postgres SELECT</a></li>
	<li><a href="https://www.postgresql.org/docs/14/sql-update.html" rel="nofollow noopener noreferrer">Postgres Update</a></li>
	<li><a href="https://www.postgresql.org/docs/14/sql-delete.html" rel="nofollow noopener noreferrer">Postgres Delete</a></li>
</ul>