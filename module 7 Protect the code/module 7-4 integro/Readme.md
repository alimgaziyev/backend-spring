<h1>integro</h1>

<p>Улучшите свои навыки интеграционного тестирования.</p>

<h3>Задача</h3>

<p>integro - web-service, который позволяет создавать заказ, оплачивать его и получать чек по заказу.</p>

<p>Сервис работает совместно с <code>postgresql</code> и сторонним веб-сервисом для получения информации о курсе.</p>

<p>Пример использования:</p>

<pre><code># Создание заказа
curl -X POST -H "Content-Type: application/json" localhost:8080/order -d '{"amount": "100.0EUR"}'
# Оплата заказа
curl -X POST -H "Content-Type: application/json" localhost:8080/order/1/payment -d '{"creditCardNumber": "4532756279624064"}'
# Получение чека
curl -X GET -H "Content-Type: application/json" "localhost:8080/order/1/receipt?currency=USD"</code></pre>

<p>Ваша задача - <strong>реализовать функции для интеграционного тестирования ресурсов и сервисов</strong></p>

<p>В файлах <code>*Test*.java</code> имеются все необходимые импорты и <code>TODO</code> которые нужно реализовать.</p>

<p>Для проверки решения вы можете запустить тесты в репозитории:</p>

<pre><code>cd integro
./mvnv clean test
</code></pre>

<h3>С чего начать ?</h3>

<ul>
	<li>Перейдите в директорию с <a href="https://github.com/jusan-singularity/protect-the-code/tree/master/integro" rel="noopener noreferrer nofollow">задачей</a> в своем репозитории</li>
	<li>Откройте файл <code>*Test*.java</code> и выполните задание</li>
	<li>Если у вас недостаточно знаний, ознакомьтесь с <a href="https://github.com/alem-io/track-backend/blob/master/subjects/protect_the_code/STEP-03.md#related-materials-information_source" rel="noopener noreferrer nofollow">ссылками ниже</a></li>
</ul>

<h3>Материалы по теме</h3>

<ul>
	<li><a href="https://www.softwaretestinghelp.com/what-is-integration-testing/" rel="nofollow noopener noreferrer">Integration testing</a></li>
	<li><a href="https://stackoverflow.com/questions/32974845/integration-test-per-layer-is-a-good-practice" rel="nofollow noopener noreferrer">Integration test per layer is a good practice?</a></li>
	<li><a href="https://square.github.io/okhttp/" rel="nofollow noopener noreferrer">OkHTTP WebServer</a></li>
	<li><a href="https://www.arhohuttunen.com/spring-boot-integration-testing/" rel="nofollow noopener noreferrer">Spring Integration Test</a></li>
</ul>