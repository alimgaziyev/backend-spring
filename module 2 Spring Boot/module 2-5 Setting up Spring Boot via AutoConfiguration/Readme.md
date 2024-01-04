<h1>Spring Boot AutoConfiguration</h1>

<p>Разработчики <code>Spring</code> решили разработать набор утилит, которые автоматизируют настройку приложения. Данный проект называется <code>Spring Boot</code>.</p>

<p><code>Spring Boot</code> — это решение <code>Spring</code>, основанное на соглашениях, а не на настройке, позволяющее создавать автономные приложения production уровня на основе Spring, которые можно «просто запустить». Он создавался с учетом мнения команды <code>Spring</code> о наилучшей конфигурации и использовании платформы <code>Spring</code> и сторонних библиотек, поэтому вы можете начать работу с минимальными усилиями. Большинству приложений Spring Boot требуется очень небольшая конфигурация <code>Spring</code>.</p>

<p>Ключевые особенности:</p>

<ul>
	<li>Создание автономных приложений Spring;</li>
	<li>Встроенный <code>Tomcat</code> или <code>Jetty</code> напрямую (будет рассмотрен в модуле <code>Endpoint</code>);</li>
	<li>Встроенный <code>pom.xml</code> чтобы упростить настройку Maven/Gradle;</li>
	<li>Автоматическая настройка Spring, когда это возможно (@ComponentScan);</li>
	<li>Готовые к работе функции, такие как метрики, проверки работоспособности.</li>
</ul>

<p>При скачивании <code>boilerplate</code> проекта с <a href="https://start.spring.io/" rel="nofollow noopener noreferrer">сайта для инициализации</a> в корневой директории хранится запускаемый класс:</p>

<pre><code>package com.example.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}</code></pre>

<p><code>Spring Boot</code> поддерживает новую аннотацию <code>@SpringBootApplication</code>, которая эквивалентна использованию <code>@Configuration</code>, <code>@EnableAutoConfiguration</code> и <code>@ComponentScan</code> с их атрибутами по умолчанию.</p>

<p>Таким образом, вам просто нужно создать класс, аннотированный с помощью <code>@SpringBootApplication</code>, а <code>Spring Boot</code> включит автоматическую настройку и просканирует ваши ресурсы в текущем пакете:</p>

<p>@EnableAutoConfiguration</p>

<p>Spring Boot состоит из четырех основных компонентов:</p>

<p><img alt="" height="283" name="image.png" src="https://ucarecdn.com/18bb64eb-b323-4ec2-987e-d569090871e6/" width="450"></p>

<ol>
	<li>Spring Boot Starter - компонент, собирающий все зависимости, указанные в <code>pom.xml</code> в один общий <code>jar</code>, к которому будет доступ у вашего проекта (Web, JDBC, ORM); Компонент подключается в <code>pom.xml</code></li>
</ol>

<pre><code>&lt;parent&gt;
	&lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
	&lt;artifactId&gt;spring-boot-starter-parent&lt;/artifactId&gt;
	&lt;version&gt;2.6.6&lt;/version&gt;
&lt;/parent&gt;
</code></pre>

<p>Все необходимые зависимости указываются в <code>pom.xml</code> файле внутри тэга <code>&lt;dependencies&gt;</code>.</p>

<ol>
	<li>AutoConfiguration - компонент, позволяет автоматически анализировать код и связывать между собой бины в вашем проекте. В <code>Spring Boot</code> имеется аннотация <code>SpringBootApplication</code>, которая эквивалентна использованию <code>@Configuration</code>, <code>@EnableAutoConfiguration</code> и <code>@ComponentScan</code> с их атрибутами по умолчанию:</li>
</ol>

<pre><code>@Target(value=TYPE)
@Retention(value=RUNTIME)
@Documented
@Inherited
@Configuration
@EnableAutoConfiguration
@ComponentScan
public @interface SpringBootApplication</code></pre>

<p>Мы рассмотрели аннотации <code>@Configuration</code> и <code>@ComponentScan</code>. <code>@EnableAutoConfiguration</code> - вызывает инициализацию классов у зависимостей проекта.</p>

<ol>
	<li>
	<p>CLI - компонент, утилита, запускаемая в командной строке. Более подробно с возможностями, можно ознакомиться <a href="https://docs.spring.io/spring-boot/docs/current/reference/html/cli.html" rel="nofollow noopener noreferrer">здесь</a>. В данном курсе, нам будет достаточно maven плагина.</p>
	</li>
	<li>
	<p>Actuator - компонент, отвечающий за предоставление данных запущенного приложения - состояние, метрики, дамп. Запускается при добавления зависимости:</p>
	</li>
</ol>

<pre><code>&lt;dependency&gt;
    &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
    &lt;artifactId&gt;spring-boot-starter-actuator&lt;/artifactId&gt;
&lt;/dependency&gt;
</code></pre>

<p>Данный компонент предоставляет возможность создания собственных метрик и просмотра их в вебе.</p>

<p>Более подробно с 4 компонентом мы познакомимся в модуле <code>Endpoints</code>.</p>

<p>Теперь давайте запустим <code>Spring Boot</code> приложение.</p>

<p>В случае, если у вас бесплатная версия <code>Intelij IDEA</code>, то вам потребуется настроить конфигурацию для запуска проекта:</p>

<p>Выберите <code>Add Configuration</code>:</p>

<p><img alt="" height="36" name="image.png" src="https://ucarecdn.com/b585762c-8df1-4e8e-91dc-3fe9c4762444/" width="389"></p>

<p>Выберите <code>Maven</code>:</p>

<p><img alt="" height="675" name="image.png" src="https://ucarecdn.com/7483d109-b8f3-40b4-a577-3858d4b69bac/" width="1045"></p>

<p>В поле <code>Run</code> добавьте <code>spring-boot:run</code>:</p>

<p><img alt="" height="669" name="image.png" src="https://ucarecdn.com/1750cf0d-1abb-4126-8ff7-3238464feae7/" width="1040"></p>

<p>Запустите проект:</p>

<p><img alt="" height="44" name="image.png" src="https://ucarecdn.com/6b92204e-0ef3-4e77-abe7-6c455e13518e/" width="493"></p>

<p>В <code>enterprise</code> версии рядом с <code>main</code> функцией должен быть логотип <code>Spring</code>.</p>

<p>С примером написания и запуска программы можно ознакомиться в данном <a href="https://www.youtube.com/watch?v=cPH5AiqLQTo" rel="nofollow noopener noreferrer">видео</a></p>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.spring-application" rel="nofollow noopener noreferrer">Spring Application</a></li>
	<li><a href="https://www.baeldung.com/spring-boot-actuators" rel="nofollow noopener noreferrer">Spring Boot Actuators</a></li>
	<li><a href="https://docs.spring.io/spring-boot/docs/2.6.6/maven-plugin/reference/htmlsingle/#?.?" rel="nofollow noopener noreferrer">Spring Boot Maven plugin</a></li>
</ul>