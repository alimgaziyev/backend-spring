<h1>Logging</h1>

<p>Логгирование - важный компонент, позволяющий определять историю работы программы.</p>

<p>Он может использоваться для вывода важной информации, которая может быть использована разработчиком для выявления причины ошибок.</p>

<p>Простой пример логов генерит <code>nginx</code>. <code>nginx</code> выводит информацию о каждом запросе, прошедшем через него. Так, администратор в случае проблем сможет обратиться к логам и увидеть, какой запрос не работает и по какому адресу был сделан запрос.</p>

<p>При создании компонента логгирования очень важно учесть несколько важных элементов:</p>

<ul>
	<li>
	<p>Дата: информация всегда должна сопровождаться датой.</p>
	</li>
	<li>
	<p>Приоритет информации:</p>

	<p>Существует несколько уровней логов:</p>

	<ol>
		<li>TRACE;</li>
		<li>DEBUG;</li>
		<li>INFO;</li>
		<li>WARN;</li>
		<li>ERROR.</li>
	</ol>
	</li>
	<li>
	<p>Сообщение: полезная и описывающая часть лога.</p>
	</li>
</ul>

<p>В <code>Spring Boot</code> есть встроенный logger, который выводит всю информацию при запуске проекта. Дополнительно, вы можете расширить его, а так же изменить его конфигурацию в <code>property</code>:</p>

<pre><code>import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

@Service
class MySimpleService {

    private Logger logger = LoggerFactory.getLogger(MySimpleService.class);
    @Value("${myApp.message}")
    private String message;

    void showMessage() {
        logger.info(this.message);
    }
}</code></pre>

<p>Данный <code>logger</code> выведет в консоли:</p>

<pre><code>2022-04-01 23:33:36.703  INFO 23593 --- [           main] com.example.springboot.MySimpleService   : hello
</code></pre>

<p>Если мы хотим видеть логи только уровня <code>WARN</code> и выше, мы можем указать данное значение в <code>property</code>:</p>

<pre><code>logging.level.root=WARN
</code></pre>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.logging" rel="nofollow noopener noreferrer">Документация по логгированию</a></li>
</ul>