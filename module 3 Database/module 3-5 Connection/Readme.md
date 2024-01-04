<h1>Connection</h1>

<p>Мы изучили основы работы с базами данных, теперь давайте научимся подключать наши приложения на <code>Java</code> к базам данных.</p>

<p>Процесс работы базы данных можно представлять отдельной программой, у которой открыт порт для подключения. Приложения подключаются по этому порту и в дальнейшем отправляют <code>SQL</code> запросы в базу данных. (Например, в <code>sqlite</code> это обычный файл)</p>

<p>Большинство программ Java используют реляционные базы данных (MySQL, PostgreSQL).</p>

<p>Подключение приложения к базе это не самый простой процесс. Программа должна постоянно учитывать пул соединений, проверять уровень доступа к данным и правильно привязывать <code>Java</code> объекты к таблицам в базе данных.</p>

<p>К счастью, на сегодняшний день уже существует множество готовых библиотек, которые реализовывают все выше сказанные аспекты.</p>

<p>В этом модуле мы познакомимся с одной из реализаций - <code>Spring Data JDBC</code></p>

<p>Прежде чем переходить на <code>Spring</code>, давайте разберемся с <code>JDBC</code>:</p>

<p><img alt="" height="328" name="image.png" src="https://ucarecdn.com/dc14197c-4345-404b-a338-fd904b855e3c/" width="400"></p>

<p><code>JDBC API</code> - это интерфейс, через который наш код взаимодействует с базой данных. <code>JDBC</code> - это стандарт, который описывает способ соединения с различными видами баз данных (MySQL, Postgres и другие). <code>MySQL JDBC Driver</code> - это реализация стандарта <code>JDBC</code> для базы данных <code>MySQL</code>.</p>

<p><code>JDBC API</code> позволяет программам создавать подключения и выполнять запросы. С реализацией можно познакомиться по данной <a href="https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/" rel="nofollow noopener noreferrer">ссылке</a>.</p>

<pre><code>public void connectToAndQueryDatabase(String username, String password) {

    Connection con = DriverManager.getConnection(
                         "jdbc:myDriver:myDatabase",
                         username,
                         password);

    Statement stmt = con.createStatement();
    ResultSet rs = stmt.executeQuery("SELECT a, b, c FROM Table1");

    while (rs.next()) {
        int x = rs.getInt("a");
        String s = rs.getString("b");
        float f = rs.getFloat("c");
    }
}</code></pre>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.oracle.com/javase/tutorial/jdbc/basics/index.html" rel="nofollow noopener noreferrer">JDBC Basics</a></li>
</ul>