<p>При объявлении <code>bean</code> в конфигурационном файле. <code>Container</code> создает лишь один экземпляр для каждого <code>bean</code>. Подобная реализация носит название <code>Singleton</code>.</p>

<p><code>Scopes</code> определяет область действия объекта <code>bean</code> (<code>singleton</code>, <code>prototype</code> и т.д.). <code>Spring</code> поддерживает пять областей видимости, три из которых доступны только в веб-приложениях. Область по умолчанию — <code>singleton</code>, в то время как область <code>prototype</code> будет создавать новый объект каждый раз, когда мы создаем экземпляр объекта.</p>

<p>Рассмотрим простой пример:</p>

<p>У нас есть два экземпляра класса <code>Client</code>, которые имею указатель на <code>Reminder</code>.</p>

<p>Класс <code>Reminder</code> имеет метод <code>increment</code>, которые увеличивает внутренний счетчик.</p>

<pre><code>// client.java
public class Client {
    private Reminder reminder;
    private String name;

    public Client(String name, Reminder reminder) {
        this.name = name;
        this.reminder = reminder;
    }

    public void action() {
        this.reminder.increment();
        System.out.printf("%s %d\n", this.name, this.reminder.getInt());
    }
}</code></pre>

<pre><code>// reminder.java
public class Reminder {
    private int i;

    Reminder() {
        this.i = 0;
    }

    public void increment() {
        this.i++;
    }

    public int getInt() {
        return this.i;
    }
}</code></pre>

<pre><code>&lt;bean id="reminder" class="com.example.demo.Reminder"&gt;
&lt;/bean&gt;

&lt;bean id="client2" class="com.example.demo.Client"&gt;
    &lt;constructor-arg type="String" value="Client 2"/&gt;
    &lt;constructor-arg ref="reminder" /&gt;
&lt;/bean&gt;

&lt;bean id="client" class="com.example.demo.Client"&gt;
    &lt;constructor-arg type="String" value="Client 1"/&gt;
    &lt;constructor-arg ref="reminder" /&gt;
&lt;/bean&gt;</code></pre>

<p>В данном примере, для каждого экземпляра класса <code>Client</code> использует один экземпляр класса <code>Reminder</code>.</p>

<pre><code>public static void main(String[] args) {
    ApplicationContext context = new ClassPathXmlApplicationContext("props.xml");
    Client client = context.getBean("client", Client.class);
    Client client2 = context.getBean("client2", Client.class);
    client.action();
    client2.action();
}</code></pre>

<p>Запустив данную программу мы получим:</p>

<pre><code>Client 1 1
Client 2 2</code></pre>

<p>Переменная <code>i</code> у экземпляра была вызвана из двух разных клиентов.</p>

<p>Для того, что бы указать создание уникального <code>Reminder</code> для каждого клиента мы можем указать <code>scope=prototype</code>.</p>

<pre><code>&lt;bean id="reminder" class="com.example.demo.Reminder" scope="prototype"&gt;
&lt;/bean&gt;</code></pre>

<p>Запустив данную программу мы получим:</p>

<pre><code>Client 1 1
Client 2 1</code></pre>

<p>Для каждого клиента <code>Container</code> создал уникальный <code>Reminder</code>.</p>

<p>В <code>Spring</code> имеются такие параметры для <code>scope</code>:</p>

<ul>
	<li>request</li>
	<li>session</li>
	<li>application</li>
	<li>websocket</li>
</ul>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes" rel="nofollow noopener noreferrer">Bean Scopes</a></li>
</ul>