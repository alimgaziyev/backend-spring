<p>При решении задач в которых важно сохранение данных используют базу данных.</p>

<p>База данных в программировании представляет собой набор данных с определенной структурой, задаваемой разработчиками.</p>

<p>Существует множество типов баз данных, более подробно о них вы можете познакомиться в данной&nbsp;<a href="https://proglib.io/p/11-tipov-sovremennyh-baz-dannyh-kratkie-opisaniya-shemy-i-primery-bd-2020-01-07" rel="nofollow noopener noreferrer">статье</a>.</p>

<p>В данном модуле мы познакомимся с реляционными базами данных:&nbsp;<code>Sqlite</code>&nbsp;и&nbsp;<code>Postgres</code>.</p>

<p>Реляционная база данных - состоит из таблиц, где каждый столбец имеет свое название и тип, каждый ряд описывает значение для столбца. Значения в таких таблицах могут иметь связи с другими таблицами.</p>

<p>Таблица&nbsp;<code>Customer</code>:</p>

<table>
	<thead>
		<tr>
			<th>CustomerId</th>
			<th>FirstName</th>
			<th>LastName</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>John</td>
			<td>Doe</td>
		</tr>
		<tr>
			<td>2</td>
			<td>Doflamingo</td>
			<td>Donquixote</td>
		</tr>
	</tbody>
</table>

<p>Таблица&nbsp;<code>Invoice</code>:</p>

<table>
	<thead>
		<tr>
			<th>InvoiceId</th>
			<th>CustomerId</th>
			<th>InvoiceDate</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>1</td>
			<td>Doe</td>
		</tr>
		<tr>
			<td>2</td>
			<td>2</td>
			<td>Donquixote</td>
		</tr>
		<tr>
			<td>3</td>
			<td>2</td>
			<td>Donquixote</td>
		</tr>
	</tbody>
</table>

<p>В данном примере, таблица&nbsp;<code>Customer</code>&nbsp;хранит клиентов (имена и фамилии). Для уникальности каждого ряда используется колонка&nbsp;<code>CustomerId</code>. Таблица&nbsp;<code>Invoice</code>&nbsp;хранит информацию о счет-фактурах - уникальный&nbsp;<code>id</code>, кому принадлежит данная счет-фактура и когда она была создана.</p>

<p>Для взаимодействия с Реляционными базами данных используется язык структурированных запросов -&nbsp;<code>SQL</code>.</p>

<p>SQL &mdash; это язык программирования, используемый в большинстве реляционных баз данных для запросов, обработки и определения данных, а также контроля доступа.</p>

<p>К основным операциям, которые позволяют манипулировать данными относятся:</p>

<ul>
	<li>Создание таблиц (create);</li>
	<li>Изменение данных в таблицах (update);</li>
	<li>Получение данных из таблиц (select);</li>
	<li>Удаление данных из таблиц (delete);</li>
</ul>

<p>В этом модуле мы будем практиковаться с&nbsp;<code>SQL</code>&nbsp;и применим полученные знания c&nbsp;<code>Spring Data</code>.</p>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://www.oracle.com/ru/database/what-is-database/" rel="nofollow noopener noreferrer">Что такое база данных</a></li>
</ul>
