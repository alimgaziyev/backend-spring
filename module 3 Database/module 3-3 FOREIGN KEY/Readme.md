<p>При проектировании базы данных разработчик должен представлять всю картину проекта.</p>

<p>Например: мы разрабатываем интернет магазин по продаже музыкальных дисков.</p>

<p>Основными участниками системы являются: Покупатели, Продавцы, Песни.</p>

<p>При регистрации покупателя к нему прикрепляется продавец, а при покупке (состоящей из более чем одной песни) выставляется счет-фактура.</p>

<p>Конечно можно все это держать в одной таблице, но тогда это создаст проблемы при выборке, удалению, обновлению данных.</p>

<p>Поэтому, будет правильно разбить все по разным таблицам, которые будут связаны внешними ключами&nbsp;<code>Foreign Key</code>.</p>

<p>Существует несколько типов данных связей.</p>

<p><img alt="" height="645" name="image.png" src="https://ucarecdn.com/8d14f996-13e2-44f6-be18-287446bff399/" width="624" /></p>

<ol>
	<li><code>one to one</code>&nbsp;- это значит, что объект таблицы A соотносится только с одним объектом таблицы B, и наоборот. Иными словами - их связь уникальна. Например таблица&nbsp;<code>CarModel</code>&nbsp;имеет внешний ключ на ряд в таблице&nbsp;<code>CarCompany</code>.</li>
	<li><code>one to many</code>&nbsp;- один объект таблицы A может соотноситься с несколькими объектами таблицы B. У каждого пациента в таблице&nbsp;<code>Patient</code>&nbsp;есть внешний ключ на таблицу&nbsp;<code>Doctor</code>. Эти внешние ключи могут повторяться.</li>
	<li><code>many to many</code>&nbsp;- для реализации данного типа обычно используют новую таблицу состоящей из внешних ключей на таблицу&nbsp;<code>Student</code>&nbsp;и&nbsp;<code>Subject</code>.</li>
</ol>

<p>Создание связи можно выполнить при создании таблицы и при ее обновлении.</p>

<p>Рассмотрим пример. У нас есть таблица&nbsp;<code>track</code>&nbsp;которая должна хранить информацию об артисте.</p>

<p>Создание таблицы артистов будет выглядеть так.</p>

<pre>
<code>CREATE TABLE artist(
  artistid    INTEGER PRIMARY KEY, 
  artistname  TEXT
);</code></pre>

<p><code>PRIMARY KEY</code>&nbsp;- это ключевое слово, которое устанавливает уникальность значение. При попытке добавить в базу второго артиста с&nbsp;<code>artistId</code>&nbsp;который уже есть в таблице мы получим ошибку добавления.</p>

<pre>
<code>sqlite&gt; insert into artist values (1, 'a');
sqlite&gt; insert into artist values (1, 'a');
Error: UNIQUE constraint failed: artist.artistid</code></pre>

<p>Давайте теперь создадим таблицу&nbsp;<code>track</code>&nbsp;в которой будет внешний ключ на таблицу&nbsp;<code>artist</code>:</p>

<pre>
<code>CREATE TABLE track(
  trackid     INTEGER, 
  trackname   TEXT, 
  trackartist INTEGER,
  FOREIGN KEY(trackartist) REFERENCES artist(artistid)
);</code></pre>

<p><code>FOREIGN KEY</code>&nbsp;это ключевое слово для создания связи.</p>

<p>Тем самым, при добавлении нового трека, если&nbsp;<code>trackartist</code>&nbsp;не будет присутствовать в таблице&nbsp;<code>artist</code>&nbsp;мы получим ошибку.</p>

<h2>JOIN</h2>

<p><code>JOIN</code>&nbsp;используется для объединения данных из нескольких объектов в реляционной базе данных.</p>

<p><code>SQL</code>&nbsp;определяет 5 соединений:</p>

<ul>
	<li>Inner Join</li>
	<li>Left Outer Join</li>
	<li>Right Outer Join</li>
	<li>Full Outer Join</li>
	<li>Cross Join</li>
</ul>

<p><img alt="" height="526" name="image.png" src="https://ucarecdn.com/641d06f6-aed2-4930-85d4-f3e3842ce013/" width="669" /></p>

<p>Если мы хотим получить список всех песен разных исполнителей, мы можем воспользоваться конструкцией&nbsp;<code>join</code></p>

<pre>
<code>SELECT * FROM track t JOIN artist a ON t.trackartist = a.artistId;</code></pre>

<h2>Transaction</h2>

<p>В работе с данными возможны случаи, когда нам нужно выполнить несколько операций с изменениями.</p>

<p>Например:</p>

<p>Необходимо обновить статус в таблице счет-фактур как &quot;Исполнено&quot; и обновить таблицу со счетами пользователей.</p>

<pre>
<code>UPDATE Invoice SET completed = true WHERE id = 13;
UPDATE Account SET balance = balance - 20 WHERE id 111;</code></pre>

<p>Нам необходимо чтобы все операции прошли успешно. В случае если одна из операций не удается, ни одна из операций не должна совершиться.</p>

<p>т.е. если операция по обновлению счета не совершится, то и обновление статуса счет-фактуры тоже не должно состояться.</p>

<p>Для этого существуют транзакции. Они оборачивают ваши запросы в отдельный контекст.</p>

<pre>
<code>BEGIN;
UPDATE Invoice SET completed = true WHERE id = 13;
UPDATE Account SET balance = balance - 20 WHERE id 111;
COMMIT;</code></pre>

<h2>Index</h2>

<p>Индексация отвечает за быстрое получение данных. Проиндексированные поля представляют отдельную структуру данных - например&nbsp;<code>Бинарное дерево</code>. Это больше используются с конструкциями&nbsp;<code>WHERE</code>,&nbsp;<code>JOIN</code>. Т.е. с данными, которые должны быстро находиться.</p>

<pre>
<code>CREATE INDEX IFK_AlbumArtistId ON Album (ArtistId);</code></pre>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://www.postgresql.org/docs/14/ddl-constraints.html" rel="nofollow noopener noreferrer">Postgres Constraints</a></li>
	<li><a href="https://www.sqlite.org/foreignkeys.html" rel="nofollow noopener noreferrer">Sqlite Foreign Key</a></li>
	<li><a href="https://www.sqlite.org/lang_transaction.html" rel="nofollow noopener noreferrer">Sqlite Transactions</a></li>
</ul>
