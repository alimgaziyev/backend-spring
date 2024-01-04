<h2>Конфигурация контейнера с использованием Java аннотаций</h2>

<p>Проект <code>Spring</code> развивается, вместе с этим улучшается функционал инициализации проекта. В 3 версии <code>Spring</code> появляется возможность инициализации проекта с использование <code>Java</code> аннотаций, который считываются при запуске программы <code>IoC</code> контейнером.</p>

<p>В прошлом модуле мы рассмотрели основные особенности конфигурации через <code>XML</code>. Теперь мы сравним это с конфигурацией через аннотации.</p>

<p>Если при запуске проекта программа считывала <code>xml</code> файл, то теперь это классы c аннотацией <code>@Configuration</code> в которых находятся методы инициализации классов.</p>

<pre><code>@Configuration
public class ApplicationConfig {
}</code></pre>

<p>Данная аннотация эквивалентна тегу <code>&lt;beans&gt;</code>.</p>

<p>Все необходимые <code>bean</code> помещаются в данный класс для дальнейшего использования:</p>

<pre><code>@Configuration
public class AppConfig {
    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl();
    }
}</code></pre>

<p>В <code>xml</code> формате данная конструкция выглядела бы так:</p>

<pre><code>&lt;beans&gt;
    &lt;bean name="transferService" class="com.acme.TransferServiceImpl"/&gt;
&lt;/beans&gt;</code></pre>

<p>Если мы хотим, что бы <code>class</code> <code>A</code> имел указатель на класс <code>B</code> то мы можем указать данные значения внутри метода инициализации:</p>

<pre><code>@Configuration
public class AppConfig {
    @Bean
    public Foo foo() {
        return new Foo(bar());
    }

    @Bean
    public Bar bar() {
        return new Bar();
    }
}</code></pre>

<p>Также мы можем использовать аннотацию <code>@Autowired</code> которая попробует сама установить необходимую для нас зависимость:</p>

<p>Давайте рассмотрим данный пример:</p>

<p>У нас есть интерфейсы <code>Service</code> и <code>Client</code>. Задача простая: <code>service</code> имеет метод <code>get</code> который возвращает строку, а <code>client</code> вызывает метод <code>get</code> у сервиса и выводить результат в консоль:</p>

<pre><code>public interface Service {
	String get();
}

public interface Client {
	void execute();
}

public class ServiceImpl implements Service {

}

public class ClientImpl implements Client {

    @Autowired
    private Service service;

    @Override
    public void execute() {
        System.out.println(this.service.get());
    }
}

public class ServiceImpl implements Service {
    private String message;

    public ServiceImpl(String message) {
        this.message = message;
    }

    @Override
    public String get() {
        return this.message;
    }
}</code></pre>

<p>Тогда наш файл конфигурации будет выглядеть так:</p>

<pre><code>@Configuration
public class AppConfig {
    @Bean
    public Client client() {
        return new ClientImpl();
    }

    @Bean
    public Service service() {
        return new ServiceImpl("hello");
    }
}</code></pre>

<p>В <code>main</code> функции инициализация всех <code>beans</code> осуществляется через <code>AnnotationConfigApplicationContext</code>:</p>

<pre><code>public class Application  {
	public static void main(String[] args) {
		ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
		Client client = ctx.getBean(Client.class);
		client.execute();
	}
}</code></pre>

<p>Для указания <code>scope</code> используется аннотация <code>@Scope</code>:</p>

<pre><code>@Scope(value="prototype")</code></pre>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-java" rel="nofollow noopener noreferrer">Java Annotations</a></li>
</ul>