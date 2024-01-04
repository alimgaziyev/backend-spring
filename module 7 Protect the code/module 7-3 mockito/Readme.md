<h1>mockito</h1>

<p>Улучшите свои навыки Unit-тестирования.</p>

<h3>Задача</h3>

<p>mockito - простой web-сервис. Принимает запросы на <code>/isAFlower</code> и проверяет отправленный текст на соответствие цветку, по ресурсу <code>/isABigFlower</code> возвращает <code>true</code> или <code>false</code>. При <code>POST</code> запросе по ресурсу <code>/message</code> отправляет сообщение.</p>

<p>Ваша задачи - <strong>реализовать функции для unit тестирования ресурсов и сервисов</strong></p>

<p>В файлах <code>*Test*.java</code> имеются все необходимые импорты и <code>TODO</code> которые нужно реализовать.</p>

<p>Для проверки решения вы можете запустить тесты в репозитории:</p>

<pre><code>cd mockito
./mvnv clean test
</code></pre>

<h3>С чего начать ?</h3>

<ul>
	<li>Перейдите в директорию с <a href="https://github.com/jusan-singularity/protect-the-code/tree/master/mockito" rel="noopener noreferrer nofollow">задачей</a> в своем репозитории</li>
	<li>Откройте файл <code>*Test*.java</code> и выполните задание</li>
	<li>Если у вас недостаточно знаний, ознакомьтесь с <a href="https://github.com/alem-io/track-backend/blob/master/subjects/protect_the_code/STEP-02.md#related-materials-information_source" rel="noopener noreferrer nofollow">ссылками ниже</a></li>
</ul>

<h3>Материалы по теме</h3>

<ul>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/test/web/servlet/MockMvc.html" rel="nofollow noopener noreferrer">MockMVC</a></li>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/context/WebApplicationContext.html" rel="nofollow noopener noreferrer">WebApplicationContext</a></li>
	<li><a href="https://junit.org/junit5/docs/current/user-guide/" rel="nofollow noopener noreferrer">junit user guide</a></li>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/test/web/servlet/result/MockMvcResultMatchers.html" rel="nofollow noopener noreferrer">MockMVCResultMatchers</a></li>
	<li><a href="https://habr.com/ru/post/527330/" rel="nofollow noopener noreferrer">Улучшение Spring Mock-MVC тестов</a></li>
	<li><a href="https://habr.com/ru/post/444982/" rel="nofollow noopener noreferrer">Mockito How To</a></li>
	<li><a href="https://site.mockito.org/" rel="nofollow noopener noreferrer">Mockito official website</a></li>
	<li><a href="https://quick-adviser.com/how-do-you-mock-beans-in-spring/" rel="nofollow noopener noreferrer">How To Mock</a></li>
</ul>