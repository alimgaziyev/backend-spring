<h1>Spring Data JDBC</h1>

<p><code>Spring Data JDBC</code>, часть более крупного семейства <code>Spring Data</code>, упрощает реализацию проектов на основе <code>JDBC</code>.</p>

<p>Этот модуль имеет дело с расширенной поддержкой уровней доступа к данным на основе <code>JDBC</code>.</p>

<p>Это упрощает создание приложений на базе <code>Spring</code>, использующих технологии доступа к базам данных.</p>

<p><code>Spring Data JDBC</code> стремится быть концептуально простым. Чтобы достичь этого, он НЕ предлагает кэширование, отложенную загрузку, отложенную запись или многие другие функции <code>JPA</code> который мы рассмотрим в будущих модулях.</p>

<h2>Функции</h2>

<ul>
	<li>Встроенные интерфейсы, реализующие операции <code>CRUD</code>.</li>
	<li>Поддержка аннотаций <code>@Query</code>.</li>
	<li>События.</li>
	<li>Конфигурация репозитория на основе <code>JavaConfig</code> путем введения <code>@EnableJdbcRepositories</code>.</li>
</ul>

<p>Spring Data JDBC - не имеет реализации <code>lazy</code> загрузки или кэширования результатов в памяти. Данные фичи реализованы в <code>Spring Data JPA</code> которые мы разберем в следующих модулях.</p>

<p>Для создания и подключения базы данных в наше приложение нам необходимы отдельные библиотеки. В первую очередь - это <code>Spring Data JDBC</code>.</p>

<p>Давайте проинициализируем <code>boilerplate</code> проект на этом <a href="https://start.spring.io/" rel="nofollow noopener noreferrer">сайте</a>:</p>

<p><img alt="" height="554" name="image.png" src="https://ucarecdn.com/80cda26c-f109-4c0b-a888-f105e2f9d9ea/" width="877"></p>

<p>Посмотрим <code>pom.xml</code> файл и в разделе с зависимостями увидим модуль, который позволяет нам использовать <code>jdbc</code>:</p>

<pre><code>&lt;dependency&gt;
    &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
    &lt;artifactId&gt;spring-boot-starter-data-jdbc&lt;/artifactId&gt;
&lt;/dependency&gt;</code></pre>

<p>Вместе с этим, нам необходим определенный диалект, по которому мы будем взаимодействовать с базой данных. Это может быть <code>postgres</code>, <code>mysql</code> и <code>другие</code>.</p>

<p>В нашем случае, мы будет тестировать базу через <a href="http://www.h2database.com/html/main.html" rel="nofollow noopener noreferrer"><code>h2</code></a>. Это простой СУБД, он сильно похож на <code>sqlite</code> за исключением того, что с ним можно хранить данные в памяти, а не в файле.</p>

<pre><code>&lt;dependency&gt;
    &lt;groupId&gt;com.h2database&lt;/groupId&gt;
    &lt;artifactId&gt;h2&lt;/artifactId&gt;
    &lt;scope&gt;runtime&lt;/scope&gt;
&lt;/dependency&gt;</code></pre>

<p>Нам также необходим механизм слежения за версиями в базе. На примере <code>h2</code> это будет выглядеть скучно, но при использовании других <code>СУБД</code> утилита за слежением версий позволит сохранять историю изменения схемы базы данных.</p>

<pre><code>&lt;dependency&gt;
    &lt;groupId&gt;org.flywaydb&lt;/groupId&gt;
    &lt;artifactId&gt;flyway-core&lt;/artifactId&gt;
    &lt;version&gt;8.5.7&lt;/version&gt;
&lt;/dependency&gt;</code></pre>

<p><code>Flywaydb</code> - то утилита, которая позволяет обновлять вашу базу на основе созданных разработчиком миграций. Миграция в данном случае это <code>sql</code> файл. Утилита при инициализации создает таблицу, в которой хранит последнюю добавленную версию. В случае добавления новых миграций, утилита проверяет последнюю версию в базе и обновляет таблицы на основе созданных разработчиком <code>sql</code> файлов.</p>

<p>Все миграции должны помещаться в директорию <code>resources/db/migrate</code>. Файлы должны придерживаться формата <code>V{version number}__{description}.sql</code>.</p>

<p>Данные параметры можно изменить. Информацию о них можно получить по данной <a href="https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html" rel="nofollow noopener noreferrer">ссылке</a>.</p>

<h2>Создание миграций</h2>

<p>Создадим нашу первую миграцию в файле <code>resources/db/migration/V1__create_invoice_and_customer_tables.sql</code>:</p>

<p>В нашем примере у нас есть задача получить все счет-фактуры у клиентов.</p>

