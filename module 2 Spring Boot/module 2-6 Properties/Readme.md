<h1>Properties</h1>

<p>При работе с проектом у нас могут быть статичные данные, например логин и пароль от базы данных или порт для запуска сервиса. В Spring Boot есть удобный способ хранения данных - <code>Properties</code>.</p>

<p>В <a href="https://start.spring.io/" rel="nofollow noopener noreferrer">Spring Init</a> имеется файл <code>src/main/resources/application.properties</code>. Данный файл автоматически считывается <code>Spring</code> и доступен в приложения. Проще говоря, это переменные окружения для проекта.</p>

<p>При разработке можно создавать и другие <code>properties</code> файлы. Для их добавления в проект используется аннотация <code>@PropertySource("classpath:foo.properties")</code>, который устанавливается под <code>@Configuration</code>.</p>

<p>Давайте создадим простую переменную <code>message=hello</code> и обратимся к ней из программы:</p>

<pre><code>// application.properties
message=hello
</code></pre>

<p>Обращение к таким данным происходит через аннотацию <code>@Value</code></p>

<pre><code>// MySimpleService.java
@Service
class MySimpleService {
    @Value("${myApp.message}")
    private String message;

    void showMessage() {
        System.out.println(this.message);
    }
}</code></pre>

<pre><code>// Application.java
@SpringBootApplication
public class Application {

	public static void main(String[] args) {
		ApplicationContext ctx = SpringApplication.run(Application.class, args);
		MySimpleService mySimpleService = ctx.getBean(MySimpleService.class);
		mySimpleService.showMessage();
	}
}</code></pre>

<p>Запустив, мы увидим наше значение:</p>

<pre><code>hello
</code></pre>

<p>При создании <code>property</code> очень важно уделять внимание иерархии:</p>

<pre><code>order.name=order
order.count=22
product.name=product
</code></pre>

<p>Данный метод позволит разделять логически ваши переменные и избежать коллизий.</p>

<p>Дополнительно можно создавать свои конфигурационные значения в виде классов.</p>

<p>Для этого используется аннотация <code>@ConfigurationProperties</code></p>

<pre><code>@ConfigurationProperties("my.service")
public class MyProperties {

    private boolean enabled;

    private InetAddress remoteAddress;

    private final Security security = new Security();

    // getters / setters...

    public static class Security {

        private String username;

        private String password;

        private List&lt;String&gt; roles = new ArrayList&lt;&gt;(Collections.singleton("USER"));

        // getters / setters...

    }

}</code></pre>

<p>Данный класс создает под собой несколько параметров, доступных программе:</p>

<ul>
	<li>my.service.enabled.</li>
	<li>my.service.remote-address;</li>
	<li>my.service.security.username;</li>
	<li>my.service.security.password;</li>
	<li>my.service.security.roles, список строк, где при пустом значении устанавливается <code>USER</code>.</li>
</ul>

<p>Объявляются данные параметры <code>application.properties</code> файле.</p>

<p>Данный класс не сканируется автоматически, поэтому его нужно использовать c <code>@SpringBootApplication</code></p>

<pre><code>@SpringBootApplication
@EnableConfigurationProperties(MyProperties.class)
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}</code></pre>

<p>В <code>Spring Boot</code> имеется большое число встроенных значений, к которым можно получить доступ из программы. Также их можно перезаписать.</p>

<p>В случае загрузки кода в репозиторий важно удалить чувствительные данные из <code>properties</code> файла.</p>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html" rel="nofollow noopener noreferrer">Встроенные параметры</a></li>
	<li><a href="https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.external-config" rel="nofollow noopener noreferrer">Документация по properties</a></li>
</ul>