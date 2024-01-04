<h1>@ComponentScan</h1>

<p>ComponentScan позволяет находить и автоматически встраивать классы в <code>IoC</code> контейнер <code>Spring</code>.</p>

<p>Существует несколько типов аннотаций, которые могут быть автоматически считаны:</p>

<ul>
	<li><code>@Component</code> - данная аннотация указывает, что класс является компонентом и управляется <code>Spring</code> (создание);</li>
	<li><code>@Repository</code> - подтип <code>Component</code>, аннотация, используемая для классов, реализующих взаимодействие с базой данных;</li>
	<li><code>@Controller</code> - подтип <code>Component</code>, данная аннотация используется <code>Spring</code> для обработки запросов (будет рассмотрен в следующих модулях);</li>
	<li><code>@Service</code> - подтип <code>Component</code>, устанавливается для классов, реализующих бизнес логику;</li>
</ul>

<p> </p>

<p><img alt="" height="174" name="image.png" src="https://ucarecdn.com/8ffb72de-7f94-4a7c-a2d9-bbd0aefb7b32/" width="400"></p>

<p>Давайте рассмотрим пример использования данных аннотаций:</p>

<p>Нам нужно создать сервис для добавления заявок.</p>

<p>Объявим класс с заявкой:</p>

<pre><code>// Ticket.java
public class Ticket {
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

<p>Объявим интерфейс для репозитория:</p>

<pre><code>// TicketRepository.java
public interface TicketRepository {
    long createTicket(String message);
}</code></pre>

<p>И создадим для него реализацию в памяти:</p>

<pre><code>// MemoryTicketRepository.java
@Repository
public class MemoryTicketRepository implements TicketRepository {
    private List&lt;Ticket&gt; tickets;
    private long id;

    public MemoryTicketRepository() {
        this.tickets = new ArrayList&lt;&gt;();
        this.id = 0;
    }

    @Override
    public long createTicket(String message) {
        this.id++;
        this.tickets.add(new Ticket(this.id, message));
        return this.id;
    }
}</code></pre>

<p>Создадим сервис выполняющий создание заявки:</p>

<pre><code>// RegisterTicket.java

@Service
public class RegisterTicket {
    private final TicketRepository ticketRepository;

    public RegisterTicket(TicketRepository ticketRepository) {
        this.ticketRepository = ticketRepository;
    }

    public void execute(String message) {
        System.out.println("Creating ticket");
        long ticketID = this.ticketRepository.createTicket(message);
        System.out.printf("Your ticket id: %d\n", ticketID);
    }
}</code></pre>

<p>И наш финальный <code>main</code>:</p>

<pre><code>// Application.java
@ComponentScan
public class Application {

	public static void main(String[] args) {
		ApplicationContext ctx = new AnnotationConfigApplicationContext(Application.class);
		RegisterTicket registerTicket = ctx.getBean(RegisterTicket.class);
		registerTicket.execute("My printer is broken");
	}
}</code></pre>

<p>Как мы видим из примера, <code>Spring</code> сам связал необходимые для нас классы. <code>RegisterTicket</code> имел в зависимостях интерфейс <code>TicketRepository</code>, мы обернули <code>MemoryTicketRepository</code> аннотацией <code>Repository</code> и <code>Spring</code> контейнер связала контейнеры за нас.</p>

<h2>@Component vs @Bean</h2>

<p>Мы рассмотрели несколько способов конфигурации, которые предоставляет <code>Spring</code> теперь давайте сравним между собой <code>Component</code> и <code>Bean</code>:</p>

<p>В общих чертах, <code>Component</code> это тот же <code>Bean</code>, мы можем использовать аннотацию <code>scope</code> для <code>Component</code> и другие.</p>

<pre><code>@Component
@Scope(value="prototype")
class ServiceCache() {
    // ...
}</code></pre>

<p>Однако, главным отличием является то, что аннотация <code>@Bean</code> используется только для методов, давая информацию <code>Spring</code> о том, как инициализировать класс, когда как <code>@Component</code> сканируется автоматически.</p>

<p>Различия подходов:</p>

<ul>
	<li><code>@Component</code> считываются автоматически, когда <code>Bean</code> нужно декларировать в конфигурации:</li>
</ul>

<pre><code>@Configuration
public class AppConfig {
   @Bean
   public TicketRepository ticketRepository() {
       return new MemoryTicketRepository();
   }

   @Bean
   public RegisterTicket registerTicket() {
       return new RegisterTicket(ticketRepository());
   }
}</code></pre>

<ul>
	<li><code>@Component</code> используется со всеми классами, которыми должен управлять <code>Spring</code>. Когда <code>Spring</code> видит класс с <code>@Component</code>, <code>Spring</code> определяет этот класс как кандидата для создания <code>bean</code>.</li>
</ul>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ComponentScan.html" rel="nofollow noopener noreferrer">ComponentScan Reference</a></li>
</ul>