<pre><code>CREATE TABLE Customer
(
    customer_id INTEGER  NOT NULL,
    first_name NVARCHAR(40)  NOT NULL,
    last_name NVARCHAR(20)  NOT NULL,
    company NVARCHAR(80),
    address NVARCHAR(70),
    city NVARCHAR(40),
    state NVARCHAR(40),
    country NVARCHAR(40),
    postal_code NVARCHAR(10),
    phone NVARCHAR(24),
    fax NVARCHAR(24),
    email NVARCHAR(60)  NOT NULL,
    CONSTRAINT PK_Customer PRIMARY KEY  (customer_id)
);

CREATE TABLE Invoice
(
    invoice_id INTEGER  NOT NULL,
    customer_id INTEGER  NOT NULL,
    invoice_date DATETIME  NOT NULL,
    billing_address NVARCHAR(70),
    billing_city NVARCHAR(40),
    billing_state NVARCHAR(40),
    billing_country NVARCHAR(40),
    billing_postal_code NVARCHAR(10),
    total NUMERIC(10,2)  NOT NULL,
    CONSTRAINT PK_Invoice PRIMARY KEY  (invoice_id),
    FOREIGN KEY (customer_id) REFERENCES Customer (customer_id)
);

INSERT INTO Customer VALUES(1,'Lus','Gonalves','Embraer - Empresa Brasileira de Aeronáutica S.A.','Av. Brigadeiro Faria Lima, 2170','dos Campos','SP','Brazil','12227-000','+55 (12) 3923-5555','+55 (12) 3923-5566','luisg@embraer.com.br');
INSERT INTO Customer VALUES(2, 'a','a','a','Av. Brigadeiro Faria Lima, 2170','dos Campos','SP','Brazil','12227-000','+55 (12) 3923-5555','+55 (12) 3923-5566','luisg@embraer.com.br');

INSERT INTO Invoice VALUES(1,1,'2009-01-01 00:00:00','Theodor-Heuss-Straße 34','Stuttgart',NULL,'Germany','70174',1.980000000000000071);
INSERT INTO Invoice VALUES(2,2,'2010-01-02 00:00:00','s 14','Oslo',NULL,'Norway','0171',3.9599999999999999644);
INSERT INTO Invoice VALUES(3,1,'2009-01-02 00:00:00','Ullevålsveien 14','Oslo',NULL,'Norway','0171',3.9599999999999999644);
INSERT INTO Invoice VALUES(4,1,'2010-01-02 00:00:00','s 14','Oslo',NULL,'Norway','0171',3.9599999999999999644);</code></pre>

<h2>Настройка переменных окружения</h2>

<p>Давайте свяжем наши компоненты <code>h2</code> и <code>flywaydb</code> вместе. Нам необходимо, чтобы <code>flywaydb</code> понимал куда делать миграции</p>

<pre><code>spring.jpa.hibernate.ddl-auto=none # Отключаем автоматическую инициализацию таблицы на основе Java объектов
spring.datasource.url:jdbc:h2:mem:mydatabasename;DB_CLOSE_DELAY=-1; # Указываем ссылку для подключения к бд
spring.datasource.driverClassName:org.h2.Driver # Выбираем драйвер
spring.datasource.username:sa # Указываем логин для подключения к бд
spring.datasource.password: # Указываем пароль для пользователя `sa`
spring.flyway.url:${spring.datasource.url}
spring.flyway.user:${spring.datasource.username}
spring.flyway.password:${spring.datasource.password}
</code></pre>

<p>Мы связали <code>h2</code> и <code>flywaydb</code>. Теперь, при старте <code>flywaydb</code> будет проверять доступные миграции и выполнять их.</p>

<p>Запустив программу, мы увидим, что произошла миграция:</p>

<pre><code>2022-04-11 22:54:38.536  INFO 20855 --- [           main] o.f.core.internal.command.DbMigrate      : Current version of schema "PUBLIC": &lt;&lt; Empty Schema &gt;&gt;
2022-04-11 22:54:38.543  INFO 20855 --- [           main] o.f.core.internal.command.DbMigrate      : Migrating schema "PUBLIC" to version "1 - Initial version"
2022-04-11 22:54:38.550  INFO 20855 --- [           main] o.f.core.internal.command.DbMigrate      : Successfully applied 1 migration to schema "PUBLIC", now at version v1 (execution time 00:00.015s)
</code></pre>

<h2>Написание классов</h2>

<p>Давайте теперь опишем наши классы.</p>

<p><code>Spring Data JDBC</code> использует <code>@Id</code> для идентификации объектов. В представлении <code>sql</code> это может быть <code>AUTO_INCREMENT</code> и <code>PRIMARY_KEY</code>.</p>

<p>Если в вашей базе данных есть столбец с <code>AUTO_INCREMENT</code> для столбца идентификатора, сгенерированное значение устанавливается в объекте после его вставки в базу данных.</p>

