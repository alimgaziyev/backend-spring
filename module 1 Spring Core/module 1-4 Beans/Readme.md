<p>Как мы уже знаем, что каждый класс в <code>java</code> может интерпретироваться как <code>bean</code>.</p>

<p><code>Container</code>, считывая конфигурационный файл создает <code>bean</code>.</p>

<p><code>bean</code> имеет параметры, которые описывают ряд функционала при работе, которые использует <code>container</code>:</p>

<ul>
	<li>class - указывает путь к файлу для загрузки класса;</li>
	<li>name, id - является идентификатором для дальнейшего обращения к <code>bean</code> из программы;</li>
	<li>scope - определяет область для <code>bean</code>;</li>
	<li>аргументы при инициализации;</li>
	<li>lazy - объявляет инициализацию только при использовании <code>bean</code> в коде;</li>
	<li>вызываемый метод при инициализации;</li>
	<li>вызываемый метод деструктор;</li>
</ul>

<p>Аргументы могут быть использованы через <code>constructor</code> или <code>set</code> методы.</p>

<p><code>set</code> метод:</p>

<pre><code>public class Client {
    Service service;

    public void setService(Service service) {
        this.service = service;
    }

    public void showMsg() {
        System.out.println(this.service.getMsg());
    }
}</code></pre>

<pre><code>&lt;bean id="service" class="com.example.demo.Service"&gt;
    &lt;constructor-arg type="String" value="Hello World"/&gt;
&lt;/bean&gt;

&lt;bean id="client" class="com.example.demo.Client"&gt;
    &lt;property name="service"&gt;
        &lt;ref bean="service"/&gt;
    &lt;/property&gt;
&lt;/bean&gt;</code></pre>

<p>Кроме примитивных типов данных, можно отправлять данные типа <code>Collection</code> - list | set | map:</p>

<pre><code>&lt;property name="someList"&gt;
    &lt;list&gt;
        &lt;value&gt;a list element followed by a reference&lt;/value&gt;
        &lt;ref bean="myDataSource" /&gt;
    &lt;/list&gt;
&lt;/property&gt;
&lt;!-- results in a setSomeMap(java.util.Map) call --&gt;
&lt;property name="someMap"&gt;
    &lt;map&gt;
        &lt;entry key="an entry" value="just some string"/&gt;
        &lt;entry key="a ref" value-ref="myDataSource"/&gt;
    &lt;/map&gt;
&lt;/property&gt;
&lt;!-- results in a setSomeSet(java.util.Set) call --&gt;
&lt;property name="someSet"&gt;
    &lt;set&gt;
        &lt;value&gt;just some string&lt;/value&gt;
        &lt;ref bean="myDataSource" /&gt;
    &lt;/set&gt;
&lt;/property&gt;</code></pre>

<h3>lazy</h3>

<p>При конфигурации <code>bean</code> со значением lazy-init="true" не будет произведена инициализация. Она будет вызвана только после явного вызова данного <code>bean</code> из кода.</p>

<h3>depends-on</h3>

<p>При использовании параметра <code>depends-on</code> можно указать зависимость от инициализации другого <code>bean</code></p>

<pre><code>&lt;bean id="UserDao" class="UserDao" depends-on="Database"/&gt;</code></pre>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-definition" rel="nofollow noopener noreferrer">Spring Beans</a></li>
</ul>