<h1>Lombok</h1>

<p>Мы привыкли писать сотни классов с параметрами и реализацией для них <code>конструкторов</code>, <code>геттеров</code> и <code>сеттеров</code>.</p>

<p>Lombok - это библиотека, состоящая из комплекта аннотаций (<code>@Getter</code>, <code>@Setter</code> и т.д.), позволяющая в разы уменьшить написание <code>boilerplate</code> кода.</p>

<p>Для подключения библиотеки, необходимо добавить его в <code>pom.xml</code>:</p>

<pre><code>&lt;dependency&gt;
    &lt;groupId&gt;org.projectlombok&lt;/groupId&gt;
    &lt;artifactId&gt;lombok&lt;/artifactId&gt;
    &lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;
</code></pre>

<p>Давайте рассмотрим пример:</p>

<p>У нас есть класс <code>Ticket</code> который выглядит так:</p>

<pre><code>public class Ticket {
    private long id;
    private String message;

    public Ticket(long id, String message) {
        this.id = id;
        this.message = message;
    }

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}</code></pre>

<p>Много одинакового кода, который передает лишь одну информацией - в ней есть значения <code>id</code> и <code>message</code>:</p>

<p>С использованием аннотаций из <code>lombok</code> наш код сократится и станет более читабельным:</p>

<pre><code>@Setter
@Getter
@AllArgsConstructor
public class Ticket {
    private long id;
    private String message;
}</code></pre>

<p>При создании класса мы увидим подсказывающие методы, которые были сгенерированы:</p>

<p><img alt="" height="132" name="image.png" src="https://ucarecdn.com/ab797c23-9942-423a-a452-8b616732f1c4/" width="572"></p>

<p><code>@Setter</code> - создает методы для установления данных. <code>@Getter</code> - генерирует методы для получения данных.</p>

<p>Данные аннотации могут быть указаны и для одной переменной:</p>

<pre><code>@AllArgsConstructor
public class Ticket {
    @Getter private long id;
    @Setter @Getter private String message;
}</code></pre>

<p><code>@AllArgsConstructor</code> - создает конструктор со всеми полями класса.</p>

<p>Мы можем использовать аннотацию <code>@Data</code>, которая сгенерирует все дефолтные методы для класса:</p>

<pre><code>@Data
public class Ticket {
    private final long id;
    private final String message;
}</code></pre>

<p><img alt="" height="172" name="image.png" src="https://ucarecdn.com/f5d73f88-5948-4b74-8fa9-3ce44c42dfdd/" width="555"></p>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://projectlombok.org/features/all" rel="nofollow noopener noreferrer">Аннотации lombok</a></li>
</ul>