<pre><code>// Customer.java

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import org.springframework.data.annotation.Id;
import org.springframework.data.relational.core.mapping.MappedCollection;

import java.util.Set;

@Data
@Builder
@AllArgsConstructor
public class Customer {
    private @Id Long customerId;
    private String firstName;
    private String lastName;
    private String email;
    @MappedCollection(keyColumn = "CUSTOMER_ID", idColumn = "CUSTOMER_ID")
    private Set&lt;Invoice&gt; students;
}</code></pre>

<p>Аннотацию <code>@MappedCollection</code> можно использовать для ссылочного типа (отношение «один к одному») или для <code>Set</code>, <code>List</code> и <code>Map</code> (отношение «один ко многим»).</p>

<p>Во всех этих типах элемент value аннотации используется для предоставления имени для столбца внешнего ключа, ссылающегося на столбец идентификатора в другой таблице.</p>

<p>В данном примере <code>CUSTOMER_ID</code> ссылается из таблицы <code>Invoice</code>.</p>

<pre><code>// Invoice.java

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import org.springframework.data.annotation.Id;

import java.util.Date;

@Data
@Builder
@AllArgsConstructor
public class Invoice {
    private @Id Long invoiceId;
    private Date invoiceDate;
}</code></pre>

<pre><code>// CustomerRepository.java
import org.springframework.data.repository.CrudRepository;

public interface CustomerRepository extends CrudRepository&lt;Customer, Long&gt; {
}</code></pre>

<p>В <code>Spring Boot JDBC</code> имеется готовый интерфейс с <code>CRUD</code> методами - <code>CrudRepository</code>. Расширив его, мы получаем доступ к методам:</p>

<pre><code>public interface CrudRepository&lt;T, ID&gt; extends Repository&lt;T, ID&gt; {
    &lt;S extends T&gt; S save(S entity);

    &lt;S extends T&gt; Iterable&lt;S&gt; saveAll(Iterable&lt;S&gt; entities);

    Optional&lt;T&gt; findById(ID id);

    boolean existsById(ID id);

    Iterable&lt;T&gt; findAll();

    Iterable&lt;T&gt; findAllById(Iterable&lt;ID&gt; ids);

    long count();

    void deleteById(ID id);

    void delete(T entity);

    void deleteAllById(Iterable&lt;? extends ID&gt; ids);

    void deleteAll(Iterable&lt;? extends T&gt; entities);

    void deleteAll();
}</code></pre>

<pre><code>@SpringBootApplication
public class SchoolApplication implements CommandLineRunner {


	@Autowired
	private CustomerRepository customerRepository;

	public static void main(String[] args) {
		SpringApplication.run(SchoolApplication.class, args);
	}

	@Override
	public void run(String... args) throws Exception {
		for (Customer i: customerRepository.findAll()) {
			System.out.println(i.toString());
		}
	}
}</code></pre>

<p>Запустив приложение, мы увидим вывод запроса <code>findAll</code>:</p>

<pre><code>Customer(customerId=1, firstName=Lus, lastName=Gonalves, email=luisg@embraer.com.br, students=[Invoice(invoiceId=1, invoiceDate=2009-01-01 00:00:00.0), Invoice(invoiceId=3, invoiceDate=2009-01-02 00:00:00.0), Invoice(invoiceId=4, invoiceDate=2010-01-02 00:00:00.0)])
Customer(customerId=2, firstName=a, lastName=a, email=luisg@embraer.com.br, students=[Invoice(invoiceId=2, invoiceDate=2010-01-02 00:00:00.0)])
</code></pre>

<p>При расширении методов нашего <code>repository</code> мы можем использовать встроенный в <code>Spring Data</code> метод определения запроса по ключевым словам.</p>

<pre><code>public interface CustomerRepository extends CrudRepository&lt;Customer, Long&gt; {
    List&lt;Customer&gt; findByFirstName(String firstName);

    List&lt;Customer&gt; findByFirstNameOrderByLastName(String firstname);

    Customer findByFirstNameAndLastName(String firstName, String lastName);

    Customer findFirstByLastName(String lastName);

    @Query("SELECT * FROM person WHERE last_name = :lastName")
    List&lt;Customer&gt; findByLastName(String lastName);

    @Modifying
    @Query("update Customer u set u.first_name = :firstName, u.last_name = :lastName where u.customer_id = :customerId")
    void updateCustomerById(String firstName, String lastName, Integer customerId);
}</code></pre>

<p>Со списком ключевых слов можно ознакомиться по данной <a href="https://docs.spring.io/spring-data/jdbc/docs/2.3.0-SNAPSHOT/reference/html/#jdbc.query-methods" rel="nofollow noopener noreferrer">ссылке</a>.</p>

<p>Аннотация <code>@Query</code> позволяет написать собственный <code>sql</code> запрос.</p>