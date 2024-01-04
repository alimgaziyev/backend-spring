<p>Познакомившись с теорией давайте перейдем к практике.</p>

<p>Давайте создадим наш первый проект на <a href="https://start.spring.io/" rel="nofollow noopener noreferrer"><code>Spring</code></a>.</p>

<p><img alt="" height="2264" name="image.png" src="https://ucarecdn.com/e5f4c2af-21bb-4bd0-8d9b-3d1ac7e6c387/" width="3680"></p>

<p>Разархивируйте файл и откройте его в <code>IDE</code>.</p>

<p><img alt="" height="2160" name="image.png" src="https://ucarecdn.com/13f57113-d3e7-4cae-bccc-4430f39d81e9/" width="3456"></p>

<p>Изменим наш класс, убрав лишние для этого модуля аннотации:</p>

<pre><code>package com.example.demo;

public class DemoApplication {
	public static void main(String[] args) {
	}
}</code></pre>

<p>Давайте разберем структуру проекта:</p>

<ul>
	<li>src/main - место, где хранится наш код и конфигурации</li>
	<li>src/test - здесь находятся тесты для нашего кода</li>
	<li>.gitignore - указывает какие файлы не должны попадать в git реестр</li>
	<li>mvn - бинарник для сборки проекта</li>
	<li>pom.xml - project object model файл, который описывает зависимости проекта</li>
</ul>

<p>При инициализации проекта мы выбрали сборщик <code>Maven</code>.</p>

<p><code>Maven</code> - это инструмент, используемый для описания того, как собирается программа и ее зависимости.</p>

<p>Maven автоматически загружает все <code>jar</code> зависимости из Интернета. Таким образом, вместо того, чтобы вручную загружать файлы <code>jar</code>, мы можем использовать <code>Maven</code>.</p>

<p>В <code>pom.xml</code> файле находятся зависимости, которые нужны для запуска <code>Spring</code> приложения. <code>xml</code> - это расширяемый язык разметки, он похож на <code>HTML</code> который используется браузерами. <code>xml</code> разметка повсеместно используется в <code>java</code> проектах для конфигурации проекта. Дополнительно про синтаксис можно почитать <a href="https://habr.com/ru/post/524288/" rel="nofollow noopener noreferrer">здесь</a></p>

<p>В данном примере <code>pom.xml</code> выглядит так:</p>

<pre><code>&lt;dependencies&gt;
    &lt;dependency&gt;
        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
        &lt;artifactId&gt;spring-boot-starter&lt;/artifactId&gt;
    &lt;/dependency&gt;

    &lt;dependency&gt;
        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
        &lt;artifactId&gt;spring-boot-starter-test&lt;/artifactId&gt;
        &lt;scope&gt;test&lt;/scope&gt;
    &lt;/dependency&gt;
&lt;/dependencies&gt;</code></pre>

<p>Каждая зависимость имеет свойства, такие как <code>groupId</code>, <code>artifactId</code> и <code>версия</code>. Вы можете найти все зависимости и значения их свойств в репозитории <a href="https://mvnrepository.com/" rel="nofollow noopener noreferrer"><code>Maven</code></a>.</p>

<p>В данных зависимостях есть основные классы для работы со <code>Spring Core</code>.</p>

<p>Теперь, напишем простую программу, где у нас есть <code>Service</code>, который принимает строку при создании и сохраняет ее у себя как переменную и класс <code>Client</code>, который будет хранить ссылку на <code>Service</code> и выводить значения из <code>Service</code>.</p>

<pre>// client.java
package com.example.demo;
</pre>

<pre><code>public class Client {
    Service service;

    Client(Service service) {
        this.service = service;
    }

    public void showMsg() {
        System.out.println(this.service.getMsg());
    }
}</code></pre>

<pre><code>// service.java
package com.example.demo;

public class Service {
    private String message;

    Service(String message) {
        this.message = message;
    }

    public String getMsg() {
        return this.message;
    }
}</code></pre>

<p>Теперь создадим файл c конфигурациями, где укажем значения для создания объектов:</p>

<blockquote>Примечание: В данном модуле мы будем рассматривать настройку через <code>xml</code> файл, в дальнейших модулях мы будем использовать только аннотации.</blockquote>

<pre><code>&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd"&gt;

    &lt;bean id="service" class="com.example.demo.Service"&gt;
        &lt;constructor-arg type="String" value="Hello World"/&gt;
    &lt;/bean&gt;

    &lt;bean id="client" class="com.example.demo.Client"&gt;
        &lt;constructor-arg ref="service"/&gt;
    &lt;/bean&gt;

&lt;/beans&gt;</code></pre>

<p>Тэг <code>beans</code> состоит из всех ваших классов, которые будут использованы во время запуска программы.</p>

<p>Тэг <code>bean</code> создается для каждого класса - <code>id</code> - является названием класса, <code>class</code> - это путь к классу.</p>

<p>В данном примере, для создания объекта <code>Service</code> нам необходимо передать строку в конструктор. Мы его указываем через <code>constructor-arg</code>.</p>

<p>Для класса <code>client</code> мы передаем созданный нами <code>bean service</code>.</p>

<p>Перейдем к <code>main</code>. Нам нужно создать <code>ApplicationContext</code>. Это интерфейс, через который мы будем обращаться к контейнеру за нашими объектами.</p>

<pre><code>public static void main(String[] args) {
    ApplicationContext context = new ClassPathXmlApplicationContext("props.xml");
    Client client = (Client) context.getBean("client");
    client.showMsg();
}</code></pre>

<p>Существует несколько способов создания <code>ApplicationContext</code>:</p>

<ul>
	<li>ClassPathXmlApplicationContext - получает xml файл с конфигурацией из <code>classpath</code> проекта.</li>
	<li>FileSystemXmlApplicationContext - указывается полный путь к конфигурационному файлу.</li>
	<li>FileSystemXmlApplicationContext - загружает конфигурационный файл по <code>url</code>.</li>
</ul>

<p>Для получения <code>bean</code> мы используем метод <code>getBean</code> указывая <code>id</code> объекта.</p>

<p>Дополнительный материал для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-basics" rel="nofollow noopener noreferrer">Spring Container</a></li>
</ul>